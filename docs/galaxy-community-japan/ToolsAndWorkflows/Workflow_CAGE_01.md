---
title: Workflow CAGE 01
permalink: /Workflow_CAGE_01/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

概要
----

CAGE 01は、 [FANTOM5](http://fantom.gsc.riken.jp/5/) プロジェクトにおいて利用され、ウェブサイト上で公開されているHeliscopeCAGE read alignmentのワークフローをGalaxy上に再現したものです。元になった情報は [FANTOM5のプロトコル](http://fantom.gsc.riken.jp/5/sstar/Protocols) に記載されています。

このワークフローは、情報学研究所、遺伝学研究所DDBJ、ライフサイエンス統合データベースセンターによるアカデミックインタークラウド環境のユースケースとして、理化学研究所 川路英哉ユニットリーダー、粕川雄也ユニットリーダーらのサポートの元で構築されたものです。アカデミックインタークラウドとGalaxyの構築についての詳細は、[Galaxy Workshop Tokyo 2015](http://wiki.pitagora-galaxy.org/wiki/index.php/Galaxy_Workshop_Tokyo_2015)における那須野 淳 (株式会社アスケイド) による講演をご覧ください。

-   STEP 01-1 : ゲノムインデックス配列作成 (delve index)
    -   HP : <http://fantom.gsc.riken.jp/software/>
    -   特徴 : マッピングのためのインデックス配列を作成します。
    -   入力 : リファレンスゲノム配列
    -   出力 : ゲノムインデックスファイル

<!-- -->

-   STEP 01-2 : rRNA配列除去 (rRNAdust)
    -   HP : <http://fantom.gsc.riken.jp/5/suppl/rRNAdust/>
    -   特徴 : 2ミスマッチまでのrRNA配列を除去します。
    -   入力 : シーケンスファイル (fastq), リファレンスrDNA配列 (fasta)
    -   出力 : フィルタされたシーケンスファイル (fastq)

<!-- -->

-   STEP 02 : マッピング (delve seed/align)
    -   HP : <http://fantom.gsc.riken.jp/software/>
    -   特徴 : マッピングとマッピングクオリティの算出を行います。
    -   入力 : シーケンスファイル (fastq), ゲノムインデックスファイル
    -   出力 : マッピングファイル (sam)

<!-- -->

-   STEP 03 : フォーマット変換 (samtools)
    -   HP : <http://samtools.sourceforge.net>
    -   特徴 : samをbamに変換します。
    -   入力 : マッピングファイル (sam)
    -   出力 : マッピングファイル (bam)

<!-- -->

-   STEP 04 : フィルタリング (samtools)
    -   HP : <http://samtools.sourceforge.net>
    -   特徴 : マッピングのクオリティでマッピング結果をフィルタリングします。
    -   入力 : マッピングファイル (bam)
    -   出力 : フィルタされたマッピングファイル (bam)

<!-- -->

-   STEP 05 : フォーマット変換 (bedtools)
    -   HP : <http://bedtools.readthedocs.org/en/latest/>
    -   特徴 : bamをbed形式のCTSSファイル (CAGE tag のマッピング開始位置とその個数が入ったファイル) に変換します。
    -   入力 : マッピングファイル (bam)
    -   出力 : CTSSファイル (bed)

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/Cage.html> |width=700 |height=950 |border=0 }}

テスト方法
----------