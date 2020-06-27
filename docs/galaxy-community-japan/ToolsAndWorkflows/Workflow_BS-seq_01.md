
概要
----

このワークフローは、２つのサンプルから得られた BS-Seq リードデータを入力として、各サンプルのシトシンメチル化率の推定と、サンプル比較によるメチル化変化領域の検出を行います。産総研が開発した BS-Seq 解析パイプライン「Bisulfighter」を Galaxy ワークフローとして移植したものです。

-   Bisulfighter
    -   HP:<http://epigenome.cbrc.jp/bisulfighter>
    -   文献:Saito et al, Nucleic Acids Res, 42(6):e45.

ワークフローは、シトシンメチル化率の推定を行う「bsfcall」と、メチル化変化領域の検出を行う「commet」から構成されます。

-   bsfcall
    -   リードをレファレンスゲノムにマッピングして、それぞれのシトシンにおける C--C マッチと C--T ミスマッチのカウントに基づいてメチル化率の推定を行う。
    -   産総研が開発したマッピングツール LAST を使用することにより、高精度な推定を実現している。

<!-- -->

-   commet
    -   bsfcall の出力結果をサンプル間で比較して、メチル化が変化しているゲノム領域を検出する。
    -   HMM に基づく独自のアルゴリズムを採用しており、転写因子結合サイトなど事前に注目すべき領域が分かっていない場合でも、メチル化変化領域の位置を de novo に決定することができる。

### 入力ファイル

比較したい２つのサンプルを、それぞれサンプル１、サンプル２とする。

-   サンプル１の BS-Seq リード (fastqsanger)
-   サンプル２の BS-Seq リード (fastqsanger)

### 出力ファイル

-   サンプル１のシトシンメチル化率推定結果 (tabular)
-   サンプル２のシトシンメチル化率推定結果 (tabular)
-   サンプル１とサンプル２の比較によるメチル化変化領域検出結果 (tabular)
-   各シトシンサイトにおけるメチル化変化の状態 (tabular)

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/BSseq_01.html> |width=550 |height=550 |border=0 }}

テスト方法
----------

-   Download References ツールを使って Bisulfighter Index Test をダウンロードします。（初回実行時のみ）
-   ツールパネルのダウンロードボタンからURLテキストボックスに以下のURLを入力して、ファイルフォーマットにfastqsangerを指定して各ファイルをダウンロードします。

[`http://download.pitagora-galaxy.org/data/workflows/BS-seq_01/1.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/BS-seq_01/1.fastq.gz)
[`http://download.pitagora-galaxy.org/data/workflows/BS-seq_01/2.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/BS-seq_01/2.fastq.gz)

-   ワークフローを実行します。 \[Workflow\] &gt; \[BS-seq 01\] &gt; Step 3/4 bsfcall で test を選択 &gt; \[run\] をクリックします。
