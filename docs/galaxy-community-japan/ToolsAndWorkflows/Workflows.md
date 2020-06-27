
仮想マシンのワークフロー
------------------------

### 現在のワークフロー

-   各ワークフローのベンチマーク結果は[こちら](/Benchmark "wikilink")

| ワークフロー名                                                | 概要                                                                                      |
|---------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| [RNA-seq 01](/Workflow_RNA-seq_01 "wikilink")                 | FastQC - TopHat2 - Cufflinks - Cuffmerge - Cuffdiff                                       |
| [RNA-seq 02](/Workflow_RNA-seq_02 "wikilink")                 | FastQC - FastqMcf - Sailfish                                                              |
| [RNA-seq 03](/Workflow_RNA-seq_03 "wikilink")                 | bam2readcount - edgeR - vehp                                                              |
| [ChIP-seq 02](/Workflow_ChIP-seq_02 "wikilink")               | Bowtie2 - MACS - 遺伝子の TSS 前後 1,000 bp と重なっているピークを取得                    |
| [ChIP-seq 03](/Workflow_ChIP-seq_03 "wikilink")               | Bowtie2 - 4種の peak calling: MACS(1.3) / MACS(1.4) / MACS2 / SICER - 同じ BED 形式で出力 |
| [ChIP-seq 04](/Workflow_ChIP-seq_04 "wikilink")               | BWA - 4種の peak calling (上の ChIP-seq 03 と同じ)                                        |
| [BS-seq 01](/Workflow_BS-seq_01 "wikilink")                   | Bisulfighter によるメチル化変化の検出                                                     |
| [Variant Calling 02](/Workflow_Variant_Calling_02 "wikilink") | GATK 3.3_0 を利用したバリアントの検出 (GenomyAnalysisTK, Picard)                         |
||

### 開発中のワークフロー

| ワークフロー名                                                | 概要                                                  |
|---------------------------------------------------------------|-------------------------------------------------------|
| [RNA-seq 05](/Workflow_RNA-seq_05 "wikilink")                 | Trinity + Rsem                                        |
| [Variant Calling 03](/Workflow_Variant_Calling_03 "wikilink") | HLA (ヒト主要組織適合抗原) タイピング/解析ツール      |
| [Variant Calling 04](/Workflow_Variant_Calling_04 "wikilink") | RNA-SeqからのVariant call: STAR + GATK                |
| [ChIP-seq 05](/Workflow_ChIP-seq_05 "wikilink")               | ピーク領域をウィンドウに分割して可視化する            |
| [DNApod Workflow](/Workflow_VM_DNApod "wikilink")             | DNA polymorphisms の検出 + 既知遺伝子アノテーション   |
| [ChIP-seq 05](/Workflow_ChIP-seq_05 "wikilink")               | ピーク領域をウィンドウに分割して可視化する            |
| [MiGAP](/Workflow_MiGAP "wikilink")                           | MiGAPを用いた微生物メタゲノムのアノテーション         |
| [MeGAP](/Workflow_MeGAP "wikilink")                           | MeGAPを用いた微生物メタゲノムのアノテーション（前半） |
| [Qiime](/Workflow_Qiime "wikilink")                           |                                                       |
| [Bridger](/Workflow_Bridger "wikilink")                       |                                                       |
| [GATK4Beta](/Workflow_GATK4Beta "wikilink")                   | GATK 4 Beta                                           |
||

### 過去のワークフロー

| ワークフロー名                                                | 概要                                |
|---------------------------------------------------------------|-------------------------------------|
| [ChIP-seq 01](/Workflow_ChIP-seq_01 "wikilink")               | MACS - Join - Join two Datasets     |
| [Variant Calling 01](/Workflow_Variant_Calling_01 "wikilink") | GATK 1.6 を利用したバリアントの検出 |
||

[外部サーバー](https://wiki.galaxyproject.org/PublicGalaxyServers)のワークフロー
--------------------------------------------------------------------------------

### [Galaxy Main](https://usegalaxy.org/)

| ワークフロー名                                            | 概要                                                                 |
|-----------------------------------------------------------|----------------------------------------------------------------------|
| [Bam to BigWig 01](/Workflow_Bam_to_BigWig_01 "wikilink") | BAM から BigWig を作成する。この際、スコアを合計リード数で補正する。 |
||

### [Orione](https://orione.crs4.it/)

| ワークフロー名                                               | 概要                                                                                                                                                                                     |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [orione-live-supplement](/orione-live-supplement "wikilink") | 微生物NGS解析：リードの前処理、リシーケンシング、de novoアセンブリ、RNA-Seq、メタゲノム                                                                                                  |
| [Workflows](/Orione_Workflows "wikilink")                    | [論文](http://www.ncbi.nlm.nih.gov/pubmed/24618473)の[Supplementary Material](http://bioinformatics.oxfordjournals.org/content/suppl/2014/03/07/btu135.DC1/OrioneSuppMat.pdf)のWorkflows |
||

### [VirAmp](http://viramp.com/)

| ワークフロー名                            | 概要                       |
|-------------------------------------------|----------------------------|
| [Workflows](/VirAmp_Workflows "wikilink") | ウイルス ゲノム アセンブリ |
||

### [Huttenhower Lab](http://huttenhower.sph.harvard.edu/galaxy/)

| ワークフロー名                                     | 概要           |
|----------------------------------------------------|----------------|
| [Workflows](/Huttenhower_Lab_Workflows "wikilink") | メタゲノム解析 |
||

この Wiki にワークフローを追加する手順
--------------------------------------

-   上記の適切な表に新規行を追加します。ワークフロー名が「Variant Calling 01」の場合は次のように記載します。

`|-`
`| [[/Workflow_Variant_Calling_01|Variant Calling 01]]`
`| GATK 1.6 を利用したバリアントの検出`
`|-`

-   このWikiリンクに移動すると新しいページ「Workflow <ワークフロー名>」が作成されます。
-   ここに、次の必須項目をコピペして、ページの内容の編集を開始します。

<!-- -->

    [[/Main_Page|Main Page]] >> [[/Workflows|Workflows]] >>

    == 概要 ==

    === 入力ファイル ===

    === 出力ファイル ===

    == フロー図 ==

    == テスト方法 ==

-   サンプル [Workflow Sample](/Workflow_Sample "wikilink") を参考にしてください。
