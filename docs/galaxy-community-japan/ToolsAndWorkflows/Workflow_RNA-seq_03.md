
概要
----

この RNA-seq ワークフローは、FASTQ のマッピングしたファイル (SAM/BAM 形式) からリードカウントデータを抽出し、二群比較の遺伝子発現変動解析、エンリッチメント解析、各種グラフ化を行います。STEP 01-06 の順序に解析を進めていくことが可能です。

-   STEP 01: bam2readcount
    -   HP : <http://www.bioconductor.org/packages/release/bioc/html/Rsubread.html>
    -   特徴 : Subread の R パッケージである Rsubread (Bioconductor) を用いて、BAM ファイルからリードカウントデータを抽出します。UCSC mm9 (マウス) と hg19 (ヒト) に対応。
    -   入力ファイル : bam ファイル
    -   出力ファイル : リードカウントデータ (テキストファイル)

<!-- -->

-   STEP 02: upload (入力ファイルのアップロード)
    -   HP : なし
    -   特徴 : Galaxy は zip ファイルを自動的に解凍してしまうので、zip 形式で保存した複数リードカウントデータファイルをそのまま Galaxy のヒストリーに登録するために利用します。複数リードカウントデータファイルのアップロードは、ドラッグアンドドロップのファイルアップロード機能ではなく、こちらをご利用下さい。将来的に利用しやすいよう改善予定です。
    -   入力ファイル : data という名前のフォルダに保存して zip 圧縮したリードカウントデータファイル複数
    -   出力ファイル : なし (ヒストリーに追加)

<!-- -->

-   STEP 03: normalization (リードカウントデータの正規化)
    -   HP : <http://bioconductor.org/packages/release/bioc/html/TCC.html>
    -   特徴 : TCC (Bioconductor) パッケージを用いて、複数リードカウントデータファイルに対して正規化を実施します。今現在利用できる手法は edgeR の TMM 正規化、DESeq2 の正規化です。
    -   入力ファイル : 解析設定ファイル (テキスト形式)、zip 圧縮したリードカウントデータファイル (ヒストリーから選択)
    -   出力ファイル : 正規化データ (テキスト形式)

<!-- -->

-   STEP 04: DEG (発現変動解析)
    -   HP : <http://www.bioconductor.org/packages/release/bioc/html/edgeR.html>, <http://bioconductor.org/packages/release/bioc/html/DESeq2.html>
    -   特徴 : edgeR もしくは DESeq2 を利用して、２群間の発現変動解析を行います。
    -   入力ファイル : 解析設定ファイル、正規化データ (ヒストリーから選択)
    -   出力ファイル : zip 圧縮された発現変動解析結果 (DEG) ファイル (テキスト形式)

<!-- -->

-   STEP 05: filtergenes (条件に合う遺伝子群の選定)
    -   HP : <http://en.wikipedia.org/wiki/False_discovery_rate> (FDR), <http://www.med.osaka-u.ac.jp/pub/kid/clinicaljournalclub1.html> (FDR: Japanese)
    -   特徴 : 二群間の fold-change、DEGA で計算した FDR (False Discovery Rate) の閾値を設定し、条件に見合う遺伝子群を選定します。
    -   入力ファイル : 解析設定ファイル、正規化データ, zip 圧縮した DEG ファイル (ヒストリーから選択)
    -   出力ファイル : 条件にあった遺伝子シンボルリスト (テキスト形式)

<!-- -->

-   STEP 06-1: heatmap (ヒートマップ作成)
    -   HP : <http://www.bioconductor.org/packages/release/bioc/html/genefilter.html> (genefilter), <http://www.pnas.org/content/95/25/14863.full> (Eisen distance PMID: 9843981), <http://cran.r-project.org/web/packages/gplots/index.html> (gplots), <http://en.wikipedia.org/wiki/Heat_map> (Heat map)
    -   特徴 : R パッケージ genefilter (Bioconductor) の genescale 関数によるデータの Z 変換、相関係数をもとにした距離 (Eisen et al, 1998) gplots の heatmap.2 によるヒートマップを描画します。
    -   入力ファイル : 解析設定ファイル、正規化データ (ヒストリーから選択), 遺伝子シンボルリスト (アップロード or ヒストリーから選択)
    -   出力ファイル : ヒートマップ (pdf 形式)

<!-- -->

-   STEP 06-2: boxplot (箱ひげ図作成)
    -   HP : <http://ggplot2.org> (ggplot2), <http://en.wikipedia.org/wiki/Box_plot> (Box-plot)
    -   特徴 : R パッケージ ggplot2 を用いて、指定した遺伝子に対する発現量の箱ひげ図を作成します。
    -   入力ファイル : 解析設定ファイル、正規化データ (ヒストリーから選択), 遺伝子シンボル名 (手入力)
    -   出力ファイル : 箱ひげ図 (pdf 形式)

