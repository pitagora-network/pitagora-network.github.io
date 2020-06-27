
概要
----

このRNA-seqワークフローは、ペアエンドの異なる2つのターゲットを比較します。

転写産物のシークエンス量から遺伝子の発現量を定量化し、配列情報から選択的スプライシングの検出や未知の転写産物を発見する解析手法です。

### 入力ファイル

（入力ファイルの説明とファイル形式を記載します。）

-   FASTQ File: 塩基配列とクオリティスコアのテキストファイル（fastqファイル）
-   アノテーションファイル（gtfファイル）

### 出力ファイル

（最終的に参照対象となる出力ファイルの説明とファイル形式を記載します。）

-   FASTQCのレポート（html）
-   TopHat2のマッピング情報（txt, bed）

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/RNAseq_01.html> |width=900 |height=900 |border=0 }}

テスト方法
----------

（テスト手順を記載します。）

（ダウンロードが必要なファイルはありますか？）

-   Download References ツールで Bowtie2 Index UCSC hg19 を入手します。

（テスト用の入力データのURLです。必要に応じてpitagora-galaxy.orgでホストします。）

-   ファイルをヒストリーにダウンロードするためには、左ペインのツールの右横にあるアイコン \[Download from URL or upload files from disk\] &gt; \[Paste/Fetch data\]を選択し、URLテキストボックスに以下のURLを入力して \[start\] をクリックします。

[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_1.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_1.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_2.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_2.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_1.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_1.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_2.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_2.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/gene_annotation.gtf`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/gene_annotation.gtf)

（ファイルフォーマットの変更が必要な場合があります。）

-   FASTQファイルのファイルフォーマットを <span style="color: red">fastqsangar</span> に変更します。

（実行時パラメータが必要な場合はここで指定します。）

-   ワークフローを実行します。
    -   \[Workflow\] &gt; \[RNA-seq 01\] &gt; \[run\] をクリックします。

（その他の注意）

-   <span style="color: red">\[注意\]</span> tophat2 はメモリを喰うので、メモリがおおよそ4GB以上ないと「Out of memory」でエラーになります。
