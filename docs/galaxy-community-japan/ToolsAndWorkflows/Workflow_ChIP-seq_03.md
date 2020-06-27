
概要
----

このChIP-seqワークフローは、ChIP-seq実験により得られたゲノム領域データより、転写制御因子の結合領域、特定のヒストン修飾領域を特定します。その他、FAIRE-seqやDNase-seqデータを用い、ゲノム上のオープンクロマチン領域の特定に用いられます。 　Bowtie2でシーケンスリードのマッピング後に、MACS及びSICERを用いてピークコールを行います。MACSについては、異なるバージョンのプログラムごとにピーク領域が検出されます。

-   Bowtie2
    -   HP：http://bowtie-bio.sourceforge.net/bowtie2/index.shtml
    -   文献：Langmead B, Salzberg S. Fast gapped-read alignment with Bowtie 2. Nature Methods. 2012, 9:357-359
    -   特徴：Burrows-Wheeler Alignmentアルゴリズムを応用し、参照ゲノムに対して一致率の高いシーケンスシードを高速でマッピングプログラム
    -   入力：fastqファイル (DNA配列が決定済みで、マッピング位置のわかっていないシーケンスリードの集合データ)
    -   出力：bamファイル (マッピング位置の特定されたシーケンスシード集合のbinaryデータ)

<!-- -->

-   MACS
    -   HP：http://liulab.dfci.harvard.edu/MACS/
    -   文献：Zhang et al. Model-based Analysis of ChIP-Seq. Genome Biol, 2008, vol. 9 (9) pp. R137
    -   特徴：数100bpの領域にリード配列が濃縮しピークを形成するデータに対して、ピーク位置を補正し、ポアソンブンプに当てはめ有意なピーク領域を特定する計算アルゴリズム
    -   入力：bamファイル、eland exportファイル (illumina社の提供するマッピングツールCASAVAによって得られたシーケンスリードデータ)
    -   出力：peak bedファイル (有意なピーク領域データで、各列は染色体、開始、終了、名前、シグナル強度を表す)、summit bedファイル (有意なピーク領域におけるピークの頂上位置データ)、wigファイル (Igb、igvでシグナル波形を見るためのテキストファイル)

<!-- -->

-   SICER
    -   HP：http://bowtie-bio.sourceforge.net/bowtie2/index.shtml
    -   文献：Zang C, Schones DE, Zeng C, Cui K, Zhao K, Peng W, Bioinformatics, 2009 Aug 1;25(15):1952-8
    -   特徴：形成されるピークの幅に一定の間隔がなく、数1000bpに及ぶ濃縮が領域が多数存在するデータに対して、特定のモデルに当てはめず有意なピーク領域を抽出するアルゴリズム
    -   入力：samファイル (テキスト形式に変換されたbamファイル、コントロールとトリートメントを両方をインプットする必要あり)
    -   出力：scoreislandファイル (bedファイル同様に、有意なピーク領域データで、各列は染色体、開始、終了、シグナル強度を表す)

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/ChipSeq_03.html> |width=800 |height=950 |border=0 }}

テスト方法
----------

-   Download References ツールを使って Bowtie2 Index UCSC hg19 をダウンロードします。（初回実行時のみ）
-   ツールパネルのダウンロードボタンからURLテキストボックスに以下のURLを入力してダウンロードします。
    -   ファイルフォーマットとして <span style="color: red">fastqsangar</span> を選択します。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/test.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/test.fastq.gz)

-   FASTQファイルのファイルフォーマットを <span style="color: red">fastqsangar</span> にするのを忘れた場合はそのように変更します。
-   ワークフローを実行します。 \[Workflow\] &gt; \[ChIp-seq 03\] &gt; bowtie2 で <span style="color: red">hg19</span> を選択 &gt; \[run\] をクリックします。
-   もっと大きな FASTQ ファイル（展開して 2.7 GB）を使う場合はこちら。上の test.fastq.gz （展開して 99.8 MB）はこのファイルの一部です。

[`http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/SRR020051.fastq.gz`](http://download.pitagora-galaxy.org/data/workflows/ChIP-seq_02/SRR020051.fastq.gz)