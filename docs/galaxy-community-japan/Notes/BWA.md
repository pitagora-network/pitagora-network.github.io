----------------------

### Tool

-   bwa_wrappers (1.2.3)

### Tool Dependency

-   bwa (0.5.9)

インストール方法
----------------

### パッケージのインストール

パッケージ「gcc」と「zlib-devel」をインストールしていないと、bwaの実行ファイルが作成されないので、

各パッケージをインストールします。

    $ sudo yum list gcc zlib-devel
    $ sudo yum install gcc zlib-devel

### ツールのインストール

下記の場所にツールを格納します。

-   カテゴリー : “NGS: Mapping”
-   レポジトリ : “bwa_wrappers”

### BWA Indexの入手(hg19)

ピタゴラギャラクシーのトップページ（192.168.56.10/galaxy）にある \[hg19_bwa_0.5.x\] をクリックします。

### インデックス・ファイルの表示設定

インデックス・ファイルの場所を設定します。（.locファイルは **タブ区切り** で記載します）

    $ vi /home/galaxy/galaxy-dist/tool-data/bwa_index.loc

    hg19    hg19    hg19    /media/igenome/hg19_bwa_0.5.x/genome.fa

実行
----

入力のFASTQファイルのデータ形式は「fastqillumina」にする必要があります。

番外 BWA Index の作成方法
-------------------------

以下のリンク先より 「chromFa.tar.gz」を取ってきます。

<http://hgdownload.cse.ucsc.edu/downloads.html#human>

     # su - galaxy
     $ cd /home/galaxy/tool_data
     $ mkdir -p hg19/raw/chromFa
     $ cd hg19/raw/chromFa
     $ wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/chromFa.tar.gz
     $ tar zxvf chromFa.tar.gz
     $ cat chr[1-9].fa > chr_cat1.fa
     $ cat chr1?.fa > chr_cat2.fa
     $ cat chr2?.fa > chr_cat3.fa
     $ cat chrX.fa chrY.fa chrM.fa > chr_cat4.fa
     $ cat chr_cat?.fa > hg19.fa
     $ mv hg19.fa ./..