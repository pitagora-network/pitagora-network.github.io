
概要
----

本ワークフローは、NGSから出力されたfastqデータをLAST、BWAの２つのアライメントツールでそれぞれマッピングを行ってBAMファイルを生成し、それぞれのBAMファイルからGATK、もしくはsomatic valiant caller(VarScan)にてバリアントをコールする、というものです。 特に、somatic valiant caller(VarScan)により、正常細胞とがん細胞の２つのfastqデータを比較することでがん細胞特有の変異を見つけ出すことに重点が置かれております。

また、その上で、ツールごと（LASTとBWA）の結果を比較等することにより、アライメントツールの差異による変異コールの違い等を作業者の手元で調べることを可能にするということを目的の一つにしております。

**尚、本解析フローは、かずさDNA研究所小原收先生よりご提供頂いており、そもそもは同研究所の皆様、及び土方敦司先生（現長浜バイオ大）により開発されました。** 実際の実装は、産総研により行われております。

### 入力ファイル

fastqファイル(NGSデータ)

通常細胞とがん細胞を比較してsomatic変異をコールする場合は、それぞれのfastqファイルが必要

リファレンス配列のfastaファイル

NGS試薬のBEDファイル

### 出力ファイル

リキャリブレーションされたBAMファイル

GATKでコールされた変異のvcfファイル

VarScanでコールされた変異のデータファイル（VarScanの各形式）

フロー図
--------

-   fastq→BAM

[<File:GWT2015> 1.png](/File:GWT2015_1.png "wikilink")

※実際の中身はこんな風になっています（LAST）

[<File:P1.png>](/File:P1.png "wikilink")

-   BAM→Recaliblated BAM

[<File:GWT2015_2.png>](/File:GWT2015_2.png "wikilink")

※実際の中身はこんな風になっています

[<File:P2.png>](/File:P2.png "wikilink")

-   Somatic Variant Caller

[<File:GWT2015> 3.png](/File:GWT2015_3.png "wikilink")

※実際の中身はこんな風になっています(VarScan)

[<File:P3.png>](/File:P3.png "wikilink")

テスト方法
----------

後日詳細説明予定