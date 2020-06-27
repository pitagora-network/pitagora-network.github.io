
2017/07/05 手順
---------------

-   ToolShed に suite_qiime というレポジトリ（IUC、2017年6月更新）があるがインストールで Error になる
-   IUC の GitHub レポジトリの XML が使えるかどうか確かめる

Qiime のインストール

メモリが必要なので t2.medium で操作する

[`http://bio.cug.edu.cn/qiime/install/install.html#how-to-install-qiime`](http://bio.cug.edu.cn/qiime/install/install.html#how-to-install-qiime)
`pip install numpy==1.7.1`
`pip install qiime`

<http://greengenes.secondgenome.com/downloads/database/13_5>

`$ cd ~`
`$ git clone ..`
`$ git checkout qiime_add_on`
`$ cd ~/galaxy/tools`
`$ ln -s  ~/tools-iuc/qiime`

tool_data.xml に以下を追加

        <!-- Locations of qiime databases -->
        <table name="qiime_reference_db" comment_char="#">
            <columns>value, name, path</columns>
            <file path="tool-data/qiime_reference_db.loc" />
        </table>

tool_conf.xml に以下を追加

<section id="Qiime" name="Qiime">
`   `<tool file="qiime/qiime_core/align_seqs.xml" />
`   `<tool file="qiime/qiime_core/alpha_diversity.xml" />
`   `<tool file="qiime/qiime_core/alpha_rarefaction.xml" />
`   `<tool file="qiime/qiime_core/assign_taxonomy.xml" />
`   `<tool file="qiime/qiime_core/beta_diversity_through_plots.xml" />
`   `<tool file="qiime/qiime_core/beta_diversity.xml" />
`   `<tool file="qiime/qiime_core/compare_categories.xml" />
`   `<tool file="qiime/qiime_core/core_diversity_analyses.xml" />
`   `<tool file="qiime/qiime_core/count_seqs.xml" />
`   `<tool file="qiime/qiime_core/filter_alignment.xml" />
`   `<tool file="qiime/qiime_core/filter_fasta.xml" />
`   `<tool file="qiime/qiime_core/filter_otus_from_otu_table.xml" />
`   `<tool file="qiime/qiime_core/filter_samples_from_otu_table.xml" />
`   `<tool file="qiime/qiime_core/filter_taxa_from_otu_table.xml" />
`   `<tool file="qiime/qiime_core/jackknifed_beta_diversity.xml" />
`   `<tool file="qiime/qiime_core/macros.xml" />
`   `<tool file="qiime/qiime_core/make_otu_heatmap.xml" />
`   `<tool file="qiime/qiime_core/make_otu_table.xml" />
`   `<tool file="qiime/qiime_core/make_phylogeny.xml" />
`   `<tool file="qiime/qiime_core/pick_open_reference_otus.xml" />
`   `<tool file="qiime/qiime_core/pick_otus.xml" />
`   `<tool file="qiime/qiime_core/pick_rep_set.xml" />
`   `<tool file="qiime/qiime_core/plot_taxa_summary.xml" />
`   `<tool file="qiime/qiime_core/split_libraries_fastq.xml" />
`   `<tool file="qiime/qiime_core/split_libraries.xml" />
`   `<tool file="qiime/qiime_core/summarize_taxa_through_plots.xml" />
`   `<tool file="qiime/qiime_core/summarize_taxa.xml" />
`   `<tool file="qiime/qiime_core/upgma_cluster.xml" />
`   `<tool file="qiime/qiime_core/validate_mapping_file.xml" />
` `

</section>
テストデータをダウンロード

`wget `[`ftp://ftp.microbio.me/qiime/tutorial_files/moving_pictures_tutorial-1.9.0.tgz`](ftp://ftp.microbio.me/qiime/tutorial_files/moving_pictures_tutorial-1.9.0.tgz)