<!-- -->

-   STEP 06-3: tsplot (時系列データ平均値プロット)
    -   HP : なし
    -   特徴 : R パッケージ ggplot2 を用いて、グループごとの遺伝子発現量の経時データ平均値をプロットします。
    -   入力ファイル : 解析設定ファイル、正規化データ (ヒストリーから選択), 遺伝子シンボルリスト (アップロード or ヒストリーから選択)
    -   出力ファイル : 経時データプロット (pdf 形式)

<!-- -->

-   STEP 06-4: enrichment 解析 (エンリッチメント解析 : 遺伝子機能やパスウェイの探索)
    -   HP : <http://www.bioconductor.org/packages/release/bioc/html/gage.html>
    -   特徴 : GAGE (Generally Applicable Gene-set Enrichment for Pathway Analysis) という R (Bioconductor) パッケージを用いて、発現データ情報を元に２群間で発現状態が異なる遺伝子群が関連している機能 (GO: Gene Ontology) やパスウェイ (KEGG) を提示します。
    -   入力ファイル : 解析設定ファイル、正規化データ (ヒストリーから選択)
    -   出力ファイル : GO/KEGG 探索結果 (テキスト形式)

<!-- -->

-   STEP 06-5: PPI (タンパク質相互作用の探索)
    -   HP : <http://string-db.org>, <http://www.bioconductor.org/packages/release/bioc/html/STRINGdb.html>
    -   特徴 : R (Bioconductor) パッケージ STRINGdb を用いて、STRING database から予測も含めたタンパク質相互作用ネットワークを探索します。
    -   入力ファイル : 解析設定ファイル、正規化データ、zip 圧縮した DEG ファイル (ヒストリーから選択)
    -   出力ファイル : 相互作用ネットワーク図 (pdf 形式)
    -   保留 : AWS Pitagora Garalxy 上で動作確認しましたが、STRINGdb に接続できませんでした。もしかすると local virtualbox 版のみのサポートになるかもしれません。

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/RNAseq_03.html> |width=850 |height=600 |border=0 }}

-   実際のフローはもっと複雑です

[<File:RNA-seq_03_workflow.png>](/File:RNA-seq_03_workflow.png "wikilink")

テスト方法
----------

-   \[データの準備\] Galaxy 付属ツールを使う等して、２グループそれぞれに対して BAM ファイルを準備します。
    -   \[解析設定ファイルの準備\] どのサンプルがどのグループ、時点であるかを記載した解析設定テーブル (table.csv) を準備します (参考: 経時データ版 DL, 通常データ版 DL)。
        -   注意１ : sampleID に入れる名前と、BAM ファイルの名前は一致させて下さい (例 SRR001.bam の場合、SRR001 を sampleID 列に入力)。
        -   注意２ : 経時データの場合、time 列に整数値、unit 列に hour や hr など、時間単位を入力して下さい。経時データではない場合、NA 等と整数値以外の文字を列全部に入力して下さい。
        -   注意３ : replicate のないサンプル、日本語は共に未対応です。
    -   \[リードカウントデータ圧縮ファイルの準備\] リードカウントデータファイルは、data というフォルダを作成後、名前を sampleID_rcsym.txt (SRR001_rcsym.txt) として zip で圧縮して下さい (data.zip)。
        -   \[リードカウントデータの保存形式\] 一列目は遺伝子シンボル名、二列目は発現値としたテキストファイルで準備して下さい (bam2readcount ツールの出力形式)
    -   \[遺伝子シンボルリスト\] heatmap, tsplot で利用する遺伝子シンボルリストは、filtergenes で選定したもの以外に、自分で作成したリストも利用できます。一行目を gene とし、二行目以降に遺伝子シンボル名を１行に１つ入力したテキストファイルを作成して下さい。

<!-- -->

-   テストデータ

[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_03/data.zip`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_03/data.zip)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_03/table.csv`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_03/table.csv)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_03/table_ts.csv`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_03/table_ts.csv)

-   \[パイプライン実行\] \[Workflow\] &gt; \[RNA-seq 03\] &gt; \[run\] をクリックします。
    -   \[エラー\] tool を実行する度、Galaxy のヒストリーに execution log file が出現します。エラーの場合は、どこでエラーが生じたのかがわかるようになっています。統計解析言語 R に関する知識がある方は、エラーを見て可能な場合は対処を試みて下さい。もしくは開発者にファイルを添付してご連絡下さい (善処しますが、対応については保証いたしません。予めご了承下さい。)
