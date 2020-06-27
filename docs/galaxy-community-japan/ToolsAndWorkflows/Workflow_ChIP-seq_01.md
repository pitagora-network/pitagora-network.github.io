
### 概要

ヒトhg19にマップ済みのリード（BAMファイル）からmacsでピークを検出した後、

ヒト遺伝子のTSS前後1,000bpとオーバーラップするピークを出力します。

使用する主なツール：

-   macs
-   awk
-   join

### 入力ファイル

-   BAM File: マップ済みリード（bamファイル）
-   Gene List File: 遺伝子リスト（bedファイル）
-   Gene ID x Name Table: UCSCの遺伝子IDと遺伝子名のクロスリファレンス表

### 出力ファイル

-   macsのレポート（html）
-   macsのピーク（wigファイル）
-   検出されたピークと対応する遺伝子（csvファイル）

### テスト方法

-   ファイルをヒストリーにダウンロードするためには、左ペインのツールの右横にあるアイコン \[Download from URL or upload files from disk\] &gt; \[Paste/Fetch data\]を選択し、URLテキストボックスに以下のURLを入力して \[start\] をクリックします。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_01/test.bam`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_01/test.bam)
[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_01/knownGene.bed`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_01/knownGene.bed)
[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_01/kgXref.tabular`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_01/kgXref.tabular)

-   ワークフローを実行します。
    -   \[Workflow\] &gt; \[ChIP-seq 01\] &gt; \[run\] をクリックします。
