
インストール手順
----------------

### zip/unzip, ghostscript (gs) のインストール (Pitagora 0.2.2)

    $sudo yum -y install zip unzip ghostscript

### R のインストール

参考 [http://wiki.pitagora-galaxy.org/wiki/index.php/ツールインストール手順](http://wiki.pitagora-galaxy.org/wiki/index.php/ツールインストール手順)

`./configure prefix=/home/galaxy/galaxy-tools/R/R-3.1.2`

R のコンパイルに必要なプログラムをインストール

`sudo yum -y install make libX11-devel.* libICE-devel.* libSM-devel.* libdmx-devel.* libx*  libFS* libX* readline-devel gcc-gfortran gcc-c++ texinfo tetex`

ソースをダウンロードし、R 3.1.2 をインストール。

Rをインストールする

<!-- -->

    $ su -
    # yum -y install make libX11-devel.* libICE-devel.* libSM-devel.* libdmx-devel.* libx*  libFS* libX* readline-devel gcc-gfortran gcc-c++ texinfo tetex
    # exit
    $ mkdir ~/galaxy-tools/R
    $ cd ~/galaxy-tools/R
    $ wget http://cran.r-project.org/src/base/R-3/R-3.1.2.tar.gz
    $ tar xvzf R-3.1.2.tar.gz
    $ mv R-3.1.2 R-3.1.2_src
    $ mkdir R-3.1.2
    $ cd R-3.1.2_src
    $ ./configure prefix=/home/galaxy/galaxy-tools/R/R-3.1.2
    $ make
    $ make install
    $ cd $HOME
    $ vi .bash_profile
    export PATH=$PATH:$GALAXY_TOOLS/R/R-3.1.2/bin
    $ . .bash_profile

これで R は動作するようになるが、このままでは R および Bioconductor パッケージのインストールに必要な library がいくつか欠損している。具体的には package “png”, “plyr”, “RCurl” のインストールで失敗する。

`sudo yum -y install libpng-devel curl libcurl libcurl-devel`

を実行して必要なライブラリーを予めインストールする。必要なパッケージをインストールしても Rcpp のインストールに失敗することがある。これに対処するため

`$ R`
`> install.packages(`“`Rcpp`”`,dependencies=TRUE,INSTALL_opts = c('--no-lock'))`

を実行する (参考 : <http://stackoverflow.com/questions/14382209/r-install-packages-returns-failed-to-create-lock-directory>)。

### R/Bioconductor パッケージのインストール

R package のインストール方法は

`$ R`
`> install.packages(c(`“`ggplot2`”`,`“`gridExtra`”`),dependencies=TRUE,repos=`“[`http://cran.ism.ac.jp/`](http://cran.ism.ac.jp/)”`)`

Bioconductor package のインストール方法は

`$ R`
`> source(`“[`http://bioconductor.org/biocLite.R`](http://bioconductor.org/biocLite.R)”`)`
`> biocLite(c(`“`TCC`”`,`“`edgeR`”`,`“`DESeq2`”`),dependencies=TRUE)`

ツールの動作に必要なパッケージは以下の通り。

-   R: argparse, ggplot2, gridExtra, gplots
-   Bioconductor: annotate, Rsubread, org.Mm.eg.db, org.Hs.eg.db, TCC, edgeR, DESeq2, STRINGdb, genefilter, pathview, gage, gageData

`> install.packages(c(`“`argparse`”`,`“`ggplot2`”`,`“`gridExtra`”`,`“`gplots`”`),dependencies=TRUE,repos=`“[`http://cran.ism.ac.jp/`](http://cran.ism.ac.jp/)”`)`
`> biocLite(c(`“`annotate`”`,`“`Rsubread`”`,`“`org.Mm.eg.db`”`,`“`org.Hs.eg.db`”`,`“`TCC`”`,`“`edgeR`”`,`“`DESeq2`”`,`“`STRINGdb`”`,`“`genefilter`”`,`“`pathview`”`,`“`gage`”`,`“`gageData`”`),dependencies=TRUE)`

### ツールの配置

-   (i) exec_bam2readcount
-   (ii) exec_upload
-   (iii) exec_normalization
-   (iv) exec_DEGA
-   (v) exec_filtergenes
-   (vi) exec_enrichment
-   (viI) exec_get_PPI
-   (viiI) exec_heatmapplot
-   (ix) exec_tsplot
-   (x) exec_boxplot

tool_conf.xml に RNAseq-ts ツールとして追加する。

### 実行

-   \[リードカウントデータ取得\] exec_bam2readcount を実行し、bam ファイルから Rsubread を利用して read count を抽出し、Entrez ID を gene symbol に変換する。
-   \[解析ファイルとプロトコルの準備\] リードカウントデータを \[sample-ID の名前\]_rcsym.txt という名前で保存し、data フォルダーに入れて zip し、data.zip を作成する。加えて、解析プロトコル (データのグループ分け、取得時刻など解析に必要な情報) を table.csv にコンマ区切りテキストとして記載する。
-   \[入力データのアップロード\] data.zip, table.csv を入力データとしてアップロードする。
-   \[正規化\] exec_normalization を実施し、TCC パッケージを利用して正規化したリードカウントデータを取得する。
-   \[発現変動解析\] exec_DEGA を実施し、正規化データを元に、edgeR もしくは DESeq2 を利用して発現変動解析を実施する。
-   \[遺伝子群の選定\] exec_filtergenes を利用し、発現値の下限や log fold change の上限・下限値 (cuttoff)、False Discovery Rate (FDR) を設定して、条件に合致する遺伝子群を抽出する。
-   \[エンリッチメント解析\] exec_enrichment (Gage パッケージ) を利用し、Gene Ontology (GO) や KEGG パスウェイ上で有意に観察されるオントロジーやパスウェイを取得する。
-   \[相互作用検索\] タンパク質相互作用や共発現データを利用した STRING database にアクセスし、input した遺伝子の相互作用ネットワーク描画図を取得する。
-   \[可視化(ヒートマップ)\] exec_heatmapplot を利用して、ヒートマップを描画する。
-   \[可視化(時系列プロット)\] exec_tsplot を利用して、遺伝子発現平均値の時系列プロットを描画する。
-   \[可視化(箱ひげ図)\] exec_boxplot を利用して、二群間の遺伝子発現の箱ひげ図を描写する。

### メモ

-   mouse は UCSC mm9 を利用。human は UCSC hg19 を利用。UCSC mm10 や hg18、NCBI/EMBL、他の生物のリファレンスは今後対応予定。

### ToDo

-   Galaxy 0.2.1 上で動作確認
-   各サブツール毎の使用方法解説 (XML に日本語・英語) を記載。
-   zip で固めたファイルを出力するとき、display となって拡張子 (.zip) が付与されない問題を回避。
-   複数同時アクセス時にファイルの重複書き込みによるエラーが予想されるため、ファイルにタイムスタンプをつける等行って重複を避けるようにする。

中岡から山中さんに伝言
----------------------

### exec_DEGA.R 修正

exec_DEGA.R line 14 後、

parser$add_argument(“--output2”, dest=“output2”, default=“execlog.txt”, required=TRUE)

を追加して下さい。また\#\# output files \#\# のすぐ上に

cmd &lt;- paste("cp “,execlog,” “,tmp$output2,sep=”")

system(cmd)

の２行を追加して下さい。

山中メモ
--------

### exec_upload.py 修正

新しいバージョンのGalaxyではupload.pyが修正されていて、以下のエラーが出る。

`NameError: global name 'from_json_string' is not defined`

exec_upload.pyの以下を書き換えればOK

`       #dataset = from_json_string( line )`
`       dataset = loads( line )`

### Tool Shed 化

その結果、とりあえずこのディレクトリが必要

`mkdir galaxy-dist/tools/ske`

### Normalize

savefiles.txt ってなんでしたっけ？

    Error in file(file, "rt") : cannot open the connection
    Calls: read.table -> file
    In addition: Warning message:
    In file(file, "rt") :
      cannot open file '/home/galaxy/galaxy-dist/tools/ske/savefiles.txt': No such file or directory
    Execution halted