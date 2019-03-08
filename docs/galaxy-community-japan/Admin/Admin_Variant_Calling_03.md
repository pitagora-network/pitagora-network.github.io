---
title: Admin Variant Calling 03
permalink: /Admin_Variant_Calling_03/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Admin](/Admin "wikilink") &gt;&gt;

インストール方法
----------------

<http://goo.gl/QzSfdm>
からHOSOMICHI_HLA_tools2内のファイルを入手

-   Galaxy-Workflow-imported__HLA2_Workflow_(alpha3).ga　はワークフロー
-   Seq-reads_for_TEST.tar.gz はサンプルのリード配列
-   To_disk-reference.tar.gz は解凍後 /disk/reference 下に置く。
-   To_galaxy-dist_tools.tar.gz は解凍後　galaxy-dist/tools 下に置く。

### パスの設定は hlaconf.pm

-   hlaconf.pm はgalaxy-dist/tools/workflow/hla2 の下に
-   hlaconf.pm の20行めまでにパス周り集中。要確認!

### tool_data_table_conf.xml

        <!-- for HOSOMICHI HLA tools, NOTE: "value" field is required.-->
        <table name="HOSOMICHI_HLA_TOOLS_DATA" comment_char="#">
            <columns>value, locus, ref, nuc, int</columns>
            <file path="tool-data/HOSOMICHI_HLA_TOOLS_DATA.loc" />
        </table>

### HOSOMICHI_HLA_TOOLS_DATA.loc

-   HOSOMICHI_HLA_TOOLS_DATA.loc はgalaxy-dist/tool-dataの下に

<!-- -->

    #All datas are into galaxy-refs/hosomichi directory.
    #Value  Locus   Reference(Bwa index)    Nucleotid   Intervals
    #value  locus   ref nuc int
    HLA-A   A   ref/HLA-A.fasta db/A_nuc_HAC.fasta  db/HLA-A_gene_start_end.intervals
    HLA-B   B   ref/HLA-B.fasta db/B_nuc_HAC.fasta  db/HLA-B_gene_start_end.intervals
    HLA-C   C   ref/HLA-C.fasta db/C_nuc_HAC.fasta  db/HLA-C_gene_start_end.intervals
    HLA-DPB1    DPB1    ref/HLA-DPB1.fasta  db/DPB1_nuc_HAC.fasta   db/HLA-DPB1_gene_start_end.intervals
    HLA-DQB1    DQB1    ref/HLA-DQB1.fasta  db/DQB1_nuc_HAC.fasta   db/HLA-DQB1_gene_start_end.intervals
    HLA-DRB1    DRB1    ref/HLA-DRB1.fasta  db/DRB1_nuc_HAC.fasta   db/HLA-DRB1_gene_start_end.intervals

### tool_conf.xml

-   tool_conf-HLA-Part.xml galaxy-dist下のtool_conf.xmlのHLAtoolsについての記載部分

<!-- -->

      <section name="HLA2 wf test" id="hla2_wf">
        <label text="HLA2 test" id="hla2"/>
           <tool file="workflow/hla2/fastq_quality_trimmerM.xml"/>
           <tool file="workflow/hla2/pairedread.xml"/>
           <tool file="workflow/hla2/bwa/bwa_wrapper.xml"/>
           <tool file="workflow/hla2/bwa/bwa4hla.xml"/>
           <tool file="workflow/hla2/samtools/samtools_HOSO.xml"/>
           <tool file="workflow/hla2/gatkFastaAltRefMaker.xml"/>
           <tool file="workflow/hla2/gatkUnifiedGenotyper.xml"/>
           <tool file="workflow/hla2/picardadd.xml"/>
           <tool file="workflow/hla2/bam2cds.xml"/>
           <tool file="workflow/hla2/rewritevcf.xml"/>
           <tool file="workflow/hla2/awkSam300.xml"/>
           <tool file="workflow/hla2/awkSam500.xml"/>
           <tool file="workflow/hla2/grepvariant.xml"/>
           <tool file="workflow/hla2/awk_cds_closest.xml"/>
           <tool file="workflow/hla2/awk_cds_perfect.xml"/>
           <tool file="workflow/hla2/blat_haplo.xml"/>
           <tool file="workflow/hla2/haplo.xml"/>
           <tool file="workflow/hla2/hoso_zip_archive.xml"/>
           <tool file="workflow/hla2/remove_minor_reads_samtools_sam.xml"/>
           <tool file="workflow/hla2/remove_minor_reads_samtools_bam.xml"/>
           <tool file="workflow/hla2/filter_hetero_reads.xml"/>
           <tool file="workflow/hla2/rmread3.xml"/>
           <tool file="workflow/hla2/hoso_param.xml"/>
      </section>