---
title: Workflow Qiime
permalink: /Workflow_Qiime/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

[Quantitative Insights into Microbial Ecology (QIIME)](http://qiime.org)
========================================================================

-   [Qiime ツールのインストール方法](/Qiime_ツールのインストール方法 "wikilink") は別のページに記載

[Illumina Overview Tutorial](http://nbviewer.jupyter.org/github/biocore/qiime/blob/1.9.1/examples/ipynb/illumina_overview_tutorial.ipynb)
-----------------------------------------------------------------------------------------------------------------------------------------

### [Getting started](http://nbviewer.jupyter.org/github/biocore/qiime/blob/1.9.1/examples/ipynb/illumina_overview_tutorial.ipynb#Getting-started)

Downloading the tutorial data from <ftp://ftp.microbio.me/qiime/tutorial_files/moving_pictures_tutorial-1.9.0.tgz>

For the purpose of this tutorial we will be using the dataset available here: <https://github.com/galaxyproject/tools-iuc/tree/qiime/tools/qiime/test-data>

### [Check our mapping file for errors](http://nbviewer.jupyter.org/github/biocore/qiime/blob/1.9.1/examples/ipynb/illumina_overview_tutorial.ipynb#Check-our-mapping-file-for-errors)

[validate_mapping_file.py – Checks user’s metadata mapping file for required data, valid format](http://qiime.org/scripts/validate_mapping_file.html)

-   Step 1: Go to the [Galaxy server](http://52.196.54.200/galaxy/).
-   Step 2: Upload mapping file
    -   From the menu on the left-hand side, click on \[Get Data\] and then \[Upload File\] from your computer. Click \[Choose local file\] to select the QIIME mapping file “map.tsv” you downloaded to your computer.
    -   Click \[Execute\] button to upload the file.
-   Step 3: Validate mapping file
    -   From the menu on the left-hand side, click on \[Qiime\] and then \[Validate mapping file\] to check for required data and format.
    -   For \[Metadata mapping filepath\], select the tree you just uploaded “map.tsv”
    -   Click on the \[Execute\] button

### [Demultiplexing and quality filtering sequences](http://nbviewer.jupyter.org/github/biocore/qiime/blob/1.9.1/examples/ipynb/illumina_overview_tutorial.ipynb#Demultiplexing-and-quality-filtering-sequences)

[split_libraries_fastq.py – This script performs demultiplexing of Fastq sequence data where barcodes and sequences are contained in two separate fastq files (common on Illumina runs).](http://qiime.org/scripts/split_libraries_fastq.html)

`!split_libraries_fastq.py -o slout/ -i forward_reads.fastq.gz -b barcodes.fastq.gz -m map.tsv`

-   Step 1: Go to the [Galaxy server](http://52.196.54.200/galaxy/).
-   Step 2: Upload files
    -   From the menu on the left-hand side, click on \[Get Data\] and then \[Upload File\] from your computer. Click \[Choose local file\] to select files “forward_reads.fastq” and “barcodes.fastq”.
    -   Click \[Execute\] button to upload the files.
-   Step 3: Split fastq libraries
    -   From the menu on the left-hand side, click on \[Qiime\] and then \[Split fastq libraries\] to performs demultiplexing of Fastq sequence data.
    -   Select Options as follows:

`Input fastq files`
` forward_reads.fastq.gz`
`Metadata mapping files (optional)`
` map.tsv`
`Barcode read files (optional)`
` barcodes.fastq.gz`

-   -   Click on the \[Execute\] button

[count_seqs.py –](http://qiime.org/scripts/count_seqs.html)

`!count_seqs.py -i slout/seqs.fna`

-   From the menu on the left-hand side, click on \[Qiime\] and then \[Count sequences\] Count the sequences in a fasta file
-   Select Options as follows:

`Input sequence file`
` 52 Split fastq libraries on data 18, data 11, and data 19: sequences`

-   Click on the \[Execute\] button

53 Count sequences on data 52: sequence counts

### \[<http://nbviewer.jupyter.org/github/biocore/qiime/blob/1.9.1/examples/ipynb/illumina_overview_tutorial.ipynb#OTU-picking:-using-an-open-reference-OTU-picking-protocol-by-searching-reads-against-the-Greengenes-database>. OTU picking: using an open-reference OTU picking protocol by searching reads against the Greengenes database.\]

[pick_open_reference_otus.py – Perform open-reference OTU picking](http://qiime.org/scripts/pick_open_reference_otus.html)

`!pick_open_reference_otus.py -o otus/ -i slout/seqs.fna -p ../uc_fast_params.txt`

-   Go to the [Galaxy server](http://52.196.54.200/galaxy/).
-   From the menu on the left-hand side, click on \[Qiime\] and then \[Perform open-reference OTU picking\]
-   Select Options as follows:

`Input sequence files`
` 52 Split fastq libraries on data 18, data 11, and data 19: sequences`

-   Click on the \[Execute\] button

`!biom summarize-table -i otus/otu_table_mc2_w_tax_no_pynast_failures.biom`

### [Run diversity analyses](http://nbviewer.jupyter.org/github/biocore/qiime/blob/1.9.1/examples/ipynb/illumina_overview_tutorial.ipynb#Run-diversity-analyses)

[core_diversity_analyses.py – A workflow for running a core set of QIIME diversity analyses.](http://qiime.org/scripts/core_diversity_analyses.html)

`!core_diversity_analyses.py -o cdout/ -i otus/otu_table_mc2_w_tax_no_pynast_failures.biom -m map.tsv -t otus/rep_set.tre -e 1114`

-   Go to the [Galaxy server](http://52.196.54.200/galaxy/).
-   From the menu on the left-hand side, click on \[Qiime\] and then \[Compute core set of diversity analyses\]
-   Select Options as follows:

`OTU table`
` 123 Perform open-reference OTU picking on data 52: OTU table with taxonomy`
` # OR`
` 122 Perform open-reference OTU picking on data 52: OTU table`
`Mapping file`
` 11 map.tsv`
`Sequencing depth to use for even sub-sampling and maximum rarefaction depth`
` 1114`
`Tree file`
` 126 Perform open-reference OTU picking on data 52: representative set tree`

-   Click on the \[Execute\] button

エラー

179 Compute core set of diversity analyses on data 11 and data 123: Core diversity report

tool error An error occurred with this dataset:

cannot find 'tree_fp' while searching for 'phylogenetic.tree_fp'

### Notes

-   <https://github.com/galaxyproject/tools-iuc/issues/1078> QIIME Contribution Fest - 9th & 10th January 2017 · Issue \#1078 · galaxyproject/tools-iuc
-   <https://github.com/galaxyproject/tools-iuc/tree/qiime>
    -   <https://github.com/galaxyproject/tools-iuc/tree/qiime/tools/qiime/test-data>
-   <http://wiki.pitagora-galaxy.org/wiki/index.php/Workflows>
    -   <http://wiki.pitagora-galaxy.org/wiki/index.php/Huttenhower_Lab_Workflows>
-   <https://github.com/haruosuz/qiime>
-   [QIIME Tutorials](http://qiime.org/tutorials/index.html)
-   -   [Bioinformatics Analysis – MetaSUB](http://metasub.org/methods/bioinformatics-analysis/)
-   [Boston subway microbiome](http://msystems.asm.org/content/1/3/e00018-16)
    -   “Operational taxonomic unit (OTU) tables were constructed with Quantitative Insights into Microbial Ecology (QIIME) software (51) version 1.8 (pick_closed_reference_otus.py from <http://qiime.org/scripts/>) with Greengenes reference version 13.5 at the 97% identity level.”

------------------------------------------------------------------------

後程削除します
--------------

たたき台としてどのツールを使うか検討：[このページ](https://github.com/haruosuz/qiime) の QIIME in Galaxy

Pitagora 0.3.2 に Tool Shed の [package_python_2_7_qiime_1_9_1](https://toolshed.g2.bx.psu.edu/repository/view_repository?sort=name&operation=view_or_manage_repository&f-free-text-search=qiime&id=6b9feb1f20b7ee53) のインストール

インストール前にこの作業が必要

`$ sudo apt-get install pkgconfig`

Pandas のインストールでエラーが出ている

`'latin-1' codec can't encode character u'\u2018' in position 1794: ordinal not in range(256)`