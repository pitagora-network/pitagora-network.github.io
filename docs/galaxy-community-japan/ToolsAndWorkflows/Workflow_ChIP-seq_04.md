
概要
----

ワークフロー「ChIP-seq 03」のマッピングを Bowtie2 から BWA に変更したものです。

フロー図
--------

テスト方法
----------

-   Download References ツールを使って BWA Index UCSC hg19 をダウンロードします。（初回実行時のみ）
-   ツールパネルのダウンロードボタンからURLテキストボックスに以下のURLを入力してダウンロードします。
    -   ファイルフォーマットとして <span style="color: red">fastqsangar</span> を選択します。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/test.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/test.fastq.gz)

-   FASTQファイルのファイルフォーマットを <span style="color: red">fastqsangar</span> にするのを忘れた場合はそのように変更します。
-   ワークフローを実行します。 \[Workflow\] &gt; \[ChIp-seq 04\] &gt; BWA で <span style="color: red">hg19</span> を選択 &gt; \[run\] をクリックします。
-   もっと大きな FASTQ ファイル（展開して 2.7 GB）を使う場合はこちら。上の test.fastq.gz （展開して 99.8 MB）はこのファイルの一部です。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/SRR020051.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/SRR020051.fastq.gz)