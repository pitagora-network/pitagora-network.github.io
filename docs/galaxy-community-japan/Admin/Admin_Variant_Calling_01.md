---
title: Admin Variant Calling 01
permalink: /Admin_Variant_Calling_01/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Admin](/Admin "wikilink") &gt;&gt;

既存ワークフローまとめ
----------------------

1.  RNAseq -- これは RNA-seq 01 と同様なので考慮しない
2.  KazusaWF -- BWA を実行するだけのWFなので気にしない
3.  LAST(Single) -- Lastal + maf convert + sam-to-bam + samtools sort
4.  LAST(Paired) -- LAST(Single) の途中で LAST pair probs というのが入る
5.  GATKs -- GATK Realignment + Picard 重複排除 + GATK Recalibration（重複排除では先でもいい気がする）
6.  GATK Variants -- GATK Genotyping + GATK Select Variants + SnpShift Annotation
7.  VarScan -- MPileup + VarScan + ProcessSomatic（**って何？自作のフィルタリング？**）

今後の作業
----------

1.  まず、入力を BAM にして Variant 検出フローのどちらか（5+6 or 7）を動かす。とりあえず、GATK = 5+6 を先にする
    -   Picard は ToolShed のものを使う（picard）
    -   GATK は**自作のものを使う？**（ToolShed にあるのは Version 1.4 / 2 対応。自作は確か 1.6 だったはず）
    -   SnpSift は ToolShed のものを使う（snpsift **リストには snpeff パッケージもあるけれど使っている？**）
2.  次に、BAM vs LAST の部分を作る。Lastal と maf convert は新規ツールが必要
    -   LAST のツールについては**齋藤さんと共有できる部分があるか確認**
3.  これらをつなげて実行できるようにする
4.  残りの Variant 検出フロー VarScan = 7 を同様に動かして、Mapper とつなげて実行できるようにする
    -   VarScan は ToolShed のものを使う（varscan_version2 / varscan_wrapper **リストに両方あるがどっちを使っている？**）
    -   MPileup は ToolShed のものを使う（samtools_mpileup）

作業ログ
--------

-   lastのパイプラインが動作するか確認(20151022)
    -   テストデータでは主要部分（lastal→maf-convert-sam-to-bam）が動作するのを確認。実データだとメモリサイズの関係からローカルだと厳しい。（メモリ３２Gのマシン上の仮想環境で落ちる）
        -   lastdbをwオプションで変更したもので試すべきか。
    -   samtools sortでは調達版時のツールではうまくいかない。原因不明。tool-shedのツールで処理が可能なのを確認。
        -   もちろんその処理結果が下流の処理で使えるものかどうかは要確認。
        -   tool-shed化については、今後のピタゴラギャラクシーがdockerに移行する可能性があるため、そこの部分の方針が定まってから実作業の予定。
-   gatkツールのtool-shed化作業関連(20151022)
    -   既に大体のツールはtool-shed化済みなので、パイプライン（gatkリキャリブレーション）に必要なものを補っていく方向で。
        -   （tool-shed化済み：GATK Table Recalibration　GATK Realigner Target Creator　GATK Unified Genotyper　GATK Count Covariate　GATK Combine Variants　GATK Select Variants ）
    -   具体的な作業
        1.  tool-shed化済みの/home/galaxy/shed_tools/testtoolshed.g2.bx.psu.edu/repos/pitagora/gatk_1_6_pitagora/04a1efe6021e/gatk_1_6_pitagora/tool_dependencies.xmlを調達版のディレクトリにコピー
        2.  tool-shed化済みの各ツールのxmlのうち、<requirements></requirements>の部分を調達版に組み入れる。
        3.  調達版の.pyについてはGATKの場所がベタ打ちなので、そこを削っていく。
