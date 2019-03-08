---
title: Workflow Variant Calling 02
permalink: /Workflow_Variant_Calling_02/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Workflows](/Workflows "wikilink") &gt;&gt;

概要
----

### 入力ファイル

-   BAM File: 対象データをBWAなどを用いてアライメントしたファイル（bamファイル）
-   BED File: ターゲットとする領域を記述したタブ区切りのテキストファイル（bedファイル）

### 出力ファイル

-   バリアントの情報(vcf)
-   既知のindexを考慮してのアライメント修正と、塩基配列に応じて塩基の精度情報を修正したアライメントファイル(BAM)
-   ツール実行時のログ

フロー図
--------

{{\#widget:Iframe |url=<http://wiki.pitagora-galaxy.org/workflows/details/VariantCalling_02.html> |width=400 |height=480 |border=0 }}

テスト方法
----------

1. Pitagora Tools &gt; Download References で Fasta UCSC hg19 をダウンロード

2. ツール GATK resource bundle downloader で hg19 のリソースバンドルを ダウンロード

3. GATK 3.3 を Broad Institute からダウンロード

-   こちらからダウンロード <https://www.broadinstitute.org/gatk/download/>
-   VMのホストとなっているマシンのコマンドライン上で、次のコマンドを実行して、ダウンロードしたファイルをuuencode。(次の内容を一行で記述)

`python -c 'import uu;uu.encode(`“`GenomeAnalysisTK-3.3-0.tar.bz2`”`,`“`gatk.uu`”`)'`

3-1. Get data -&gt; Upload File from your computer で、gatk.uuを Pitagora Galaxy に登録

3-2. GATK binary loader で　GATK.uu を選択して実行（この作業によりPitagora　galaxy上でGATKが動作するようになります。）

4. GATK variant callerにbam と bedファイルを与えて実行