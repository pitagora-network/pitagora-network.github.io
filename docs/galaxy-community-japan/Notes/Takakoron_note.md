--------------------------

--1. Dockerを作ってみる --

**(1). 環境構築：VM 0.3.1 Beta [1](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.3.1-Beta.ova) を使用。
** でも、こちらを使う前に、VM 0.2.8を使ったり、模索しています。
Mac上にCore OSなどなどをインストールして試したりしてみたけれど、VM 0.3.1 Beta 上に環境は揃っているので、こちらを使用。
**(2). Ryotas先生の「ツールのコンテナ化（Docker）の手順」に沿ってやってみる。
** VM 0.3.1 Betaができる前だったので、VM 0.2.8でやってみた。
まず、VMをVirtualBoxで立ち上げて、ターミナルで ssh galaxy@192.168.56.10 にログインして開発開始。
この辺りは、藤澤さんの作業ログ[2](https://www.evernote.com/l/AAGfn4yqLfJEgIT-nT1itT_jTYhXvFYHQD4) を参考にさせて頂いた。
最初VM 0.2.8を使っていて、次にVM 0.3.1 Beta に変えて、galaxyを実行しようとしたら、
左側のメニューパネルでツールをクリックすると、
Tool request failed
\[history_id=None\] Failed to retrieve history. History unavailable. Please specify a valid history id.
とエラーが出た。これは、galaxy へのログインに失敗していることに起因するものであった。
ログイン時、特にエラーメッセージが出なかったので気付けなかった。
なにかgalaxy の挙動がおかしければ、Sagari のCookie を削除し、再びログインし直すといい。
**(3). inutano先生にご教授頂きながら、自分のPerl スクリプトをdocker化
** (2)のhello galaxy の例では、CentOSのdockerをコールしていたけど、今回は、perlを使いたいので
Docker hub [3](https://hub.docker.com) で 'perl' と検索するとperlが使えるDocker が既に用意されている。
そのDockerで自分のperl スクリプトが動くかどうかの動作確認は、Description の実行コマンドに従って実行してみる。
今回は、perl オフィシャルの Docker を使う。
sudo docker run -it --rm --name my-running-script -v “$PWD”:/data -w /usr/src/myapp perl:5.20 perl -d /data/kick_detect_snps.pl mpileup /data/Koshihikari.vcf 0 0 both 0 /data/result.vcf
Web上に記載されている、このperlのdockerファイルが、
----
FROM perl:5.20
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
CMD \[ “perl”, “./your-daemon-or-script.pl” \]
-----
と記載されていたので、kick_detect_snp.pl が格納されているディレクトリで、この docker コマンドを実行すれば、docker 上の領域 /usr/src/myapp にコピーされて使用できるのかと思ったけど、ダメだった。
docker の -v オプションで、docker の空間上にマウントする領域を明示的に記載する必要がある。
perl -d オプションでデバックで確認することもできる。
今回使用した perl:5.20 は、docker hub に既にあるので、自分でDocker を作る必要がないのかと思ったけど、perl プログラムの設置するディレクトリと、Input / Output ファイルのディレクトリとを 分けたいので、Web 上に記載されている、perl のdocker ファイルをコピーして、作成する。 Docker ファイルと自分のperl プログラムを同じディレクトリに置いて、Docker build & run 。

sudo docker run -it -v “$HOME/docker/data”:/data takakoron/detect_snp perl -d /usr/src/myapp/kick_detect_snps.pl mpileup /data/Koshihikari.vcf 0 0 both 0 /data/result.vcf
perl プログラムは、フルパスで指定しないと動作しないことがあるらしい。。。
この辺りは、inutano 先生にかなり助けて頂いたので、心より感謝申し上げます。
-- 2. Docker と perl プログラムをDocker hub に登録 --

手順は、Ryotas 先生の「ツールのコンテナ化（Docker）の手順」を参照。
-- 3. galaxy で Docker 化したツールを実行 -- 手順は、Ryotas 先生の「ツールのコンテナ化（Docker）の手順」を参照。
入出力ファイルの指定がうまくいくか心配だったが、通常のxml のまま、perl スクリプトへのパラメータの指定は
変更することなく正常動作した。

------------------------------------------------------------------------

xml のコマンドタグは、こんな感じ。パラメータの指定方法は変更していないけど、perlプログラムのパスを追加。/usr/src/myapp/
<command interpreter="perl">
/usr/src/myapp/kick_detect_snps.pl $how.how_specify

`       #if $how.how_specify == `“`pileup`”`:`
`            $how.pileup`
`            $how.consensus`
`            $how.snp`
`            $how.map`
`            $how.coverage`
`            $how.het_hom_both`
`            $output1`
`        #else:`
`            $how.mpileup`
`            $how.dp`
`            $how.mq`
`            $how.het_hom_both`
`            $how.gq`
`            $output1`
`        #end if`
`    `</command>

----

Tool Shed への更新 by Planemo
-----------------------------

hg push でのTool Shed への更新はできなくなったらしい。BioStars [4](https://biostar.usegalaxy.org/p/16872/#16883)
そのあたりの機能は、もろもろ Planemo へ移行。
参照ページ [5](https://media.readthedocs.org/pdf/planemo/latest/planemo.pdf) [6](http://planemo.readthedocs.org/en/latest/commands.html#shed-update-command)
''' --1. Planemo のインストール -- '''

VM 環境上に Planemo をインストールした。 参照ページには、linuxbrew でもインストールできると書いてあるけど、glibc のインストールでエラーが出て、インストールできなかったので、pip にてインストール。

**(1). vertualenv のインストール**

curl -kL <https://raw.github.com/pypa/pip/master/contrib/get-pip.py> | python

pip install virtualenv virtualenvwrapper

**(2). Planemo のインストール**

virtualenv .venv; . .venv/bin/activate

pip install planemo

適宜、.bash_profile でパスの指定をしてください。

'''--2. Tool Shed に登録したいデータの準備 -- '''

**(1). 作業ディレクトリの作成**

ディレクトリ名は、Tool Shed に登録したいレポジトリ名にしておくと便利。 mkdir snpEff_3_6c

cd snpEff_3_6c

**移行の処理は全てこのディレクトリ内で行うこと！！！**

'''(2). 更新したいgalaxy ファイルを作業ディレクトリにコピー'’'

cp /path/path/snpEff_docker.xml .

cp /path/path/snpEff_genomes.loc ./snpEff_genomes.loc.sample

loc ファイルを xml 内で参照していれば、loc ファイルも同じディレクトリ内に用意していないとエラーになる。

loc ファイルのファイル名は、.loc の後ろに .sample を付ける規則らしい。

プログラムもあれば、同じようにコピーしておく。

''' --3. Tool Shed にリポジトリの作成 -- '''

**(1). .shed.yml ファイルの作成**

この.shed.yml ファイルに情報を参照して、リポジトリの作成やプログラムの更新をするので、まずは作成する。

planemo shed_init --name ' snpEff_3_6c' --owner 'takakoron' --description ' snpEff_3_6c' --category 'Variant Analysis'

オプションの詳細は、planemo shed_init --help で確認してください。

**(2). リポジトリの作成**

planemo shed_create --shed_email xx@xx.xx.xx --shed_password xxxxxxx --shed_target toolshed

<span style="color:red">Exception: Attempting to create a repository with configured owner \[takakoron\] that does not matche API user \[1-204-10-1002\]. </span>

Tool Shed のアカウント API key もしくは、e-mail & password で shed_create コマンドでログイン。

なぜか、必ず、このエラーが出るので、エラーが出たら、

vi .shed.yml で、owner を '1-204-10-1002' に変更して、再度、上記 Planemo shed_ create コマンドを実行。

さらに、<span style="color:red"> Repository \[import_mpileup\] does not exist in the targeted Tool Shed. </span> と言われることがあるが、web ブラウザでレポジトリを確認して出来ていればOK。

オーナーを '1-204-10-1002' にして実行しても、web ブラウザで見るとオーナーは takakoron になっているので、'.shed.yml' の owner を 'takakoron' に戻す。(戻さないと後続処理でエラーになります.... 涙)

''' --4. Tool Shed にプログラムを更新 -- '''

planemo shed_update --shed_target toolshed --shed_email xxxx@xxx.xx.xx --shed_password xxxxxxxx

<span style="color:green">Repository import_mpileup updated successfully.</span>