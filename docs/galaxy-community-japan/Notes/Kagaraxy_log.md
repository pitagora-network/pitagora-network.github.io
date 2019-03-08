---
title: Kagaraxy log
permalink: /Kagaraxy_log/
---

[Amazon Web Services](/Amazon_Web_Services "wikilink") &gt;&gt;

インスタンスの詳細
------------------

Galaxyのインストール
--------------------

### ホスト名の設定

コンソールに表示されるホスト名をkaga01に変更します

`$ su -`
`$ hostname kaga01`

### OSユーザーの作成

ec2-userユーザーからrootユーザーにスイッチし、rootユーザーのパスワードを設定する

`$ sudo su -`
`$ passwd`

galaxyユーザーを作成する

`$ su -`
`# useradd galaxy`
`# passwd galaxy`
`# exit`

この後の作業はgalaxyユーザーを使用する

`$ su - galaxy`

### Pythonのインストール

Galaxyは起動時にPythonのバージョンを見て対応するEggをダウンロードする

既存のPythonの確認

まずバンドルされているPythonのバージョンを確認する

`$ python -version`
`Python 2.6.8`

※上記のようにAWSのAmazonLinuxではPython2.6がバンドルされているためGalaxyがインストールできるが、Galaxyで使用するツールがPythonにライブラリを追加する事があるため、Galaxyで使うPythonをバンドルされているものとは別に作成しておく。

必要なパッケージのインストール

`sudo yum install gcc zlib-devel bzip2-devel openssl-devel sqlite-devel`

Python-2.7.5をソースからコンパイル

`cd`
`mkdir galaxy-python`
`cd galaxy-python`
`wget `[`http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tgz`](http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tgz)
`tar xvzf Python-2.7.5.tgz`
`mv Python-2.7.5 Python-2.7.5_src`
`mkdir Python-2.7.5`
`cd Python-2.7.5_src`
`./configure --prefix=/home/galaxy/galaxy-python/Python-2.7.5`
`make`
`make install`

PATH環境変数の設定と確認

`$ cd`
`$ vi .bash_profile`
`PATH=$HOME/galaxy-python/Python-2.7.5/bin:$PATH`
`$ . .bash_profile`
`$ which python`

~/galaxy-python/Python-2.7.5/bin/python

`$ python --version`
`Python 2.7.5`

### Mercurial

`$ su -`
`# yum -y install mercurial`
`$ exit`

### Galaxyのインストール


インストールし試しに起動(起動したときにuniverse_wsgi.iniが作成される)

