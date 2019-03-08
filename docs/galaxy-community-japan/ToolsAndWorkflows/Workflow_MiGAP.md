---
title: Workflow MiGAP
permalink: /Workflow_MiGAP/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

<s>2016.4.22現在、-remote をつけるblastが動かない</s>
那須野さんから、Galaxyでdockerを呼ぶ時にオプション --net none がつくことを教えていただき （通常のdockerコマンドのデフォルトは --net bridge）、 これをbridgeに変更すべくjob_conf.xmlに&lt;param id="docker_net"&gt;bridge&lt;/param&gt;をつけたところ、 blastが-remoteで実行できるようになりました

概要
----

MiGAPを用いて微生物ゲノムのアノテーションを行う

1.  **TestToolShed：** 「migap（Owner:emihat）」をインストール
2.  **テストデータ：** 適当な微生物ゲノムのFastaを「GetData」（例：NC_009925）
3.  **アノテーションデータ：** 以下のCOGデータを「GetData」
    -   <ftp://ftp.ncbi.nih.gov/pub/COG/COG/myva>
    -   <ftp://ftp.ncbi.nih.gov/pub/COG/COG/whog>
    -   <ftp://ftp.ncbi.nih.gov/pub/COG/COG/myva=gb>
    -   myvaはfasta、myva=gbとwhogはtabular。フォーマットAuto detectだとmyva=gbがgg（genomegraph?）、whogがJSONと判定されるので適宜直しておく（※ggはそのままでも動いた）。
4.  **ワークフロー実行：** 以下のデータを指定して実行
    -   **Step1のInput dataset：**　2のFastaを指定
    -   **Step2のInput dataset：**　3のCOG myvaを指定
    -   **Step3のInput dataset：**　3のCOG whogを指定
    -   **Step4のInput dataset：**　3のCOG myva=gbを指定

### 注意

1.  RefSeq search(-remoteでNCBIに対してblastp)で以下のエラーが出た。NCBIサーバの問題？再試行中

`Error: Error: internal_error: (Severe Error) Blast search error: Details: search failed. # Informational Message: [blastsrv4.REAL]: Error: CPU usage limit was exceeded, resulting in SIGXCPU (24).  `

Biostartで似た報告あり→https://www.biostars.org/p/106819/

1.  COG search(手元に用意したCOG myva(fasta)に対してblastp)がNC_009925をクエリとして実行すると12時間程度経ったのちKillされる（2回実行して2回とも）。小さいクエリだと正常終了する。原因不明。makedbしておかないと不安定なのか？

### 入力ファイル

微生物ゲノムのFasta

### 出力ファイル

Genbank, Emblフォーマットなどでアノテーション結果が出る

フロー図
--------

[<File:MiGAP_WF.png>](/File:MiGAP_WF.png "wikilink")

テスト方法
----------