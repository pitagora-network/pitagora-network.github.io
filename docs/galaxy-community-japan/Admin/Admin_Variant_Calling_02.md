
-   Test Tool Shed -- tool_shed_test_1

Reference genome hg19 で動作させるためには下記の作業が必要です。1,2,3,の 順序は可換

1. pitagora tools でhg19のリファレンスをダウンロード

2. 本ツールのGATK resource bundle downloader でhg19のリソースバンドルを ダウンロード

3. GATK 3.3 を Broad Institute からダウンロード

`VMのホストとなっているマシンのコマンドライン上で、`
`次のコマンドを実行して、ダウンロードしたファイルをuu encode。`
`(下記は一行で記述)`

`python -c 'import uu;uu.encode(`“`GenomeAnalysisTK-3.3-0.tar.bz2`”`,`“`gatk.uu`”`)'`

3-1. Get data -&gt; Upload File from your computer で、gatk.uuを Pitagora Galaxy に登録

3-2 GATK binary loader で　GATK.uu を選択して実行（この作業によりPitagora　galaxy上でGATKが動作するようになります）

4. GATK variant callerにbam と bedファイルを与えて実行