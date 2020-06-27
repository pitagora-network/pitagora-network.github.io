
-   立ち上げ方は２通り
    -   $HOME/GalaxyCommunityVM/Pitagora-Galaxy-0.2.3.ova をダブルクリック
        -   （なぜかこちらがうまくいかなくなった）
    -   $HOME/Application/VirtualBox.appをダブルクリック
-   起動したらブラウザで <http://192.168.56.10:8080/>
-   adminとしてログイン（しないとヒストリーが見れない）
    -   admin@pitagora-galaxy.org
    -   galaxy

##### ターミナルから触る

-   ssh -l galaxy 192.168.56.10
-   galaxy

##### ログを見る

-   tail -f /home/galaxy/galaxy-dist/paster.log

##### 再起動

-   sudo service galaxy restart
    -   終わったかどうかは tail -f ~/galaxy-dist/paster.log で確認

##### bowtie とか samtools とかのパスを通す

-   vi .bash_profile
    -   export PATH=$HOME/tool_dependency/bowtie/1.0.0/iuc/package_bowtie_1_0_0/9fcaaedbbfd6/bin:$PATH
    -   export PATH=$HOME/tool_dependency/samtools/0.1.19/devteam/package_samtools_0_1_19/95d2c4aefb5f/bin/:$PATH
-   . .bash_profile
-   which bowtie
-   bowtie --version
-   sudo service galaxy restart
    -   終わったかどうかは tail -f ~/galaxy-dist/paster.log で確認
-   終わったら <http://192.168.56.10:8080/> をリロード

##### 新しいツールを入れる

-   admin をクリックして Search and browse tool sheds

##### ヒストリーからワークフローを作る

-   まず、失敗した計算など、不要なヒストリーを削る。
-   <http://192.168.56.10:8080> 画面右上の「ヒストリー」の歯車をクリック
-   プルダウンメニューから「Extract Workflow」を選択
-   出て来た画面の「Create Workflow」をクリック
-   「Workflow “Workflow constructed from history 'Unnamed history'” created from current history. You can edit or run the workflow.」というメッセージ中の「edit」をクリック。
-   要らないノードを消しつつAuto Re-layoutしたりして整える。→ Save → Close
-   Rename で名前をつける
-   Download or export

##### 既存のワークフローをインポートする

-   <http://192.168.56.10:8080> 上部の「Shared Data」→「Published workflow」→「Workflow import」

##### Dockerがらみのコマンド（未整理）

sudo docker build

sudo docker images

sudo docker run

sudo docker login

sudo docker push

sudo docker images

\[galaxy@localhost ~\]$ sudo docker run -it maskot1977/bridger:0.1.0 bash

\[galaxy@localhost test\]$ sudo docker run -v \`pwd\`:/data -w /data maskot1977/bridger:0.1.0 /src/Bridger_r2014-12-01/Bridger.pl --seqType fq --right /src/Bridger_r2014-12-01/sample_test/reads.right.fq.gz --left /src/Bridger_r2014-12-01/sample_test/reads.left.fq.gz --CPU 8

\[galaxy@localhost test\]$ sudo docker push maskot1977/bridger:0.1.0

\[galaxy@localhost test\]$ sudo docker tag a8d2ce8c481a maskot/bridger:0.1.0

\[galaxy@localhost test\]$ sudo docker push maskot/bridger:0.1.0

-   Dockerイメージとコンテナの削除方法 <http://qiita.com/tifa2chan/items/e9aa408244687a63a0ae>
