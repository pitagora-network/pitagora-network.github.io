
**概要**

-   MACS (version 1.3)
    -   HP：http://liulab.dfci.harvard.edu/MACS/
    -   文献：Zhang et al. Model-based Analysis of ChIP-Seq. Genome Biol, 2008, vol. 9 (9) pp. R137
    -   特徴：数100bpの領域にリード配列が濃縮しピークを形成するデータに対して、ピーク位置を補正し、ポアソンブンプに当てはめ有意なピーク領域を特定する計算アルゴリズム
    -   入力：bamファイル、eland exportファイル (illumina社の提供するマッピングツールCASAVAによって得られたシーケンスリードデータ)
    -   出力：peak bedファイル (有意なピーク領域データで、各列は染色体、開始、終了、名前、シグナル強度を表す)、summit bedファイル (有意なピーク領域におけるピークの頂上位置データ)、wigファイル (Igb、igvでシグナル波形を見るためのテキストファイル)
