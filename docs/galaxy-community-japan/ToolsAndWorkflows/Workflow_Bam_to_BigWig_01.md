---
title: Workflow Bam to BigWig 01
permalink: /Workflow_Bam_to_BigWig_01/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

概要
----

BAM から BigWig を作成する。この際、各ポジションのリード数を RPM (reads per million: 合計リード数/100万) で割った値をスコアとする。

フロー
------

-   NGS: SAMtools &gt; Flagstat tabulate descriptive stats for BAM datset
    -   BAM ファイルの合計リード数とマップされたリード数を出力します。
-   手作業: RPM (read per million: 合計リード数/100万) の逆数 (100万/合計リード数) を計算しておきます。
    -   合計リード数が 200万リードであれば、RPM の逆数は 0.5 となります。
-   BEDTools &gt; Create a BedGraph of genome coverage
    -   BAM ファイルから各領域のカバレッジをスコアとする BedGraph ファイルを作成します。
    -   この際、RPMで補正をかけるためには、“Scale the coverage by a constant factor” に RPM の逆数を指定します。
-   Convert Formats &gt; Wig/BedGraph-to-bigWig converter
    -   IGV で表示するためには bigWig ファイルが必要なので、BedGraph ファイルから bigWig ファイルを作成します。
    -   bigWig はバイナリ形式なのでテキストとして中身を表示できませんが、サイズが小さいという利点があります。
