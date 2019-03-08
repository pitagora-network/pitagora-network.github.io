---
title: Workflow RNA-seq 01
permalink: /Workflow_RNA-seq_01/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

概要
----

このRNA-seqワークフローは、ペアエンドの異なる2つのターゲットを比較します。

転写産物のシークエンス量から遺伝子の発現量を定量化し、配列情報から選択的スプライシングの検出や未知の転写産物を発見する解析手法です。

### 入力ファイル

-   FASTQ File: 塩基配列とクオリティスコアのテキストファイル（fastqファイル）
    -   サンプル1（フォワード側）
    -   サンプル1（リバース側）
    -   サンプル2（フォワード側）
    -   サンプル2（リバース側）
-   アノテーションファイル（gtfファイル）

### 出力ファイル

-   FASTQCのレポート（html）
-   TopHat2のマッピング情報（txt, bed）
-   TopHat2のマップ済みリード（bam）
-   CufflinksのFPKM（txt, tsv）
-   Cufflinksのトランスクリプト（gtf）
-   Cuffmergeでマージされたトランスクリプト（gtf）
-   Cuffdiffの発現量の統計情報（tsv）

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/RNAseq_01.html> |width=900 |height=900 |border=0 }}

テスト方法
----------

-   Download References ツールで Fasta UCSC hg19 および Bowtie2 Index UCSC hg19 をダウンロードします。（初回実行時のみ）
    -   索引作成の元となる Fasta ファイルも必要であることに注意してください。
-   ファイルをヒストリーにダウンロードするためには、左ペインのツールの右横にあるアイコン \[Download from URL or upload files from disk\] &gt; \[Paste/Fetch data\]を選択し、URLテキストボックスに以下のURLを入力して \[start\] をクリックします。

[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_1.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_1.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_2.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/adrenal_2.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_1.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_1.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_2.fastq`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/brain_2.fastq)
[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/gene_annotation.gtf`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/gene_annotation.gtf)

-   FASTQファイルのファイルフォーマットを <span style="color: red">fastqsangar</span> に変更します。
-   ワークフローを実行します。
    -   \[Workflow\] &gt; \[RNA-seq 01\] &gt; \[run\] をクリックします。
-   <span style="color: red">\[注意\]</span> tophat2 はメモリを喰うので、メモリがおおよそ4GB以上ないと「Out of memory」でエラーになります。

補足： リファレンス・ゲノムがピタゴラ・ギャラクシーに用意されていない場合
-------------------------------------------------------------------------

ピタゴラ・ギャラクシーで用意しているリファレンス・ゲノムはヒトとマウスのみであるため、それ以外のリファレンス・ゲノムは以下の手順で利用します。

-   リファレンス・ゲノム（ FASTA 形式、圧縮されたままでも OK）をヒストリーにアップロードします。
-   Tophat2 の実行時に「Use a built in reference genome or own from your history」で「use a genome from your history」を選択します。
-   「Select the reference genome」という項目が現れるので、アップロードしたリファレンス・ゲノムを指定します。

この場合、Tophat2 の実行時に毎回リファレンス・ゲノムから Bowtie2 インデックスが作成された後、マッピング処理が実行されます。

補足： フルサイズのデータ
-------------------------

-   このワークフローは Galaxy Team のチュートリアルの内容を基にしています。
    -   <https://usegalaxy.org/u/jeremy/p/galaxy-rna-seq-analysis-exercise>
-   このチュートリアルおよび上記のテストでは chr19 のみに絞ることでデータサイズを小さくしています。フルサイズで実行する場合には次のファイルを使用します。
    -   ERR030881 (adrenal) および ERR030882 (brain) -- <http://www.ebi.ac.uk/ena/data/view/ERP000546>
    -   アノテーション
        -   iGenomes [こちら](https://support.illumina.com/sequencing/sequencing_software/igenome.html)のhg19をダウンロードおよび展開してGTFファイルを入手
            -   /igenome/Homo_sapiens/UCSC/hg19/Annotation/Archives/archive-2013-03-06-11-23-03/Genes/genes.gtf (current) のみを取り出したものがこちら

[`http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/genes.gtf`](http://download.pitagora-galaxy.org/data/workflows/RNA-seq_01/genes.gtf)

-   -   UCSC [こちら](http://genome.ucsc.edu/cgi-bin/hgTables?hgsid=324946975&clade=mammal&org=Human&db=hg19&hgta_group=genes&hgta_track=refGene&hgta_table=refFlat&hgta_regionType=genome&hgta_outputType=gtf&hgta_outFileName=hg19.refFlat.20130205.gtf.gz)
    -   これらの違いについては[ここ](https://cell-innovation.nig.ac.jp/wiki/tiki-index.php?page=GTF%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%81%AE%E7%B4%B0%E3%81%8B%E3%81%AA%E9%81%95%E3%81%84)

