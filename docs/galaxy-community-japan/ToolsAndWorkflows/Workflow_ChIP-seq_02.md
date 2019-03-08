---
title: Workflow ChIP-seq 02
permalink: /Workflow_ChIP-seq_02/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

概要
----

ヒトhg19のサンプル（FASTQファイル）にbowtie2でマッピングし、macsでピークを検出した後、

ヒト遺伝子のTSS前後1,000bpとオーバーラップするピークを出力します。

使用する主なツール：

-   bowtie2
-   macs
-   awk
-   join

### 入力ファイル

-   BAM File: マップ済みリード（bamファイル）
-   Gene List File: 遺伝子リスト（bedファイル）
-   Gene ID x Name Table: UCSCの遺伝子IDと遺伝子名のクロスリファレンス表

### 出力ファイル

-   リード済みリード（bamファイル）
-   macsのレポート（html）
-   macsのピーク（wigファイル）
-   検出されたピークと対応する遺伝子（csvファイル）

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/ChipSeq_02.html> |width=900 |height=1150 |border=0 }}

テスト方法
----------

-   Download References ツールを使って Fasta UCSC hg19 および Bowtie2 Index UCSC hg19 をダウンロードします。（初回実行時のみ）
    -   索引作成の元となる Fasta ファイルも必要であることに注意してください。
-   ツールパネルのダウンロードボタンからURLテキストボックスに以下のURLを入力して各ファイルをダウンロードします。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/test.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/test.fastq.gz)
[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/knownGene.bed`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/knownGene.bed)
[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/kgXref.tabular`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/kgXref.tabular)

-   FASTQファイルのファイルフォーマットを <span style="color: red">fastqsangar</span> に変更します。
-   ワークフローを実行します。 \[Workflow\] &gt; \[ChIp-seq 02\] &gt; bowtie2 で <span style="color: red">hg19</span> を選択 &gt; \[run\] をクリックします。
-   もっと大きな FASTQ ファイル（展開して 2.7 GB）を使う場合はこちら。上の test.fastq.gz （展開して 99.8 MB）はこのファイルの一部です。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/SRR020051.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/SRR020051.fastq.gz)