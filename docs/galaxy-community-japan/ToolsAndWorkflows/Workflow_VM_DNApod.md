---
title: Workflow VM DNApod
permalink: /Workflow_VM_DNApod/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

概要
----

DNApod: DNA Polymorphism annOtation Database

DNApod は、DDBJが運営する新型シーケンサ出力配列のアーカイブデータベースDDBJ Seuence Read Archive (DDBJ SRA) から、統一された解析手法でDNA多型を抽出し、既知の遺伝子注釈をつけたデータベースと、Webベースで同様の解析ができる解析ワークフローです。現在、植物に対応しています。

オリジナルのDNApodサイトは[こちら](http://tga.nig.ac.jp/dnapod)です。

DNApod ワークフローは、[DDBJ Read Annotation Pipeline 基礎処理部](https://p.ddbj.nig.ac.jp/pipeline/Login.do) + Pitagora-Galaxy Virtual Machine Image から構成されます。 ワークフローの詳細は、[こちら](https://sites.google.com/a/g.nig.ac.jp/dnapod-help/howto/workflow) を参照してください。

### 入力ファイル

-   VCF File: samtools mpileup にて作成されたVCFファイル

### 出力ファイル

-   SNP データ (VCF)
-   Insertion/Deletion データ (VCF）
-   DNA 多型 (SNP + InDel) データ (VCF)
-   ゲノム上の DNA 多型の分布の描画ファイル (html)
-   DNA 多型の既知遺伝子注釈情報ファイル (html, VCF)

フロー図
--------

[<File:Flowchart.001.png>](/File:Flowchart.001.png "wikilink")

テスト方法
----------

`テスト方法、テストデータは`[`こちら`](https://sites.google.com/a/g.nig.ac.jp/dnapod-help/howto/workflow)` をご参照ください。`