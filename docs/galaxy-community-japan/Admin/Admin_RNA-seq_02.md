---
title: Admin RNA-seq 02
permalink: /Admin_RNA-seq_02/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Admin](/Admin "wikilink")

ピタゴラのVMとの接続が確認できたあとに、RNAseq-Sailfish WF の動作環境を構築するためにやったこと

１）yum update

２）galaxyのadminになる

３）Fastqmcf、FastQCがあるか確認し、なければToolShedからLatestをインストール

注意：g++が無いとFastqmcfがインストールに失敗します

４）sailfishのインストール ※Galaxy環境のターミナルで行う

・http://www.cs.cmu.edu/~ckingsf/software/sailfish/downloads.htmlから、バイナリ版をDL

・PATHとlibを環境変数に通す

`   >LD_LIBRARY_PATH=`<install Dir>`/Sailfish-0.6.3-Linux_x86-64/lib `

`   >PATH=$PATH:`<install Dir>`/Sailfish-0.6.3-Linux_x86-64/bin `

５）sailfish indexを実行する

▶ mouse mm10 Transcriptome の場合・・・

`   ・http://hgdownload.cse.ucsc.edu/goldenPath/mm10/bigZips/ とかからfastaを任意のDirにD`

`   ・sailfish index --force -t ＜任意のpath＞/refMrna.fa -o ＜任意のpath＞ --kmerSize 20`

`   ・locファイルを作成 →sailfish_indexes.loc`

`   ・locファイルの場所をtool_data_table_conf.xmlに追加する`

６）sailfishツールを配置

`   ・toolsの配下にディレクトリを作成し、xmlを配置`

`       `[`https://github.com/myoshimura080822/galaxy-mytools_sailfish.gitからclone`](https://github.com/myoshimura080822/galaxy-mytools_sailfish.gitからclone)

`   ・tool_data_table_conf.xmlでセットしたidと、上記xmlで記述されている名前が一致するよう修正する`

`   ・tool_conf.xmlにsailfishツールとして追加する`

７）WFをインポートする

`   ・ツールの名前を変えたりすると無効になるので、必ずedit画面で確認する。`

８）adapter/primer 配列、fastq配列をヒストリーにimportして、Run

＊＊＊＊＊＊＊＊＊＊＊＊＊＊＊

注意！！fastqmcfやsailfishで設定している実行時引数のパラメーターは、BiTでの値になっています。

追記 2015/03/04（山中）
-----------------------

-   sailfish ツールの Dependency 作りたいですね。（配布OK？）**--&gt; Test Tool Shed に既に dependency (iuc) が置いてある。これが使えるか確認（山中）**
-   sailfish_indexes.loc と tool_data_table_conf.xml の該当箇所いただけますか？ **--&gt; 共有（芳村）** --&gt; **メールにて共有（芳村）**
-   ツールは Git &gt; ToolShed にコピーしてOKですか？ **--&gt; OK、そうする（山中）**
-   ワークフローファイルもいただけますか？ **--&gt; 共有（芳村）** --&gt; **[Githubにあります](https://github.com/myoshimura080822/galaxy-mytools_sailfish/tree/master/workflow) （芳村）**
-   テスト実行用の adapter/primer 配列、fastq配列もいただけますか？ **--&gt; 確認後に共有（芳村）--&gt; FTPにアップしました（芳村）**
-   Index ファイルは上記の手順を参考に作る（山中）**--&gt; mm10のみ先にFTPにアップしました（芳村）**

追記 2015/03/27（山中）
-----------------------

### Tool

-   “sailfish_linux” という名前で Test Tool Shed にリポジトリを作成しました。
    -   <https://testtoolshed.g2.bx.psu.edu/view/pitagora/sailfish_linux/32a9ed1edef6>
-   ライセンス
    -   このコード自体にライセンスを付与していません
    -   芳村さんのコードの改変であることをコード上およびリポジトリのREADMEで明記しています。
-   ピタゴラ 0.2.2 のツールリストに FastqMCF と共に加えました。
    -   <https://docs.google.com/spreadsheets/d/114eKhwlDCUYVAHAMoz_QfaNJZHw2nCp-D5AoqOoAJXw/edit?usp=sharing>

### Tool Dependency

-   バイナリ展開するだけ Dependency Package を作成しました。
    -   <https://testtoolshed.g2.bx.psu.edu/view/pitagora/package_sailfish_linux_0_6_3/2c66da813a79>

<!-- -->

-   Wrapper XML に以下の修正を加えます。

<!-- -->

        <requirements>
            <requirement type="package" version="0.6.3">sailfish_linux</requirement>      <-- この行を修正
        </requirements>

-   同じディレクトリに tool_dependency.xml を作成します。

<!-- -->

    <?xml version="1.0"?>
    <tool_dependency>
      <package name="sailfish_linux" version="0.6.3">
            <repository changeset_revision="2c66da813a79" name="package_sailfish_linux_0_6_3" owner="pitagora" prior_installation_required="False" toolshed="http://testtoolshed.g2.bx.psu.edu" />
      </package>
      </package>
    </tool_dependency>

### Index

-   tool_data_table_conf.xml

<!-- -->

    <table name="sailfish_custom_indexes" comment_char="#">
        <columns>value, dbkey, name, path</columns>
        <file path="tool-data/sailfish_index.loc" />
    </table>

-   sailfish_index.loc
    -   他と揃えた表現にしたくて “mm10_refMrna” を “mm10” に変更しました。問題ないでしょうか。

`mm10    mm10    mouse/UCSC Dec.2011 (mm10)    /disk/reference/sailfish_index/mm10`

-   Download Reference ツール
    -   .tar.gz で固め直して、ここに置きました。
        -   <http://download.pitagora-galaxy.org/data/reference/hg10_sailfish.tar.gz> **hg19はまだです。共有頂けると嬉しいです（今は FTP 落としています）**
        -   <http://download.pitagora-galaxy.org/data/reference/mm10_sailfish.tar.gz>
    -   Download Reference ツールに “Sailfish Index UCSC hg19/mm10” を登録しました。
    -   /disk/reference/sailfish_index/mm10_refMrna に展開されます。

### Workflow

-   GitHub にある Galaxy-Workflow-SailfishWF_quant_exec.ga を Galaxy-Workflow-RNA-seq_02_v001.ga という名前で次のピタゴラに含めました。（名前が無機質ですみません。。。）
    -   <https://github.com/pitagora-galaxy/install-0.2.2/blob/master/workflows/Galaxy-Workflow-RNA-seq_02_v001.ga>

### テストデータ

-   まだ公開していません。**公開OKになったら教えて下さい**
    -   SRR....fastq 8個 および test_adapter.fa