`$ hg clone `[`https://bitbucket.org/galaxy/galaxy-dist/`](https://bitbucket.org/galaxy/galaxy-dist/)
`$ cd galaxy-dist`
`$ ./run.sh --daemon`


　ログを確認(正常に起動されているか)

`$ tail -f paster.log`
`   .`
`   .`
`serving on 0.0.0.0:8080 view at `[`http://127.0.0.1:8080`](http://127.0.0.1:8080)


Galaxyを最新版にバージョンアップ

`$ hg update stable`


どのIPからでもアクセスできるようにする

`$ vi universe_wsgi.ini`
`host = 0.0.0.0`

再起動用スクリプトの作成

`$ cd ~/galaxy-dist`
`$ vi restart.sh`
`./run.sh --stop`
`./run.sh --daemon`
`tail -f paster.log | grep -v `“`DEBUG`”
`$ chmod +x restart.sh`

### 接続確認


ブラウザを開き、「192.168.8.○○:8080」で接続する。

Toolのインストール
------------------

### Tool sheds 事前準備

-   ディレクトリ作成(shed_tools , tool_dependency , tool_data , tool_sheds_conf.xml)

`$ cd /home/galaxy/`
`$ mkdir shed_tools tool_dependency tool_data`

-   ファイル編集(universe_wsgi.ini , shed_tool_conf.xml , migrated_tools_conf.xml)

`$ cd galaxy-dist`
`$ vi universe_wsgi.ini`
`tool_config_file = tool_conf.xml,shed_tool_conf.xml`
`tool_dependency_dir = ../tool_dependency`
`$ vi shed_tool_conf.xml`
<toolbox tool_path="../shed_tools">
`$ vi migrated_tools_conf.xml`
<toolbox tool_path="../shed_tools">
`$ vi tool_sheds_conf.xml`
<tool_shed name="RCAST tool shed" url="http://54.238.209.66:9009/"/>

### tool shedsからのツールインストール方法


1. 画面上部のメニュー「Admin」を選択

2. 左タブ\[Tool sheds\]の「Search and browse tool sheds」を選択

3. 「Galaxy main tool shed」を選択　※1

4. インストールしたいツールを検索/選択します

5. ツールのプルダウン「Preview and install」を選択

6. インストール内容を確認し、「Install to Galaxy」を選択

7. ツールのsectionを設定し、「Install」を選択　※2

8. Statusに緑色の“Installed”が表示されると完了　※3

　※1 インストールするツールによって選択先を変更してください。

　※2 ツールによって「Handle tool dependencies」項目がsection項目の上に出てきます。その場合チェックを入れ「Install」を選択してください。

　　　 事前設定が出来ていない場合、「Handle tool dependencies」項目にチェックを入れることが出来ません。再度、事前設定を確認してください。

　※3 手順2の左タブ「Server」の「Manage installed tool shed repositories」からインストールされたツールの確認が出来ます。

### UNIX Tools (unix_tools)

-   Tool shedを用いて『unix tools』をインストールする。


　　上記手順3で、「RCAST tool shed」を選択し、新規section「UNIX Tools」に『unix_tools』をインストールします。

-   ファイル編集(sed_wrapper.sh 34行目)


tool shedからunix toolsインストール後は、以下のフォルダに格納される。

　/home/galaxy/shed_tools/54.238.52.160/repos/yamanaka/unix_tools/3373c698f092/unix_tools

`$ vi sed_wrapper.sh`
`#$GALAXY_TOOLS/sed/sed-4.2.1/sed -r --sandbox `“`$@`”` `“`$PROG`”` `“`$INPUT`”` > `“`$OUTPUT`”
`sed -r `“`$@`”` `“`$PROG`”` `“`$INPUT`”` > `“`$OUTPUT`”


※ sed_wrapper.sh の“--sandbox”オプションを削除しないと、sedのジョブがエラーになる※

-   sedのダウンロード

`$ cd galaxy-tools/`
`$ mkdir sed `
`$ cd sed`
`$ wget `[`ftp://anonymous@ftp.gnu.org/gnu/sed/sed-4.2.1.tar.gz`](ftp://anonymous@ftp.gnu.org/gnu/sed/sed-4.2.1.tar.gz)
`$ tar zxvf sed-4.2.1.tar.gz`
`$ cd sed-4.2.1`
`$ ./configure --prefix=/home/galaxy/galaxy-tools/sed/sed-4.2.1`
`$ make`
`$ make install `

-   ファイル編集(.bash_profile)

`$ cd /home/galaxy`
`$ vi .bash_profile`
`export PATH`
`# Galaxy Tools (on Solexa512G)`
`export GALAXY_TOOLS=/home/galaxy/galaxy-tools`
`export PATH=$GALAXY_TOOLS/sed/sed-4.2.1/bin:$PATH`
`$ . .bash_profile`
`$ which sed`

### BWA (bwa_wrappers)

-   パッケージのインストール

gccとzlib-develをインストールしていないと、bwaの実行ファイルが作成されない

`$ sudo yum install gcc47 zlib-devel`

-   Tool shed “Galaxy main tool shed” &gt; カテゴリー “NGS: Mapping” &gt; レポジトリ “bwa_wrappers”

<!-- -->

-   bwa_indexの作成(hg19)


ダウンロードURL:<http://hgdownload.cse.ucsc.edu/downloads.html#human>

`# su - galaxy`
`$ cd /home/galaxy/tool_data`
`$ mkdir -p hg19/raw/chromFa`
`$ cd hg19/raw/chromFa`
`$ wget `[`http://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz`](http://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz)
`$ tar zxvf chromFa.tar.gz`
`$ cat chr[1-9].fa > chr_cat1.fa`
`$ cat chr1?.fa > chr_cat2.fa`
`$ cat chr2?.fa > chr_cat3.fa`
`$ cat chrX.fa chrY.fa chrM.fa > chr_cat4.fa`
`$ cat chr_cat?.fa > hg19.fa`
`$ mv hg19.fa ./..`


※bwaとsamtoolsインストール後、追加手順あり。

-   ファイル編集(bwa_index.loc)

`$cd /home/galaxy/galaxy-dist/tool-data`
`$vi bwa_index.loc `
`hg19    hg19    hg19    /home/galaxy/tool_data/hg19/raw/hg19.fa`


<font color="red">※スペース区切りでなくタブ区切り！※</font>

### Interaction PETs (chiapet)

-   Tool shedを用いて『chiapet』をインストールする。


　　手順3には、「RCAST tool shed」を選択し、新規section「Others」に『chiapet』をインストールします。

-   Java Runtime Environment(JRE)インストール

`$ mkdir -p /home/galaxy/galaxy-tools/java`
`$ cd /home/galaxy/galaxy-tools/java`


<http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html> より、

JRE「jre-7u45-linux-x64.tar」をローカルにダウンロードし、上記ディレクトリに転送、展開する。

`$ tar zxvf jre-7u45-linux-x64.tar`

-   java のバージョンを確認

`$ cd /home/galaxy/galaxy-tools/java/jre1.7.0_45/bin`
`$ ./java -version`

-   ファイル編集(.bash_profile)

`$ cd /home/galaxy`
`$ vi .bash_profile`
`export PATH=$GALAXY_TOOLS/java/jre1.7.0_45/bin:$PATH`
`$ . .bash_profile`
`$ which java`

### Count Reads (count_reads)

-   Tool shedを用いて『count_reads』をインストールする。


　　手順3には、「RCAST tool shed」を選択し、section「NGS: SAM Tools」に『count_reads』をインストールします。

-   samtoolsをインストール

`$ su `
`# yum install ncurses-devel`
`# exit`
`$ cd ~/galaxy-tools/`
`$ mkdir samtools`
`$ cd samtools`
`$ wget `[`http://downloads.sourceforge.net/project/samtools/samtools/0.1.19/samtools-0.1.19.tar.bz2`](http://downloads.sourceforge.net/project/samtools/samtools/0.1.19/samtools-0.1.19.tar.bz2)
`$ tar lxvf samtools-0.1.19.tar.bz2`
`$ cd samtools-0.1.19`
`$ make`

-   ファイル編集(.bash_profile)

`$ cd /home/galaxy`
`$ vi .bash_profile`
`export PATH=$GALAXY_TOOLS/samtools/samtools-0.1.19:$PATH`
`$ . .bash_profile`
`$ which samtools`

### Export to SAM (count_reads)

-   Tool shedを用いて『export2sam』をインストールする。


　　手順3には、「RCAST tool shed」を選択し、section「Others」に『export2sam』をインストールします。

-   export_to_sam.sh

実行ファイル：illumina_export2sam_gz.pl、illumina_export2sam.plの格納先をフルパスで指定する。

`$ vi export_to_sam.sh`
`if [ $FLG_G ] ; then`
`  CMD1=`“`/home/galaxy/shed_tools/54.238.209.66/repos/hashimoto/export2sam/109ab8482dcd/export2sam/illumina_export2sam_gz.pl`` ``$ARGS`”
`else`
`  CMD1="/home/galaxy/shed_tools/54.238.209.66/repos/hashimoto/export2sam/109ab8482dcd/export2sam/illumina_export2sam.pl $ARGS`

### Cistrome (cistrome)

-   Tool shedを用いて『cistorome』をインストールする。


　　手順3には、「RCAST tool shed」を選択し、section「Cistrome」に『cistorome』をインストールします。

Cistome-Apps のインストール

`cd galaxy-tools`
`hg clone `[`https://username@bitbucket.org/cistrome/cistrome-applications-harvard`](https://username@bitbucket.org/cistrome/cistrome-applications-harvard)` cistrome-apps`

各ツールをPythonにインストール

`cd galaxy-tools/cistrome-apps/cistrome-extra-apps`
`python setup.py install`
`cd galaxy-tools/cistrome-apps/published-packages/CEAS`
`python setup.py install`
`cd galaxy-tools/cistrome-apps/published-packages/MACS`
`python setup.py install`

### SQL Tools (sql_tools)

-   Tool shedを用いて『sql_tools』をインストールする。

　　Galaxy main tool shed を選択し、新規section「SQL Tools」に『sql_tools』をインストールする。

-   Python コンパイル時に sqlite-devel が必要

` $ yum install sqlite-devel`

### Bowtie

Galaxy main tool shed &gt; レポジトリ: bowtie_wrappers (tool dependency を使用する)

bowtieコマンドはtophatから呼ばれるためPATHに追加しておく。

`$ vi .bash_profile`
`PATH=/home/galaxy/tool_dependency/bowtie/0.12.7/devteam/bowtie_wrappers/e1c59c194b7b/bin:$PATH;`
`$ . .bash_profile`
`$ cd galaxy-dist`
`$ ./restart.sh`

### Tophat2

このツールはGalaxyのディストリビューションに含まれている

-   binary

`cd ~/galaxy-tools`
`wget `[`http://tophat.cbcb.umd.edu/downloads/tophat-2.0.8b.Linux_x86_64.tar.gz`](http://tophat.cbcb.umd.edu/downloads/tophat-2.0.8b.Linux_x86_64.tar.gz)
`tar xvzf tophat-2.0.8b.Linux_x86_64.tar.gz`
`rm tophat-2.0.8b.Linux_x86_64.tar.gz`

新しいバージョンは

`wget `[`http://tophat.cbcb.umd.edu/downloads/tophat-2.0.10.Linux_x86_64.tar.gz`](http://tophat.cbcb.umd.edu/downloads/tophat-2.0.10.Linux_x86_64.tar.gz)
`tar xvzf tophat-2.0.10.Linux_x86_64.tar.gz`
`rm tophat-2.0.10.Linux_x86_64.tar.gz`

-   .bashrc

`export PATH=$GALAXY_TOOLS/tophat-2.0.8b.Linux_x86_64:$PATH`

### Tophat for Illumina

このツールはGalaxyのディストリビューションに含まれている

Tophat2を実行している？これはPATHの問題？

-   binary

`cd ~/galaxy-tools`
`wget `[`http://tophat.cbcb.umd.edu/downloads/tophat-1.4.1.Linux_x86_64.tar.gz`](http://tophat.cbcb.umd.edu/downloads/tophat-1.4.1.Linux_x86_64.tar.gz)
`tar xvzf tophat-1.4.1.Linux_x86_64.tar.gz`
`rm tophat-1.4.1.Linux_x86_64.tar.gz`

-   .bashrc

`export PATH=$GALAXY_TOOLS/tophat-1.4.1.Linux_x86_64:$PATH`

-   bowtie_indices.loc

`$cd /home/galaxy/galaxy-dist/tool-data`
`$vi bowtie_indices.loc`
`hg19   hg19    hg19    /media/indexes/igenome/Homo_sapiens/UCSC/hg19/Sequence/BowtieIndex/genome`

-   bowtie2_indices.loc

`$cd /home/galaxy/galaxy-dist/tool-data`
`$vi bowtie2_indices.loc`
`hg19   hg19           hg19        /media/indexes/igenome/Homo_sapiens/UCSC/hg19/Sequence/Bowtie2Index/genome`


<font color="red">※スペース区切りでなくタブ区切り！※</font>

### Cufflinks (cufflinks)

Galaxy main tool shed &gt; レポジトリ: cufflinks (tool dependency を使用する)

### Cuffmerge (cufflmerge)

Galaxy main tool shed &gt; レポジトリ: cuffmerge (tool dependency を使用する)

### Cuffdiff (cuffdiff)

Galaxy main tool shed &gt; レポジトリ: cuffdiff (tool dependency を使用する)

### FastQC

このツールはGalaxyのディストリビューションに含まれている

`cd /home/galaxy/galaxy-dist_vm23/tool-data/shared/jars/`
`wget `[`http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.10.1.zip`](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.10.1.zip)
`unzip fastqc_v0.10.1.zip`
`rm fastqc_v0.10.1.zip`
`cd FastQC`
`chmod 755 fastqc`

### SAM-to-BAM

Galaxy main tool shed &gt; レポジトリ: sam_to_bam - Revision 0:30fdbaccb96b (tool dependency を使用する)

-   tool_data_table_conf.xml

``` text

    <table name="fasta_indexes" comment_char="#">
        <columns>value, dbkey, name, path</columns>
        <file path="tool-data/fasta_indexes.loc" />
    </table>
```

-   fasta_indexes.loc

`hg19    hg19    hg19    /diskarray_3/galaxy/tool-data/igenome/Homo_sapiens/UCSC/hg19/Sequence/WholeGenomeFasta/genome.fa`

Revision “1:93f2e3337a33” は以下の修正が必要

-   sam_to_bam.py (51L)

`#fai_index_file_base = seq_path`

### Sonoda-Fusion

-   easy_install

`mkdir galaxy-python/work`
`cd galaxy-python/work`
`wget `[`http://peak.telecommunity.com/dist/ez_setup.py`](http://peak.telecommunity.com/dist/ez_setup.py)
`python ez_setup.py`
`which easy_install`

-   site-packages

`mkdir ~/galaxy-python/Python-2.7.5/site-packages`
`vi .bash_profile`
`PYTHONPATH=$HOME/galaxy-python/Python-2.7.5/site-packages`
`export PYTHONPATH`

-   cython

`easy_install --install-dir ~/galaxy-python/Python-2.7.5/site-packages -U cython`

-   bedtools

`su -`
`ln -s /usr/libexec/gcc/x86_64-amazon-linux/4.6.3/cc1plus /usr/bin/cc1plus`
`exit`
`easy_install --install-dir ~/galaxy-python/Python-2.7.5/site-packages -f `[`http://pythonhosted.org/pybedtools/`](http://pythonhosted.org/pybedtools/)` pybedtools`

-   biopython

`easy_install --install-dir ~/galaxy-python/Python-2.7.5/site-packages -f `[`http://biopython.org/DIST/`](http://biopython.org/DIST/)` biopython`

-   pysam

`cd galaxy-python/work`
`wget `[`http://code.google.com/p/pysam/`](http://code.google.com/p/pysam/)` (0.6系の最新版)`
`python setup.py build`
`python setup.py install`

ワークフローのインストール
--------------------------

-   移行の方法


テスト用Galaxyに接続し、『\[Verification\]MiSeq shRNA』をインポート


(1)192.168.8.23にインターネットブラウザで接続する

(2)新規Galaxyにて『ワークフロー』⇒『Upload or import workflow』を選択する。

(3)新規Galaxyにてローカルにダウンロードした、“Mi-seq shRNA”を選択しインポートする。

### Mi-seq shRNA

-   必要なツール: UNIX Tools,Gunzip,
-   移行するワークフロー: \[Verification\]Fujimoto:MiSeq shRNA
-   入力データ
    -   (1) shRNALibrary
        -   テスト用Galaxy『\[Verification\] Fujimoto: Mi-Seq shRNA』の『shRNALibrary』をサーバーにコピー（Importツールを使う）
    -   (2) 実験データ
        -   テスト用Galaxy『\[Verification\] Fujimoto: Mi-Seq shRNA』の『Import』をサーバーにコピー（Importツールを使う）
-   ワークフローの実行
    -   ワークフロー: Fujimoto:Mi-seq

### Okabe:Sonication

-   必要なツール: bwa, Interaction PET
-   必要なパッケージ：zlib-devel
-   移行するワークフロー:\[Verification\]Okabe:Sonication
-   入力データ
    -   (1) 実験データ
        -   テスト用Galaxy『\[Verification\] Okabe:Sonication』の『L-ChIA-2-22_S3_L001_R1_001.fastq』をサーバーにコピー（Importツールを使う）
        -   テスト用Galaxy『\[Verification\] Okabe:Sonication』の『L-ChIA-2-22_S3_L001_R1_002.fastq』をサーバーにコピー（Importツールを使う）
-   ワークフローの実行
    -   ワークフローOkabe:Sonication

### Count Reads

-   必要なツール: Export to SAM,SAM-to-BAM,Count Reads,Sort
-   必要なパッケージ:ncurses-lib,bzip2-devel,libxmlo2-devel
-   移行するワークフロー:\[Verification\]Count Reads
-   入力データ
    -   (1) refGene_hg19_v02_modified_tss
        -   テスト用Galaxy『Count Reads』の『refGene_hg19_v02_modified_tss』
    -   (2)実験データ
        -   テスト用Galaxy『Count Reads』の『Galaxy1-MKN7_WT_K4me3: s_1_sorted.txt.gz』をサーバーにコピー(Importツールを使う）
-   ワークフローの実行
    -   ワークフロー: Count Reads

※データタイプ“eland_export”は初期設定されていないため、以下の記載を追加し設定する。(!-- 真ん中以外はコメントアウト --!)

`$ vi ~/galaxy-dist/datatype_conf.xml`
`<-- RCAST-Lab Datatypes -->　`
<datatype extension="eland_export" type="galaxy.datatypes.tabular:Tabular" display_in_upload="true"/>
`<-- End RCAST-Lab Datatypes-->`

### Cistrome

-   必要なツール: Export to SAM,SAM-to-BAM,Cistrome
-   必要なパッケージ:
-   移行するワークフロー:\[Verification\]Cistrome
-   入力データ
    -   (1)
        -   テスト用Galaxy『Cistrome』の『refGene_hg19_v02_modified_tss』
    -   (2)実験データ
        -   テスト用Galaxy『Cistrome』の『』をサーバーにコピー(Importツールを使う）
-   ワークフローの実行
    -   ワークフロー: Cistrome

### RNA-seq Tutorial

-   必要なツール: Tophat for illumina,Cufflinks,Cuffmerge,Cuffdiff,FastQC
-   必要なパッケージ:
-   移行するワークフロー:\[Verification\] RNA-seq Tutorial3
-   入力データ
    -   (1)iGenomes_UCSC_hg19,_chr19_gene_annotation
        -   テスト用Galaxy『RNA-seq Tutorial 3』の『iGenomes_UCSC_hg19,_chr19_gene_annotation』をサーバーにコピー(Importツールを使う）
    -   (2)実験データ
        -   テスト用Galaxy『RNA-seq Tutorial 3』の『adrenal_1.fastq 』をサーバーにコピー(Importツールを使う）
        -   テスト用Galaxy『RNA-seq Tutorial 3』の『adrenal_2.fastq』をサーバーにコピー(Importツールを使う）
        -   テスト用Galaxy『RNA-seq Tutorial 3』の『brain_1.fastq 』をサーバーにコピー(Importツールを使う）
        -   テスト用Galaxy『RNA-seq Tutorial 3』の『brain_2.fastq』をサーバーにコピー(Importツールを使う）
-   ワークフローの実行
    -   ワークフロー: RNA-seq Tutorial

Zabbixの設定
------------

### Zabbixクライアント

1)対象サーバでrootユーザで以下のコマンドを実行する

`groupadd zabbix`
`useradd -g zabbix zabbix`

2)teraterm で zabbix_agent.tar.gz を転送

3)ファイルを展開する

`cd /`
`tar xvzf /home/ec2-user/zabbix_agent.tar.gz`

4)ログディレクトリの作成

`mkdir /var/log/zabbix`
`chown zabbix:zabbix /var/log/zabbix`

5)プロセスID保存ディレクトリの作成

`mkdir /var/run/zabbix`
`chown zabbix:zabbix /var/run/zabbix`

6)agent実行に必要な、ディレクトリの作成

`mkdir /etc/zabbix/zabbix_agentd.d`

7)コンフィグレーションファイルの修正

`cd /etc/zabbix`
`vi zabbix_agentd.conf`

末行のHostnameを変更する↓
/////////////////////////////////////////////////////////
☆zabbix_agent.conf の内容（抜粋)
PidFile=/var/run/zabbix/zabbix_agentd.pid
LogFile=/var/log/zabbix/zabbix_agentd.log
LogFileSize=10
DebugLevel=2
EnableRemoteCommands=1　　←1になっていることを確認
LogRemoteCommands=1
Server=tmonit01.cloud-koubou.jp
ListenPort=10050
ListenIP=0.0.0.0
Hostname=route-03
/////////////////////////////////////////////////////////
8)自動起動の設定

`chkconfig zabbix-agent on`
`chkconfig zabbix-agent --list`

起動

`/etc/init.d/zabbix-agent start`

9)visudo の設定　　※忘れやすいので注意

`visudo`
`Defaults:zabbix   !requiretty`
`zabbix ALL=(ALL) NOPASSWD: /sbin/shutdown -h now, /sbin/reboot, /etc/init.d/httpd restart`

10)セキュリティグループの解放(クライアント側) 10050 を開ける

### Zabbixホスト

1)セキュリティグループの解放(サーバ側)


10051 を開ける

10050 を開ける

2）Zabbix管理コンソールにログイン <https://zabbix04.cloud-koubou.jp/zabbix/>

3）「設定タブ/ホスト」→「ホストの作成」

4）ホストの設定

-   ホスト名：kaga01
-   表示名：kaga01
-   グループ：Linux servers
-   IPアドレス：192.169.0.111


→まだアプリケーションやアイテムは０

5）登録マクロでクライントをホストに登録 「Linuxホストの設定シート」

-   ホスト名：kaga01
-   fqdn：https://kaga01.cloud-koubou.jp/ping.html
-   アカウント：aws-1120040-01


※３２ビットのExcelでないと実行できません※

→zabbixのホスト管理ページ「kaga01」のアプリケーション、アイテムなどの値が変化する。

6）クラウド工房の監視対象に登録 •ssh接続

-   ホスト：「zabbix03.cloud-koubou.jp/ckbweb」
-   鍵：ckb_sample.pem

`ec2-user mysql -u ckbconsole -p`
`mysql> use ckbconsole`
`mysql> insert into ckbinstance(account,tag_name,zabbix_id,public_ip,dns_name)values(`
`           'aws-1120040-01'    ←対象のクラウド工房`
`           ,'Kaga002'      ←追加する子インスタンス`
`           ,'zabbix04'     ←親のZabbixサーバ`
`           ,null`
`           ,null`

7)Zabbixのアカウントを追加 以下、コマンドをサーバ側にて実施します。対象は、任意に変更してください。
insert into ckbzabbixapi(id,url,user,password)values(

`        'zabbix04'                      　　　　   ←サーバに登録するID`
`        ,'`[`https://zabbix04.cloud-koubou.jp`](https://zabbix04.cloud-koubou.jp)`'            ←アクセス時のURL`
`        ,'admin'                                               ←アクセス時のユーザー名`
`        ,'c1283530ab8a77dfe9c1b7e7a646c23e'    ←アクセス時のパスワード`
`        ) ;`

8)サーバ /etc/hosts の編集 以下を参考に、IPアドレス、アクセス時のURL、サーバに登録したIDを記載します。

`[root@zabbix04 tomcat6]# cat /etc/hosts`
`127.0.0.1   localhost localhost.localdomain`
`192.169.0.70 zabbix04.cloud-koubou.jp zabbix04`

その他
------

### FTPサーバー起動

-   AWSコンソールから security group で 21, 50000-50010 ポートを開放
-   /etc/vsftpd/vsftpd.conf 編集

`$ su -`
`$ yum install vsftpd`
`$ vi /etc/vsftpd/vsftpd.conf`
`(追記)`
`pasv_enable=YES`
`pasv_min_port=50000`
`pasv_max_port=50010`
`pasv_address=54.238.249.174　<-- ここにグローバルIPを記載`
`$ service vxftpd start`

### ファイル転送

-   ftp client インストール

`#su -`
`#yum install ftp`
`#exit`

### Databaseシンボリックリンク作成

database

-   galaxy停止

`$ cd ~/galaxy-dist `
`$ ./run.sh --stop `

-   シンボリックリンク作成

`$ mkdir /media/data/database `
`$ chown galaxy:galaxy /media/data/database `
`$ cp -r database/* /media/data/database/ `
`$ mv database database_bak `
`$ ln -s /media/data/database database `
`$ ./run.sh --daemon `

-   起動、設定を確認後、バックアップを削除

`$ ls -l`
`$ rm database_bak `
`(追記)`
`rwxrwxrwx  1 galaxy galaxy      21 Nov 19 01:03 database -> /media/data/database/`

### EBSストレージマウント

AWSインスタンス作成時に設定した、EBSストレージをマウントする。

-   fstab

`# vi /etc/fstab`
`LABEL=/     /           ext4    defaults,noatime  1   1`
`tmpfs       /dev/shm    tmpfs   defaults        0   0`
`devpts      /dev/pts    devpts  gid=5,mode=620  0   0`
`sysfs       /sys        sysfs   defaults        0   0`
`proc        /proc       proc    defaults        0   0`
`/dev/sdb        /media/ephemeral0       auto    defaults,comment=cloudconfig    0       2`
`/dev/sdc        /media/ephemeral1       auto    defaults,comment=cloudconfig    0       2`
`/dev/sdd        /media/data             ext4    defaults,comment=cloudconfig    0       2`

-   マウント

指定したデバイスにファイル・システムを構築しマウントする。

`# mkdir /media/ephemeral1`
`# mkdir /media/data`
`# mkfs.ext4 /dev/xvdd`
`# mount -a`

-   設定後に確認

`# df -h`
`Filesystem            Size  Used Avail Use% Mounted on`
`/dev/xvda1            7.9G  3.5G  4.4G  45% /`
`tmpfs                 3.7G     0  3.7G   0% /dev/shm`
`/dev/xvdb             414G   13G  381G   4% /media/ephemeral0`
`/dev/xvdc             414G  199M  393G   1% /media/ephemeral1`
`/dev/xvdd              50G  180M   47G   1% /media/data`

### ツールパネルの修正

-   参考 <http://wiki.galaxyproject.org/GalaxyToolPanel>

tool_conf.xmlにラベル追加

`$ vi tool_conf.xml`
<label id="standard_tools" text="Standard Tools" />
`...`

shed_tool_conf.xmlにラベル追加

`$ vi shed_tool_conf.xml`
<label id="custom_tools" text="Custom Tools" />
`...`

integrated_tool_panel.xmlでラベルの位置を変更

`$ ./restart.sh`
`$ vi integrated_tool_panel.xml`
<label id="custom_tools" text="Custom Tools" version="" />
`...`
<label id="standard_tools" text="Standard Tools" version="" />
`...`
`$ ./restart.sh`

### タイトルをGalaxyからKagalaxyに変更

-   タブ表示　database/compiled_templates/base/base_panels.mako.py

`           #__M_writer(unicode(self.title()))`
`           __M_writer(u'Kagalaxy')`

-   タブ表示　database/compiled_templates/base.mako.py

`       #__M_writer(unicode(self.title()))`
`       __M_writer(u'Kagalaxy')`

-   これは不要　database/compiled_templates/webapps/galaxy/base_panels.mako.py

`       #__M_writer(u'">\n        Galaxy\n')`
`       __M_writer(u'">\n        Kagalaxy\n')`

-   タイトル　static/scripts/galaxy.masthead.js (445L)

`'`<span id="brand">` Pitagora-Galaxy ' + brand_text + '`</span>`' +`

再起動

`./restart.sh`

### AWSインスタンスの複製方法


1) AWS上でインスタンスの停止を確認する

　　　　※構成を変更する場合は、アンマウントしてからでないと、system checkエラーになる。
: 2) 停止したインスタンスを選択して「Action」から「create Image」
　　　　→OS領域を拡張したい場合は、この時に「Instance Volume」を変更する(※起動後、拡張コマンド実施が必要)
: 3) 左側タブのAMIsにある、先程作成したAMIを選択して「Launch」
: 4) instancetype を「All instancetype」から選択してNext
: 5) instance Ditails からnetworkを「vpc-78070cla(192.169.0.0/24)」を選択してNext
　　　　→「PraimaryIP」に 192.169.0.111 を入力してNext
: 6) AddStorage は変更なしでNext
　　　※(1)の時点でアンマウントしていないと、既存と同じ構成が表示される。これを変更してしまうと、system checkエラーになる。
: 7) Tag は、Valueに任意の名前を入力してNext
: 8) SecurityGroup では、「create securitygroup」選択してNext
: 9) Review instance Launch で内容を確認して、「Launch」
: 10) keypairは、既存の鍵/新規作成を選択して、チェックボックスにチェックをして「launch instance」

### ストレージの追加・拡張


1) 左側タブのvolumeを選択し、EBSのスナップショットを作成する

2) 作成したスナップショットから「create volume」で拡張したEBSを作成する。

3) 左側タブのvolumeから、作成したものを選択し「Attach volume」を選択

　　　　→インスタンスを選択し、デバイスの部分を指定「/dev/sd?」でアタッチ　※要確認
: 4) 起動後、コンソールより/etc/fstabを編集しEBS情報を追記する
vi /etc/fstab
追加例

`/dev/sdd  /media/data  ext4   defaults,comment=cloudconfig   0   2`
` mkdir /media/sample`
`mount -a`
`df -h`

　　　※※拡張の場合※※
: 5) 「resize2fs」コマンドを実施し、EBSを拡張する。
resize2fs /dev/xvda1　←　df -hの結果のデバイス名

`df -h`