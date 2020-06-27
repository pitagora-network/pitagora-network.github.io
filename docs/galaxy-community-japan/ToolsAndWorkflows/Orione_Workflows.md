
-   Orione論文: [pubmed](http://www.ncbi.nlm.nih.gov/pubmed/24618473); [Full text](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4071203/pdf/btu135.pdf); [Supplementary Data](http://bioinformatics.oxfordjournals.org/content/suppl/2014/03/07/btu135.DC1/OrioneSuppMat.pdf)
-   [Orione 0.1 documentation](http://orione-documentation.readthedocs.org/en/latest/index.html)

------------------------------------------------------------------------

##### Workflows ワークフロー

<http://bioinformatics.oxfordjournals.org/content/suppl/2014/03/07/btu135.DC1/OrioneSuppMat.pdf>

Galaxyのワークフローを用いて、異なるツールを組み合わせた再現性のある処理パイプラインを異なるデータセットに対して自動実行できる（パラメータをリセットする必要なし）。Orioneの主たる機能（前処理、細菌のde novo・再シーケンシング、RNA-Seq、メタゲノム解析）を網羅したワークフローを用意した。ワークフローの説明では、各ステップで使用するツールを\[\]括弧内に示した。[リンク付きの補足資料](http://orione.crs4.it/u/puva/p/orione-live-supplement)

-   Workflow \#1: Pre-processing 前処理
-   Workflow \#2: Bacterial re-sequencing 細菌の再シーケンシング
-   Workflow \#3: Bacterial de novo assembly 細菌のde novoアセンブリ
-   Workflow \#4: Bacterial RNA-Seq 細菌のトランスクリプトーム解析
-   Workflow \#5: Metagenomics メタゲノム
-   Workflow \#6: Metatranscriptomics メタトランスクリプトーム

------------------------------------------------------------------------

------------------------------------------------------------------------

##### 準備: サンプルデータのインポート

-   <https://orione.crs4.it> にアクセス
-   画面上部のShared Data -&gt; Data Libraries をクリックして、Example: E. coli をクリック

`NC_012967.1.fasta`
`README.txt`
`SRR030257_1.fastq`
`SRR030257_2.fastq`

“Import to current history”を選び、“Go”ボタンを押す。

------------------------------------------------------------------------

##### Workflow \#1: Pre-processing 前処理

Input 入力:

-   Raw FASTQ paired-end reads 未処理のペアエンド・リード配列（SRR030257_1.fastq と SRR030257_2.fastq）

Output 出力:

Steps 操作:

1. Conversion of the input FASTQ file to the fastqsanger quality encoding \[FASTQ groomer\] 入力FASTQファイルをfastqsangerクオリティ表記に変換

`File to groom:`
` SRR030257_1.fastq`

`File to groom:`
` SRR030257_2.fastq`

2. Quality control of raw reads \[FastQC\] 処理前のリード配列のクオリティ・コントロール

`Short read data from your current history:`
` FASTQ Groomer on data *`

`Short read data from your current history:`
` FASTQ Groomer on data *`

3. Adapter removal \[Flexbar\] アダプタ除去

`Sequencing reads:`
` FASTQ Groomer on data *`
`2nd read set (paired):`
` ON`
`Reads 2:`
` FASTQ Groomer on data *`

4. Trimming/filtering by quality and length \[FASTQ positional and quality trimming\] クオリティと長さでトリミング/フィルタリング

`Is this library mate-paired?:`
` Paired-end`
`Forward FASTQ file:`
` Flexbar on data * and data * (FlexbarTargetFile-1.fastq)`
`Reverse FASTQ file:`
` Flexbar on data * and data * (FlexbarTargetFile-2.fastq)`

5. Filtering by sequence composition \[Paired-end compositional filtering\] 配列組成によるフィルタリング

`Forward FASTQ file:`
` FASTQ positional and quality trimming on data * and data *: trimmed forward FASTQ`
`Reverse FASTQ file:`
` FASTQ positional and quality trimming on data * and data *: trimmed reverse FASTQ`

6. Quality control of processed reads \[FastQC\] 処理後のリード配列のクオリティ・コントロール

`Short read data from your current history:`
` Paired-end compositional filtering on data * and data *: filtered forward FASTQ`

`Short read data from your current history:`
` Paired-end compositional filtering on data * and data *: filtered reverse FASTQ`

------------------------------------------------------------------------

##### Workflow \#2: Bacterial re-sequencing 細菌の再シーケンシング

Input 入力:

-   Workflow \#1で処理されたFASTQリード配列データ
-   参照ゲノム配列（NC_012967.1.fasta）

Output 出力:

-   Contigs sequences (FASTA)
-   Scaffolds sequences (FASTA)
-   Scaffolds annotations (multiple formats available)
-   Report with draft/contigs/scaffolds quality

Steps 操作:

1. Align against a reference genome with BWA \[Map with BWA-MEM\] BWAを用いて参照ゲノムにマッピング

`Will you select a reference genome from your history or use a built-in index?:`
` Use one from the history`
`Select a reference from history:`
` NC_012967.1.fasta`
`Is this library mate-paired?:`
` Paired-end`
`Forward FASTQ file:`
` trimmed forward FASTQ　（filtered forward FASTQ が表示されないバグ？）`
`Reverse FASTQ file:`
` trimmed reverse FASTQ　（filtered reverse FASTQ が表示されないバグ？）`

2. Convert alignment from SAM to BAM format \[SAM to BAM\] マッピング結果をSAM形式からBAM形式に変換

`Choose the source for the reference list:`
` History`
`SAM file to convert:`
` Map with BWA-MEM on data *, data *, and data *: mapped reads`
`Using reference file:`
` NC_012967.1.fasta`

3. Extract a draft consensus sequence \[BAM to consensus\] ドラフトのコンセンサス配列を抽出

`Choose the source for the reference list:`
` History`
`Input BAM file:`
` SAM-to-BAM on data * and data *: converted BAM`
`Using reference file:`
` NC_012967.1.fasta`

4. Convert the draft from FASTQ to FASTA \[FASTQ to FASTA converter\] ドラフトゲノムをFASTQ形式からFASTA形式に変換

`FASTQ file to convert:`
` BAM to consensus on data * and data *: FASTQ`

Output (FASTQ to FASTA on data \*) contains 1 sequence by concatenating multiple sequences with 'n'

5. Evaluate draft quality \[Check bacterial draft\] ドラフトゲノムのクオリティ評価

`Draft genome:`
` FASTQ to FASTA on data *`
`Reference genome:`
` NC_012967.1.fasta`

6. Extract contigs (longer than a given threshold) from draft \[Extract contigs\] ドラフトゲノムからコンティグを抽出

`Draft:`
` FASTQ to FASTA on data *`
`Minimum contig length:`
` 300`

Output (Extract contigs on data \*: contigs) contains 7 sequences.

Output (Extract contigs on data \*: high quality contigs) contains 100 sequences.

7. Evaluate contigs quality \[Check bacterial contigs\] コンティグのクオリティ評価

`Contigs collection:`
` Extract contigs on data *: contigs`
`Estimated genome size in Mb:`
` 4.629812`

`Contigs collection:`
` Extract contigs on data *: high quality contigs`
`Estimated genome size in Mb:`
` 4.629812`

8. Contigs scaffolding by SSPACE/SSAKE \[SSPACE/SSAKE\] ペアエンドを使って、コンティグ配列を結合し、スーパーコンティグ（スキャフォールド）を生成する SSPACE (for paired-end reads) or SSAKE (for single-end reads)

`Contigs FASTA file (-s):`
` Extract contigs on data *: high quality contigs`
`Paired-end reads 1:`
` Paired-end compositional filtering on data * and data *: filtered forward FASTQ`
`Paired-end reads 2:`
` Paired-end compositional filtering on data * and data *: filtered reverse FASTQ`
`Insert size:`
` 300`
`Variability (e.g. 0.25 for 25%):`
` 0.25`

9. Scaffolds evaluation \[Check bacterial contigs\] スキャフォールドの評価

`Contigs collection:`
` SSPACE on data *, data *, and data *: final scaffolds`
`Estimated genome size in Mb:`
` 4.629812`

10.Align scaffolds against reference \[Mugsy\] スキャフォールドを参照ゲノム配列にマッピング

`Reference:`
` NC_012967.1.fasta`
`Contigs/draft:`
` SSPACE on data *, data *, and data *: final scaffolds`

11. Convert Mugsy output to FASTA \[MAF to FASTA\] Mugsyの出力をFASTA形式に変換

`MAF file to convert:`
` Mugsy on data * and data *: MAF`

12.Reformat FASTA output with 60 nucleotides per row \[FASTA Width formatter\] 配列の表示形式を行あたり60塩基になるように変換

`Library to re-format:`
` MAF to FASTA on data *`
`New width for nucleotides strings:`
` 60`

13.Annotate draft/contigs \[Prokka\] ドラフトゲノム（コンティグ）配列のアノテーション

`Contigs to annotate:`
` FASTA Width on data *`
`Fast mode (--fast):`
` Skip CDS /product searching`

------------------------------------------------------------------------

##### Workflow \#3: Bacterial de novo assembly 細菌のde novoアセンブリ

このワークフローは、複数のde novoアセンブラー（Velvet, ABySS, SPAdes）を用いて、細菌ゲノムのde novoアセンブリを実行する。このワークフローでは、異なるK-mer値でVelvetを3回実行し、コンティグをマージしCD-HITツール（類似度1.0）でクラスタリングする。コンティグをCISAで統合し、基本的な統計を“Check bacterial contigs”ツールで計算し、最後にProkkaで配列をアノテーションする。

Input 入力:

-   Workflow \#1で処理されたFASTQリード配列データ

Output 出力: • Contigs/Scaffolds from each assembler (FASTA) • Integrated contig sequences (FASTA) • Sequence annotations (multiple formats available) • Report with de novo assembly statistics

Steps 操作:

1. Prepare reads for Velvet \[velveth\] with three k-mer values ３つのk-mer値でvelvethを実行

`Hash Length:`
` 21`
`Input Files`
` Input Files 1`
`  file format:`
`   fastq`
`  read type:`
`   -shortPaired reads`
`  Dataset:`
`   Paired-end compositional filtering on data * and data *: filtered forward FASTQ`
` Input Files 2`
`  file format:`
`   fastq`
`  read type:`
`   -shortPaired2 reads`
`  Dataset:`
`   Paired-end compositional filtering on data * and data *: filtered reverse FASTQ`

2. De novo assembly with Velvet \[velvetg\] Velvetでde novoアセンブリ

`Velvet Dataset:`
` velveth on data * and data *`

3. Merge Velvet contigs and cluster them at 1.00 identity \[CD-HIT\] Velvetのコンティグをマージし、類似度1.0でクラスタリング

`Sequences to cluster:`
` velvetg on data *: Contigs`
`Similarity threshold: 0.75 - 1.0:`
` 1.0`

4. De novo assembly with ABySS \[ABySS\] ABySSでde novoアセンブリ

`Paired Reads Files`
` Paired Reads Files 1`
` Paired read sequences:`
`  Paired-end compositional filtering on data * and data *: filtered forward FASTQ`
` Paired Reads Files 2`
` Paired read sequences:`
`  Paired-end compositional filtering on data * and data *: filtered reverse FASTQ`
`[-k] K-mer size:`
` 21`

Tool execution generated the following error message: ERROR RUNNING COMMAND: gunzip /SHARE/USERFS/els7/users/biobank/galaxy/central/job_wd/000/167/167659/dataset_285127_files/abyss-3.sam.gz gzip: /SHARE/USERFS/els7/users/biobank/galaxy/central/job_wd/000/167/167659/dataset_285127_files/abyss-3.sam.gz: No such file or directory

5. De novo assembly with SPAdes \[SPAdes\] SPAdesでde novoアセンブリ

` Forward reads:`
`  Paired-end compositional filtering on data * and data *: filtered forward FASTQ`
` Reverse reads:`
`  Paired-end compositional filtering on data * and data *: filtered reverse FASTQ`

6. Integrates contigs with CISA \[CISA Contigs Integrator\] CISAでコンティグを統合

`Contigs file 1`
`Contigs file:`
` SPAdes scaffolds (fasta)`
`Contigs file 2`
`Contigs file:`
` CD-HIT on data 64: representatives.fasta`
`Expected Genome Size (bp):`
` 4629812`

7. Evaluate contigs/scaffolds quality \[Check bacterial contigs\] コンティグ/スキャフォールドのクオリティ評価

`Contigs collection:`
` CISA Contigs Integrator on data * and data *`
`Estimated genome size in Mb:`
` 4.629812`

8. Annotate sequences \[Prokka\] 配列のアノテーション

`Contigs to annotate:`
` CISA Contigs Integrator on data * and data *`
`Fast mode (--fast):`
` Skip CDS /product searching`

------------------------------------------------------------------------

##### Workflow \#4: Bacterial RNA-Seq 細菌のRNA-Seq解析

-   [Galaxy Workflow | W4 - Bacterial RNA-Seq | Paired-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w4---bacterial-rna-seq--paired-end) 入力データ：ペアエンド・リード配列
-   [Galaxy Workflow | W4 - Bacterial RNA-Seq | Single-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w4---bacterial-rna-seq) 入力データ：シングルエンド・リード配列

`Listeria_monocytogenes_EGD_e_uid61583/NC_003210.fna`
`Listeria_monocytogenes_EGD_e_uid61583/NC_003210.ptt`
`Listeria_monocytogenes_EGD_e_uid61583/NC_003210.rnt`
`L monocytogenes_EGD/NC_003210.fastq`

-   [Data Library](https://orione.crs4.it/library_common/browse_library?sort=name&f-description=All&f-name=All&cntrller=library&operation=browse&id=9cb534f738e19216)にアクセスし、\[RNA-Seq - Listeria monocytogenes\] に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。
-   画面上部の Shared Data -&gt; Published Workflows をクリックし、“W4 - Bacterial RNA-Seq | Single-end” をimport
-   Workflows をクリックし、“imported: W4 - Bacterial RNA-Seq | Single-end” をRun

`Step 1: Input dataset`
`Reads in FASTQ format`
`Reads - Single-end`
` L monocytogenes_EGD/NC_003210.fastq`
`Step 2: Get EDGE-pro files (version 1.0.1)`
` RefSeq Genomic Accession ID`
`  NC_003210`

“Run workflow”ボタンを押す。

------------------------------------------------------------------------