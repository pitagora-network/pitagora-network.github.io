---
title: VirAmp Workflows
permalink: /VirAmp_Workflows/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

[VirAmp](https://wiki.galaxyproject.org/PublicGalaxyServers#VirAmp)は、ウイルス ゲノム アセンブリと変動発見のためのGalaxyサーバー。

Introduction
============

パイプラインの概要

<http://docs.viramp.com/en/latest/_images/pipeline_overview.png>

データ
======

-   [VirAmp](http://viramp.com/root)にアクセスし、画面上部の \[Shared Data\] -&gt; \[Data Libraries\] をクリックし、

<http://viramp.com/library> Data library name の[HSV_sample_datasets](http://viramp.com/library/browse_libraries?sort=name&f-description=All&f-name=All&operation=browse&id=66481f6a6bed8042)をクリック。 Name の下記項目に✔️を付ける。

`250000reads_1.fastq`
`250000reads_2.fastq`
`HSV-reference.fasta    `

画面下部の“For selected datasets:”のプルダウンメニューで\[Import to current history\]を選び、\[Go\]ボタンを押す。

Usage
=====

[One-click pipeline](http://docs.viramp.com/en/latest/viramp_intro.html#one-click-pipeline)
-------------------------------------------------------------------------------------------

ワンクリックで、ペアエンド・データとシングルエンド・データを解析するパイプラインを提供。 画面左側の“Tools”における\[Entire Pipeline\]内の\[Paired-end pipeline\]か\[Single-end pipeline\]をクリック

<http://docs.viramp.com/en/latest/_images/paired_end_pipeline.png>

------------------------------------------------------------------------

[Quality Control](http://docs.viramp.com/en/latest/viramp_intro.html#quality-control)
-------------------------------------------------------------------------------------

まず、入力[FASTQ](https://ja.wikipedia.org/wiki/Fastq)ファイルのクオリティの低い塩基をトリミングする。 画面左側の“Tools”における\[VirAmp\]内のQUALITY CONTROL下の\[Trim Sequence by Quality\]か\[Trim Sequence by Bases\]をクリック

<http://docs.viramp.com/en/latest/_images/trim_qual.png>

[Diginorm](http://docs.viramp.com/en/latest/viramp_intro.html#diginorm)
-----------------------------------------------------------------------

次に、[Digital normalization](http://ged.msu.edu/papers/2012-diginorm/)を使用して、サンプルのバイアスと変動を低減する。 画面左側の“Tools”における\[VirAmp\]内のDIGINORM下の\[Reduce the coverage\]をクリック

<http://docs.viramp.com/en/latest/_images/diginorm.png>

[de novo Contig assembly](http://docs.viramp.com/en/latest/viramp_intro.html#de-novo-contig-assembly)
-----------------------------------------------------------------------------------------------------

短いリード配列を長いコンティグにアセンブルする。 画面左側の“Tools”における\[VirAmp\]内の“DE NOVO CONTIG ASSEMBLING”下のツール（\[Velvet\]か\[SPAdes\]か\[VICUNA\]）をクリック

<http://docs.viramp.com/en/latest/_images/de-novo.png>

[Reference-based scaffolding](http://docs.viramp.com/en/latest/viramp_intro.html#reference-based-scaffolding)
-------------------------------------------------------------------------------------------------------------

コンティグをアセンブルしてスーパー・コンティグにする（[AMOScmp](https://sourceforge.net/projects/amos/)）。 画面左側の“Tools”における\[VirAmp\]内の“SCAFFOLDING”下の\[Reference-guided Scaffolding\]をクリック

<http://docs.viramp.com/en/latest/_images/amoscmp.png>

[Reference-independent scaffolding](http://docs.viramp.com/en/latest/viramp_intro.html#reference-independent-scaffolding)
-------------------------------------------------------------------------------------------------------------------------

スーパー・コンティグを拡張し、SSPACEを使用して接続。 画面左側の“Tools”における\[VirAmp\]内の“SCAFFOLDING”下の\[Scaffolding pre-assembled contigs using paired-read data\]をクリック

<http://docs.viramp.com/en/latest/_images/sspace.png>

[Gap closing](http://docs.viramp.com/en/latest/viramp_intro.html#reference-independent-scaffolding)
---------------------------------------------------------------------------------------------------

\[ \]
-----

参考文献
========

-   VirAmp: a galaxy-based viral genome assembly pipeline
-   [PubMed - NCBI](http://www.ncbi.nlm.nih.gov/pubmed/25918639)
-   [Full text](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC4410580/pdf/13742_2015_Article_60.pdf)
-   [Galaxy](http://viramp.com/)
-   [Documentation](http://docs.viramp.com)
-   [VirAmp Documentation](https://media.readthedocs.org/pdf/viramp/latest/viramp.pdf)
-   [GitHub repository](http://github.com/SzparaLab/viramp-project)

<!-- -->

-   [Public Galaxy Servers - Galaxy Wiki](https://wiki.galaxyproject.org/PublicGalaxyServers#VirAmp)
-   [Workflows - Pitagora-Galaxy Wiki](http://wiki.pitagora-galaxy.org/wiki/index.php/Workflows)

<http://wiki.pitagora-galaxy.org/wiki/index.php/VirAmp_Workflows>