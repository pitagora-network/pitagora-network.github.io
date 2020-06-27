
Galaxyで微生物NGS解析を実行する[Orione](https://wiki.galaxyproject.org/PublicGalaxyServers#Orione)の主たる機能（リード配列の前処理、細菌の再シーケンシング、de novoアセンブリ、RNA-Seq、メタゲノム・メタトランスクリプトーム）を網羅したワークフロー。

Workflow \#1 - Preprocessing
============================

[W1 - Pre-processing | Paired-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w1---pre-processing)
---------------------------------------------------------------------------------------------------------------------------------------

### W1 - Pre-processing | Paired-end | E coli DH10B MiSeq

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Whole genome - Escherichia coli”内の下記項目に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`E coli DH10B MiSeq R1.fastq`
`E coli DH10B MiSeq R2.fastq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W1 - Pre-processing | Paired-end” をimport
-   画面上部の Workflows をクリックし、“imported: W1 - Pre-processing | Paired-end” をRun
-   下記項目を選択し、“Run workflow”ボタンを押す。

`Step 1: Input dataset`
` ForwardReads`
`  E coli DH10B MiSeq R1.fastq`
`Step 2: Input dataset`
` ReverseReads`
`  E coli DH10B MiSeq R2.fastq`

[W1 - Pre-processing | Single-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w1---pre-processing--single-end)
---------------------------------------------------------------------------------------------------------------------------------------------------

### W1 - Pre-processing | Single-end | B suis HiSeq2000

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Whole genome - Brucella suis”内の下記項目に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`B suis HiSeq2000 SE.fastq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W1 - Pre-processing | Single-end” をimport
-   画面上部の Workflows をクリックし、“imported: W1 - Pre-processing | Single-end” をRun
-   画面下部の“Run workflow”ボタンを押す。

Workflow \#2: Bacterial re-sequencing
=====================================

[W2 - Bacterial re-sequencing | Paired-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w2---bacterial-re-sequencing--paired-end)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

### W2 - Bacterial re-sequencing | Paired-end | E coli DH10B MiSeq

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Whole genome - Escherichia coli”内の下記項目に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`E coli DH10B - Reference`
`E coli DH10B MiSeq R1.fastq`
`E coli DH10B MiSeq R2.fastq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W2 - Bacterial re-sequencing | Paired-end” をimport
-   画面上部の Workflows をクリックし、“imported: W2 - Bacterial re-sequencing | Paired-end”をRun
-   下記項目を選択・記入し、“Run workflow”ボタンを押す。

`Step 1: Input dataset`
` Forward Reads`
`  E coli DH10B MiSeq R1.fastq`
`Step 2: Input dataset`
` Reverse Reads`
`  E coli DH10B MiSeq R2.fastq`
`Step 3: Input dataset`
` Reference Genome`
`  E coli DH10B - Reference`
`Step 10: Check bacterial contigs (version 0.0.1)`
` Estimated genome size in Mb`
`  4.686137`
`Step 12: SSPACE (version 1.0.7)`
` Insert size`
`  550`
` Variability (e.g. 0.25 for 25%)`
`  0.25`
`Step 15: Check bacterial contigs (version 0.0.1)`
` Estimated genome size in Mb:`
`  4.686137`
`Step 16: Prokka (version 1.4.0)`
` Fast mode (--fast)`
`  ✔️`

[W2 - Bacterial re-sequencing | Single-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w2---bacterial-re-sequencing--single-end)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

### W2 - Bacterial re-sequencing | Single-end | B suis HiSeq2000

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Whole genome - Brucella suis” に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`B suis HiSeq2000 SE.fastq`
`B suis - Reference genome`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W2 - Bacterial re-sequencing | Single-end” をimport
-   画面上部の Workflows をクリックし、“imported: W2 - Bacterial re-sequencing | Single-end”をRun
-   下記項目を選択・記入し、“Run workflow”ボタンを押す。

`Step 1: Input dataset`
` Reads - Single-end`
`  B suis HiSeq2000 SE.fastq`
`Step 2: Input dataset`
` Reference Genome`
`  B suis - Reference genome`
`Step 10: Check bacterial contigs (version 0.0.1)`
` Estimated genome size in Mb`
`  3.315175`
`Step 15: Check bacterial contigs (version 0.0.1)`
` Estimated genome size in Mb:`
`  3.315175`
`Step 16: Prokka (version 1.4.0)`
` Fast mode (--fast)`
`  ✔️`

Workflow \#3: Bacterial de novo assembly
========================================

[W3 - Bacterial de novo assembly | Paired-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w3---bacterial-de-novo-assembly--paired-end)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### W3 - Bacterial de novo assembly | Paired-end | E coli DH10B MiSeq

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Whole genome - Escherichia coli”内の下記項目に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`E coli DH10B MiSeq R1.fastq`
`E coli DH10B MiSeq R2.fastq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W3 - Bacterial de novo assembly | Paired-end” をimport
-   画面上部の Workflows をクリックし、“imported: W3 - Bacterial de novo assembly | Paired-end”をRun
-   下記項目を選択・記入し、“Run workflow”ボタンを押す。

`Step 1: Input dataset`
` Left/Forward FASTQ Reads`
`  E coli DH10B MiSeq R1.fastq`
`Step 2: Input dataset`
` Right/Reverse FASTQ Reads`
`  E coli DH10B MiSeq R2.fastq`
`Step 14: CISA Contigs Integrator (version 1.0.1)`
` Expected Genome Size (bp)`
`  4686137`
`Step 15: Check bacterial contigs (version 0.0.1)`
` Estimated genome size in Mb`
`  4.686137`
`Step 17: Prokka (version 1.4.0)`
` Fast mode (--fast)`
`  ✔️`

[W3 - Bacterial de novo assembly | Single-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w3---bacterial-de-novo-assembly--single-end)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### W3 - Bacterial de novo assembly | Single-end | B suis HiSeq2000

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Whole genome - Brucella suis”内の下記項目に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`B suis HiSeq2000 SE.fastq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W3 - Bacterial de novo assembly | Single-end” をimport
-   画面上部の Workflows をクリックし、“imported: W3 - Bacterial de novo assembly | Single-end”をRun
-   下記項目を選択・記入し、“Run workflow”ボタンを押す。

`Step 1: Input dataset`
` Reads - Single-end`
`  B suis HiSeq2000 SE.fastq`
`Step 12: CISA Contigs Integrator (version 1.0.1)`
` Expected Genome Size (bp)`
`  3315175`
`Step 13: Check bacterial contigs (version 0.0.1)`
` Estimated genome size in Mb`
`  3.315175`
`Step 15: Prokka (version 1.4.0)`
` Fast mode (--fast)`
`  ✔️`

Workflow \#4: Bacterial RNA-Seq
===============================

[W4 - Bacterial RNA-Seq | Single-end](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w4---bacterial-rna-seq)
---------------------------------------------------------------------------------------------------------------------------------------------

### W4 - Bacterial RNA-Seq | Single-end | Listeria monocytogenes

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“RNA-Seq - Listeria monocytogenes”に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`Listeria_monocytogenes_EGD_e_uid61583/NC_003210.fna`
`Listeria_monocytogenes_EGD_e_uid61583/NC_003210.ptt`
`Listeria_monocytogenes_EGD_e_uid61583/NC_003210.rnt`
`L monocytogenes_EGD/NC_003210.fastq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W4 - Bacterial RNA-Seq | Single-end” をimport
-   画面上部の Workflows をクリックし、“imported: W4 - Bacterial RNA-Seq | Single-end”をRun
-   下記項目を選択・記入し、“Run workflow”ボタンを押す。

`Step 1: Input dataset`
` Reads - Single-end`
`  L monocytogenes_EGD/NC_003210.fastq`
`Step 2: Get EDGE-pro files (version 1.0.1)`
` RefSeq Genomic Accession ID`
`  NC_003210`

Workflow \#5: Metagenomics
==========================

[W5 - Metagenomics](https://orione.crs4.it/workflow/display_by_username_and_slug?username=puva&slug=w5---metagenomics)
----------------------------------------------------------------------------------------------------------------------

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Data Libraries をクリックし、“Orione SupMat”をクリック。“Metagenomics”に✔️を付けて、“Import to current history”を選び、“Go”ボタンを押す。

`MetagenomicsDataset.fq`

-   [Orione](https://orione.crs4.it)にアクセスし、画面上部の Shared Data -&gt; Published Workflows をクリックし、“W5 - Metagenomics” をimport
-   画面上部の Workflows をクリックし、“imported: W5 - Metagenomics”をRun
-   画面下部の“Run workflow”ボタンを押す。

[配列データ](https://orione.crs4.it/library_common/browse_library?sort=name&f-description=All&f-name=All&cntrller=library&operation=browse&id=9cb534f738e19216)
===============================================================================================================================================================

`B suis - Reference genome`
` >gi56968325_chr1  # 2107794 bp # `[`http://www.ncbi.nlm.nih.gov/nuccore/56968325`](http://www.ncbi.nlm.nih.gov/nuccore/56968325)
` >gi_56968493_chr2 # 1207381 bp # `[`http://www.ncbi.nlm.nih.gov/nuccore/56968493`](http://www.ncbi.nlm.nih.gov/nuccore/56968493)
` 2107794 + 1207381 = 3315175 bp`

参考文献
========

-   Orione論文: [pubmed](http://www.ncbi.nlm.nih.gov/pubmed/24618473); [Full text](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4071203/pdf/btu135.pdf); [Supplementary Data](http://bioinformatics.oxfordjournals.org/content/suppl/2014/03/07/btu135.DC1/OrioneSuppMat.pdf)
-   [Galaxy | Published Page | Orione live supplement / CRS4](https://orione.crs4.it/u/puva/p/orione-live-supplement)
-   [Orione 0.1 documentation](http://orione-documentation.readthedocs.org/en/latest/index.html)

------------------------------------------------------------------------

-   [Orione](https://orione.crs4.it)画面右上の歯車マーク（History options）の Create New をクリック。Rename
