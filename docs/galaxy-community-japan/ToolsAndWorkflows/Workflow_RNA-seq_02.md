
概要
----

このRNA-seqワークフローは、「FastqMcf」を用いてRNA-seqの結果で得られたfastqをトリミングしたあと、「Sailfish」を使用した発現定量を行います。

-   FastqMcf
    -   HP:<https://code.google.com/p/ea-utils/wiki/FastqMcf>
    -   特徴：fastqデータから任意のアダプター/プライマー配列、低クオリティなリードなどを取り除き、発現定量の精度を高めます。
    -   入力ファイル :
        -   adapter/primer File: 取り除きたいアダプター、プライマー配列（fastaファイル）
        -   FASTQ File: 塩基配列とクオリティスコアのテキストファイル（fastqsangerファイル）
            -   サンプル1（フォワード側 / リバース側）
            -   サンプル2（フォワード側 / リバース側）
    -   出力ファイル : logファイル、reads（トリミング済みフォワード側リード）、 meats（トリミング済みリバース側リード）

<!-- -->

-   FastQC
    -   HP:<https://code.google.com/p/ea-utils/wiki/FastqMcf>
    -   特徴：fastqデータから任意のアダプター/プライマー配列、低クオリティなリードなどを取り除き、発現定量の精度を高めます。
    -   入力ファイル : FastqMcfでトリミングしたreads、meats（ワークフローで自動的に選択されます）
    -   出力ファイル : RawDataファイル、Webpage（クオリティチェックの結果レポート、html）

<!-- -->

-   Sailfish
    -   HP：http://www.cs.cmu.edu/~ckingsf/software/sailfish/
    -   文献：Rob Patro, Stephen M. Mount, and Carl Kingsford (2014) Sailfish enables alignment-free isoform quantification from RNA-seq reads using lightweight algorithms. Nature Biotechnology
    -   特徴：RNA-seqのデータから既知RNA-isoformの定量を行うツールです。ハッシュを利用したアルゴリズムを採用し、正確さを失わず既存の定量ツールと比較して大変高速な定量を行うことができます。
    -   入力ファイル :
        -   FastqMcfでトリミングしたreads、meats（ワークフローで自動的に選択されます）
        -   Sailfish インデックスファイル（シーケンスを行った生物種を手動で指定。現在はヒトhg19 or マウスmm10が選択可能）
    -   出力ファイル : すべて.txt形式で以下の4つ
        -   log
        -   reads_count_info
        -   quant（発現定量結果）
        -   quant_bias_corrected.sf（上記quantにバイアス補正を行ったもの）

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/RNAseq_02.html> |width=450 |height=450 |border=0 }}

テスト方法
----------

-   Download References ツールで Sailfish Index UCSC mm10 を入手します。
-   トリミングしたい配列（adapter/primer）をヒストリーに読み込みます。(.fa形式)

[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_02/test_adapter.fa`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_02/test_adapter.fa)

-   FASTQファイルをヒストリーに読み込みます。ファイルフォーマットを確認し、fastqsangar でなければ手動で変更します。

[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_02/SRR616850_div100_1.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_02/SRR616850_div100_1.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_02/SRR616850_div100_2.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_02/SRR616850_div100_2.fastq)

-   \[Workflow\] &gt; \[RNA-seq 02\] &gt; \[run\] をクリックします。
-   手順１で読み込んだ配列と手順２で読み込んだFASTQファイルを、FastqMcfのインプットに指定します。
-   ワークフローを実行します。
