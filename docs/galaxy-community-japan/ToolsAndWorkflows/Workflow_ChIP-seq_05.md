---
title: Workflow ChIP-seq 05
permalink: /Workflow_ChIP-seq_05/
---

ワークフロー概要
----------------

-   count_reads： BAM と TSS リスト（TSS に限らず、注目している染色体位置のリスト）を入力として、各 TSS の周辺のリード数を Window 毎に合計して出力する。各 TSS の周辺のリード数を見たい場合には、出力された BedGraph ファイルからその TSS を選び出し、IGV で表示することができる。

これと同時に、TSS のリストをまとめてグラフ化（ヒートマップや平均値の線グラフ）する要件もあるので、以下の手順を実行する。

-   ソート： TSS ごとにリード数が最も多いウィンドウの値を調べておき、その値でソートする。（これはヒストグラムのため）
    -   awk： {max=$6; for(i=7;i&lt;=26;i++) if($i&gt;max) max=$i; print max,$0}
    -   distinct 処理： SQL を使う？
    -   sort： 1行目でソート
    -   cut： 1行目を削除

awk で max のカラムを作った後、SQL 一発で処理する場合：

    SELECT DISTINCT
      *
    FROM (
      SELECT
        c2 || c3 || c4 AS id
      , c7, c8, c9, c10, c11, c12, c13, c14
      , c15, c16, c17, c18, c19, c20, c21
      , c22, c23, c24, c25, c26, c27
      FROM
        t1
      ORDER BY c1 DESC
      LIMIT 100
    )

-   ヒートマップ： 縦軸を TSS リスト、横軸を Window としたヒートマップを描画する。

<!-- -->

-   平均値のグラフ： Window ごとに全ての TSS リストの値を平均して折れ線グラフを描画する。

仕様詳細
--------

### Window の位置

各 TSS について、-50~50 bp には −250~250 bp に始まりがあるリードの数が表示される。

100 ずつスライドの場合は、50~150 bp には −150~350 bp のリード数、といった要領。

### BedGraph ファイルの出力

ZIP されたディレクトリが出力される。ここには TSS 毎に別々の bedgraph ファイルが入っていて、それらはファイルサイズが小さいので bigwig に変換することなく IGV で読み込める。

ファイルを別々にしない場合、領域が重なっている別の TSS のウィンドウをどう扱うかという問題が生じてしまうので、このようにした。

開発
----

### BAM Index の読み込み

BAM はアップロード時にバックグラウンドで BAM Index が作成されるので、その BAM Index を使えるようにする

XML でこのように記述しておくと、スクリプトで input.bam と input.bai をそれぞれ参照できるようになる。

参照： <https://biostar.usegalaxy.org/p/10128/>

shell の場合はこう書いてもよいが：

      <command interpreter="shell">
        <![CDATA[
          ln -s $input1 input.bam  &&
          ln -s $input1.metadata.bam_index input.bai &&
          ..
        ]]>
      </command>

perl の場合はこんな感じにしておいて：

      <command interpreter="perl">
        count_reads.pl $bam_file $bam_file.metadata.bam_index ..
      </command>

perl スクリプトでシンボリックリンクを作成する：

    system "ln -s $bam $tmp";
    system "ln -s $bai $tmp.bai";