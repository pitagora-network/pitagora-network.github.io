
概要
----

MeGAPを用いて微生物メタゲノムのアノテーションを行う（※前半まで。CLUSTを用いたアライメント以降は未実装）

1.  **TestToolShed：** 「megap_pre（Owner:emihat）」をインストール
2.  **テストデータ：** 適当な微生物メタゲノムのFastqを「GetData」（例：[DRR018420](ftp://ftp.ddbj.nig.ac.jp/ddbj_database/dra/fastq/DRA002/DRA002222/DRX016646/DRR018420.fastq.bz2)）
3.  **アノテーションデータ：** [hg38](http://hgdownload.cse.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz)と[PhiX174](https://raw.githubusercontent.com/galaxyproject/tools-devteam/master/data_managers/data_manager_fetch_genome_all_fasta/test-data/phiX174.fasta)のリファレンス配列Fastaを「GetData」（※テストではhg38は[chr22](http://hgdownload.cse.ucsc.edu/goldenPath/hg38/chromosomes/chr22.fa.gz)のみ使用）
4.  **ワークフロー：** 以下のデータを指定して実行
    -   **Step1のInput dataset：** 2のテストデータを指定
    -   **Step2,3のInput dataset：** 3のアノテーションデータを指定

### 入力ファイル

fastq

### 出力ファイル

フロー図
--------

[<File:MeGAP> WF.png](/File:MeGAP_WF.png "wikilink")

テスト方法
----------