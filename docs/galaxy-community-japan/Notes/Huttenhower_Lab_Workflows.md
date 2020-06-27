
[Huttenhower Lab](https://galaxyproject.org/public-galaxy-servers/#huttenhower-lab)は、メタゲノム・機能ゲノム解析のためのGalaxyサーバー。

<http://huttenhower.org/galaxy/> <https://huttenhower.sph.harvard.edu/galaxy/>

------------------------------------------------------------------------

[Huttenhower Lab Tools](https://bitbucket.org/biobakery/biobakery/wiki/Home)
----------------------------------------------------------------------------

**Composition Analysis**

-   MetaPhlAn2
-   PhyloPhlAn
-   PICRUSt: Phylogenetic Investigation of Communities by Reconstruction of Unobserved States

**Statistical Analysis**

-   LEfSe (Linear discriminant analysis Effect Size)
-   MaAsLin
-   microPITA (microbiomes: Picking Interesting Taxonomic Abundance)
-   SparseDOSSA

[Segata Lab - Computational Metagenomics](http://segatalab.cibio.unitn.it/tools/)

------------------------------------------------------------------------

[MetaPhlAn2 tutorial | Visualize results | Create a cladogram with GraPhlAn](https://bitbucket.org/biobakery/biobakery/wiki/metaphlan2#rst-header-create-a-cladogram-with-graphlan)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

[MetaPhlAn2](http://huttenhower.sph.harvard.edu/metaphlan2)は、メタゲノム配列データから微生物群集の組成を推定する。

[biobakery / MetaPhlAn2 — Bitbucket](https://bitbucket.org/biobakery/metaphlan2)

[GraPhlAn in Galaxy](http://wiki.pitagora-galaxy.org/wiki/index.php/Huttenhower_Lab_Workflows#GraPhlAn_.28Galaxy_module.29)

------------------------------------------------------------------------

[PhyloPhlAn Tutorial | 1.2 Visualization with GraPhlAn (Galaxy module)](https://bitbucket.org/biobakery/biobakery/wiki/phylophlan#rst-header-visualization-with-graphlan-galaxy-module)
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

PhyloPhlAnは、全ゲノム配列情報に基づいて系統樹を再構築する。

You may use tree visualization softwares to inspect the files. We provide support for GraPhlAn to visualize the output tree. Please follow the instructions below to view the resulting tree. Go to the Huttenhower Galaxy Server. Click on the GraPhlAn link on the left pane. Click on the Load input tree link on the left pane under the GraPhlAn link, and upload the PhyloXML file (for this tutorial example_corynebacteria.tree.reroot.xml). You can do this by clicking on the Browse button, selecting the PhyloXML file, and then pressing the Execute button.

<https://bitbucket.org/repo/49y6o9/images/3667087422-phylophlan1.png>

------------------------------------------------------------------------

[PICRUSt](http://picrust.github.io/picrust/)
--------------------------------------------

PICRUStは、（16S rRNA等）マーカー遺伝子の調査と完全ゲノムからメタゲノムの機能を予測する。

-   [Use online Galaxy version](http://huttenhower.sph.harvard.edu/galaxy/root?tool_id=PICRUSt_normalize)

\[Installing PICRUSt in Galaxy <https://github.com/picrust/picrust_galaxy>\]

-   [Amplicon functional profiling with PICRUSt | Curtis Huttenhower 08-15-14](https://stamps.mbl.edu/images/2/25/Stamps05-lab4-pdf.pdf) PICRUSt in Galaxy

<!-- -->

-   [PICRUSt: Phylogenetic Investigation of Communities by Reconstruction of Unobserved States — PICRUSt 1.0.0-dev documentation](http://picrust.github.io/picrust/)

------------------------------------------------------------------------

[MetaPhlAn (Galaxy Module)](https://bitbucket.org/biobakery/biobakery/wiki/metaphlan#rst-header-metaphlan-galaxy-module)
------------------------------------------------------------------------------------------------------------------------

[MetaPhlAn](http://huttenhower.sph.harvard.edu/metaphlan)は、メタゲノム配列データから微生物群集の組成を推定する。

-   [Huttenhower's Galaxy server](http://huttenhower.sph.harvard.edu/galaxy)に行く。
-   左側メニューの\[Get Data\] -&gt; \[Upload File\]をクリック。\[URL/Text\]テキストボックスにデータのリンク (https://bitbucket.org/nsegata/metaphlan/wiki/LC1.fna) をペーストし、\[Execute\]ボタンをクリックしてファイルをアップロード。
-   左側メニューの\[MetaPhlAn\] -&gt; \[MetaPhlAn\] metagenomic profiler をクリックし、\[Input metagenome\]ドロップダウン・メニューからアップロードしたデータ (https://bitbucket.org/nsegata/metaphlan/wiki/LC1.fna) を選び、\[Execute\]を押す。
-   完了すると、画面右側パネルの結果を示すアイコンが緑色に変わる。データ（1列目は微生物の分類群、2列目は微生物の存在量）をダウンロードするには保存ボタンをクリック。

<https://bitbucket.org/repo/49y6o9/images/1284509036-metaphlan2_out.png>

------------------------------------------------------------------------

### [MetaPhlAn Visualization | Using the GraPhlAn Galaxy module](https://bitbucket.org/biobakery/biobakery/wiki/metaphlan#rst-header-using-the-graphlan-galaxy-module)

-   [Huttenhower Galaxy server](http://huttenhower.sph.harvard.edu/galaxy)に行く。
-   左側メニューの \[Get Data\] -&gt; \[Upload File\] をクリック
    -   \[File Format\]ドロップダウン・メニューから circl を選択
    -   \[File\]でtreeファイル\[merged_table.tree\]を選択
    -   \[Execute\]ボタンをクリックしてファイルをアップロード。

??????????

1.  Issue \# It is stated that '• Select the input tree file (merged_table.tree. for this tutorial)' at <https://bitbucket.org/biobakery/biobakery/wiki/metaphlan#rst-header-using-the-graphlan-galaxy-module>

However, the input tree file “merged_table.tree” was not found..... Also, the filename is “tmp.tree” at <https://bitbucket.org/repo/49y6o9/images/3445894745-graphlan_load.png> ??????????

------------------------------------------------------------------------

[GraPhlAn (Galaxy module)](https://bitbucket.org/biobakery/biobakery/wiki/graphlan#rst-header-graphlan-galaxy-module)
---------------------------------------------------------------------------------------------------------------------

[GraPhlAn](https://huttenhower.sph.harvard.edu/graphlan)は、分類学的・系統学的ツリーを環状で表現する。

以下のデモ用ファイルをダウンロードする:

-   ツリー・ファイル: [guide.txt](https://bytebucket.org/nsegata/graphlan/wiki/guide.txt)
-   注釈ファイル: [guide_rings.txt](https://bytebucket.org/nsegata/graphlan/wiki/guide_rings.txt)

GraPhlAn Galaxyを使用する手順は以下の通り

**ステップ1**: [Huttenhower Galaxy server](http://huttenhower.sph.harvard.edu/galaxy)に行く。

**ステップ2**: ツリー・ファイルをアップロード

-   左側メニューの \[Get Data\] -&gt; \[Upload File\] をクリック
-   \[File Format\]ドロップダウン・メニューから circl を選択
-   \[File\]でダウンロードしたツリー・ファイル\[guide.txt\]を選択
-   \[Execute\]ボタンをクリック

<https://bitbucket.org/repo/49y6o9/images/4218449326-Screenshot%20from%202016-07-11%2015-06-50.png>

**ステップ3**: ツリーに注釈を付ける。

-   左側メニューの\[GraPhlAn\] -&gt; \[Annotate tree\]をクリック
-   \[Input file\]で、アップロードしたツリー・ファイル\[guide.txt\]を選択
-   \[Select Clade(s)\]リストから図に表示させたいクレード\[Bacillaceae\]を選択
-   \[Annotation Label\]テキストボックスに\[\*\]を入力
-   \[Annotation Label Clade Selector\]ドロップダウン・メニューから\[Clade and its leaf nodes\]を選択
-   \[Execute\]ボタンをクリック

<https://bitbucket.org/repo/49y6o9/images/1693804870-Screenshot%20from%202016-07-11%2015-24-32.png>

**ステップ4**: 注釈ファイルをアップロード

-   左側メニューの\[Get Data\] -&gt; \[Upload File\]をクリック
-   \[File\]で、ダウンロードしたannotationファイル\[guide_rings.txt\]を選択
-   \[Execute\]ボタンをクリックしてファイルをアップロード

<https://bitbucket.org/repo/49y6o9/images/2026880093-Screenshot%20from%202016-07-11%2015-38-52.png>

**ステップ5**: ツリーにリングを追加

-   左側メニューの \[GraPhlAn\] -&gt; \[Add rings to the tree\] をクリック
-   \[Input Tree\]ドロップダウン・メニューから（前のステップで生成された）注釈付きツリーを選択
-   \[Ring Input File\]ドロップダウン・メニューからアップロードした注釈ファイル\[guide_rings.txt\]を選択
-   \[Execute\]ボタンをクリック

<https://bitbucket.org/repo/49y6o9/images/1582742557-Screenshot%20from%202016-07-11%2015-46-15.png>

**ステップ6**: ツリーをプロット

-   左側メニューの \[GraPhlAn\] -&gt; \[Plot tree\] をクリック
-   （前のステップで生成された）リング付きツリーを選択
-   \[Execute\]ボタンをクリック

<https://bitbucket.org/repo/49y6o9/images/3764378899-Screenshot%20from%202016-07-11%2015-59-10.png>

結果として得られる画像を以下に示す:

<https://bitbucket.org/repo/49y6o9/images/510538251-Screenshot%20from%202016-07-11%2016-06-39.png>

GraPhlAnの他のオプションについては、[GraPhlAnのドキュメント](https://bitbucket.org/nsegata/graphlan/wiki/Home)を参照されたい。

------------------------------------------------------------------------

[LEfSe (Galaxy)](https://bitbucket.org/biobakery/biobakery/wiki/lefse#rst-header-lefse-galaxy)
----------------------------------------------------------------------------------------------

LEfSe (Linear discriminant analysis Effect Size) は、クラス間の違いを説明する可能性が最も高い 特徴（生物、クレード、OTU、遺伝子、機能）を決定する。

<http://huttenhower.sph.harvard.edu/webfm_send/129>

??????????

1.  Issue \# It is stated that “click on the LEfSe link on the left pane” and “Click on the Load data link to upload the input file” at <https://bitbucket.org/biobakery/biobakery/wiki/lefse#rst-header-lefse-galaxy>

However, \[LEfSe\] -&gt; \[Load data\] was not found at <http://huttenhower.sph.harvard.edu/galaxy> ??????????

<https://bitbucket.org/repo/49y6o9/images/3931070337-load_lefse_graphlan.png>

[Meta’omic taxonomic profiling with MetaPhlAn2 and biomarker discovery with LEfSe | Curtis Huttenhower 08-15-14](https://stamps.mbl.edu/images/c/c2/Stamps02-lab1-pdf.pdf)

[Meta’omic Analysis with MetaPhlAn & LEfSe | Eric Franzosa 17 February 2013](http://www.cs.utexas.edu/users/tandy/franzosa-workshop.pdf)

------------------------------------------------------------------------

[MaAsLin (Galaxy module)](https://bitbucket.org/biobakery/biobakery/wiki/maaslin#rst-header-maaslin-galaxy-module)

MaAsLinは、臨床メタデータと実験データとの間の関連を発見する。

MaAsLin requires an input file of microbial abundance table with metadata attached (e.g.: sample input). Go to the Huttenhower Galaxy server. Click on the MaAsLin link on the left pane, and then on the Load data for MaAsLin link. Click on the Choose File button to select the input file (microbial abundance table with metadata data attached) OR enter the URL in the URL/Text text box to upload the data. Click on the Execute button.

------------------------------------------------------------------------

[microPITA (Galaxy module)](https://bitbucket.org/biobakery/biobakery/wiki/micropita#rst-header-micropita-galaxy-module)

microPITA (microbiomes: Picking Interesting Taxonomic Abundance) は、2段階（階層）の研究でサンプルの選択を可能にする。

1. microPITA (Galaxy module) microPITA requires a PCL or BIOM input file. For further information on details of format, please refer to the documentation. For the purpose of this tutorial we will be using a sample input file depicting the general format of the input file. Go to the Huttenhower Galaxy server. Click on the microPITA link on the left pane, and then on the Load data for microPITA link. Click on the Choose File button to select the input file OR enter the URL in the URL/Text text box to upload the data. Click on the Execute button.

------------------------------------------------------------------------

[SparseDOSSA Galaxy](https://bitbucket.org/biobakery/biobakery/wiki/SparseDOSSA#rst-header-sparsedossa-galaxy)
--------------------------------------------------------------------------------------------------------------

Nothing is written..

------------------------------------------------------------------------

-   [How to make uploaded data available in Huttenhower galaxy](https://biostar.usegalaxy.org/p/15837/)
