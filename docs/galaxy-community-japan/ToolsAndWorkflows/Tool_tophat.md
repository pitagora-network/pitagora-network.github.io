
-   TopHatはRNA-seqで得られたリード・データ用の高速なスプライス・ジャンクション・マッパーです。超ハイスループットなショート・リード・アライナーであるBowtieを使ってRNA-seqのリードを哺乳類サイズのゲノムにマップした後、エクソンの間のスプライス・ジャンクションを見つけるためにマッピング結果を解析します。
-   [TopHatのウェブサイト](http://ccb.jhu.edu/software/tophat/index.shtml)

**入力ファイル**

-   シングルエンドの場合：FASTQ File
-   ペアエンドの場合：FASTQ File（フォワード側）、FASTQ File（リバース側）

**出力ファイル**

-   マッピング情報（txt, bed）
-   マップ済みリード（bam）
