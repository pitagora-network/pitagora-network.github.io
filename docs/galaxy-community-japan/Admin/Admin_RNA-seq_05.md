---
title: Admin RNA-seq 05
permalink: /Admin_RNA-seq_05/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Admin](/Admin "wikilink") &gt;&gt;

インストール手順
----------------

これら3つのツールを使用する

-   Trinity (version 0.0.1)
    -   <http://toolshed.g2.bx.psu.edu/view/biomonika/trinityrnaseq>

<!-- -->

-   RSEM prepare reference (version 1.1.17)
    -   <http://toolshed.g2.bx.psu.edu/view/jjohnson/rsem>

<!-- -->

-   RSEM calculate expression (version 1.1.17)
    -   <http://toolshed.g2.bx.psu.edu/view/jjohnson/rsem>

インストールするレポジトリは2つ

既にツールのリストに記載： [ここ](https://docs.google.com/spreadsheets/d/1cfcYACIXXBHTa3TnYs6HuoWPlaapLFLzubCOgn-ZEMc/edit?usp=sharing)

### trinityrnaseq

-   biomonica さんの trinityrnaseq： [ここ](https://toolshed.g2.bx.psu.edu/repository?repository_id=8231d162d0d5693c&changeset_revision=39b85d32b0bf)
    -   bowtie（bowtie2 ではない）が必要だが、dependency に定義されていない。

そこで

-   anmoljh さんの trinityrnaseq： [ここ](https://toolshed.g2.bx.psu.edu/view/anmoljh/trinityrnaseq/0a576a57a9eb)
    -   trinity のバージョンが新しい？ (r20140717)
    -   bowtie と samtools も dependency に含まれている
    -   しかし、こいつはこいつで package_trinityrnaseq_r20140717 のインストールでエラー

`'latin-1' codec can't encode character u'\u2018' in position 30: ordinal not in range(256)`

### rsem

-   jjohnson さんの rsem
    -   rsem ではこのツールが最新の模様。
    -   package_rsem_1_1_17 のインストールでエラー。NCURSES が必要。
    -   先の trinityrnaseq で使われていた、anmoljh さんの package_ncurses_5_9 を含めればよいか。

<!-- -->

    bam_tview.c:5:20: error: curses.h: No such file or directory
    bam_tview.c:7:2: warning: #warning "_CURSES_LIB=1 but NCURSES_VERSION not defined; tview is NOT compiled"
    bam_tview.c:434:2: warning: #warning "No curses library is available; tview is disabled."
    make[1]: *** [bam_tview.o] Error 1
    make: *** [build-sam] Error 2

テストデータ
------------