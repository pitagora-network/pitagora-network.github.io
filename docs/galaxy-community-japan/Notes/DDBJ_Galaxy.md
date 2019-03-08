---
title: DDBJ Galaxy
permalink: /DDBJ_Galaxy/
---

目次
----

1.  DDBJ Pipeline高次部のVM移植開発について
2.  VM化Galaxyワークフローリスト
3.  Pitagora VM移植情報まとめ

DDBJ Pipeline高次部のVM化開発について
-------------------------------------

    DDBJでは、NGS解析ウェブサービス「DDBJ Pipeline」を提供しています。
    DDBJ Pipeline高次部はPSUのgalaxy interfaceを用いて開発されており、遺伝研スパコン上のウェブサービス
    として提供して来ました。しかし高次部ユニーク利用者数は近年減少傾向にあります(2015年度DNAデータ利用
    研究委員会報告書、Pipeline利用統計(追加資料)参照)。そこで、ユニーク利用者数が増えている基礎部に
    計算機資源を絞り、高次部はPitagora galaxy VMをローカルPC環境で実行して頂くようウェブサービス形式から
    アプリケーション配布形式に移行致します
    [2015年12月DDBJ開発,文責 遺伝研神沼]。

-   DDBJ Pipeline (http://p.ddbj.nig.ac.jp/)
-   [基礎部と高次部の説明](https://sites.google.com/a/g.nig.ac.jp/pipeline_help/)
-   論文(Nagasaki et al,DNA Res, 20:383-390, 2013.; Kaminuma et al., Nucleic Acids Res, 38:D33-38, 2010.)　　

VM化Galaxyワークフローリスト
----------------------------

Pitagora galaxy/Toolshedでの識別用ワークフロー名と、DDBJ Pipeline基礎部との 接続有無、出力結果のDDBJ登録接続の有無などを紹介します。

<table>
<tbody>
<tr class="odd">
<td></td>
<td><p>Workflow Name</p></td>
<td><p>所属[制作者※]</p></td>
<td><p>説明</p></td>
<td><p>PL基礎部後スタート</p></td>
<td><p>DDBJ登録接続</p></td>
<td><p>VM配布時packing</p></td>
</tr>
<tr class="even">
<td><p>1</p></td>
<td><p>vm_denovoGA</p></td>
<td><p>DDBJ,<br />
NIG中村研[長崎]</p></td>
<td><p>Denovo genome annotation</p></td>
<td><p>o</p></td>
<td><p>開発中</p></td>
<td><p>VM-(1)</p></td>
</tr>
<tr class="odd">
<td><p>2</p></td>
<td><p>vm_denovoTA</p></td>
<td><p>DDBJ,<br />
NIG中村研[長崎]</p></td>
<td><p>Denovo transcriptome annotation</p></td>
<td><p>o</p></td>
<td><p>開発中</p></td>
<td><p>VM-(1)</p></td>
</tr>
<tr class="even">
<td><p>3</p></td>
<td><p>Prokka2DDBJ</p></td>
<td><p>DDBJ,<br />
NIG中村研[藤澤],<br />
NIG有田研[谷沢]</p></td>
<td><p>微生物ゲノム注釈解析</p></td>
<td><p>x</p></td>
<td><p>開発中</p></td>
<td><p>VM-(1)</p></td>
</tr>
<tr class="odd">
<td><p>4</p></td>
<td><p><a href="http://wiki.pitagora-galaxy.org/wiki/index.php/Workflow_VM_DNApod">vm_dnapod</a></p></td>
<td><p>NIG中村研[望月]</p></td>
<td><p>植物・微生物の系統間多型注釈解析</p></td>
<td><p>o</p></td>
<td><p>x</p></td>
<td><p>VM-(1)</p></td>
</tr>
<tr class="even">
<td><p>5</p></td>
<td><p>HLAworkflow</p></td>
<td><p>NIG井ノ上研[細道]</p></td>
<td><p>ヒトゲノムHLA領域Typing</p></td>
<td><p>x</p></td>
<td><p>x</p></td>
<td><p>VM-(2)</p></td>
</tr>
</tbody>
</table>

※制作者異動：長崎→かずさDNA研究所、細道→金沢大学

Pitagora VM移植情報まとめ
-------------------------

進捗は下記の通りです

1.  denovoGA_PL
2.  denovoTA_PL
3.  Prokka2DDBJ
4.  dnapod_PL
5.  HLAworkflow
