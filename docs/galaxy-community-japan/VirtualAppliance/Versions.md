
ダウンロード方法
----------------

[right](/File:Virtualbox-ova-128px.png "wikilink")

### 準備

[VirtualBox](https://www.virtualbox.org/) をインストールしてください。

### 仮想マシンの入手

以下から仮想マシンを入手します。

### 起動手順

1.  上記のリンクからOVAイメージをダウンロードします。
2.  VirtualBox マネージャーで、ファイル &gt; 仮想アプライアンスのインポート、を開きます。
3.  ダウンロードしたOVAイメージを選択し、設定を変更せずにインポートします。
4.  VirtualBox マネージャーで、インポートされたVMを右クリックし「起動」します。
5.  VMの起動後、ブラウザで <http://192.168.56.10/galaxy/> にアクセスします。

### 再起動方法

VirtualBox マネージャーで「閉じる」&gt;「ACPIシャットダウン」で終了した後、再度「起動」します。

### トラブルシューティング

-   上記手順の5でGalaxyにアクセスできない場合、VMを再起動してみてください。
-   それでも、アクセスできない場合、ネットワーク設定の問題が考えられますので、詳しい方に訊くことをオススメします。VirtuaBoxマネージャーで、VirtuaBox &gt; 環境設定 &gt; ネットワーク &gt; ホストオンリーアダプター &gt;「+」 アイコンをクリックし、 新しいホストオンリーネットワークを追加します。このネットワークのアドレスを「192.168.56.1 / 255.255.255.0」として、ピタゴラギャラクシーのアダプター2に設定します。

作成中の仮想マシン
------------------

-   Version 0.3.5
    -   **OS:** Ubuntu 16.04.3
    -   **OS ユーザー:** galaxy/galaxy
    -   **Python:** 2.7.11
    -   **Galaxy:** release_17.05
    -   **Tools:** [Tool List 0.3.2](https://docs.google.com/spreadsheets/d/1aVfXp5_dvt7I2EJ5LYeMH1Hzyx1ZAjv1QXwR5xmhT7M/edit?usp=sharing)
    -   **URL:** <http://192.168.56.10/galaxy/>

最新の仮想マシン
----------------

はじめて仮想マシンをインストールする方はこちらのガイドを参照してください。

-   [はじめよう Galaxy 仮想マシン](https://docs.google.com/document/d/15x92uQyS4lMQUUyTHyQqKz-6O4u64TQdQW9vcwvC4u0/edit?usp=sharing)

仮想マシンのイメージをこちらからダウンロードして下さい。

-   Version 0.3.4 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.3.4.ova))
    -   **OS:** Ubuntu 14.04.4
    -   **OS ユーザー:** ubuntu/ubuntu
    -   **Python:** 2.7.11
    -   **Galaxy:** release_17.05
    -   **Tools:** [Tool List 0.3.2](https://docs.google.com/spreadsheets/d/1aVfXp5_dvt7I2EJ5LYeMH1Hzyx1ZAjv1QXwR5xmhT7M/edit?usp=sharing)
    -   **URL:** <http://192.168.56.10/galaxy/>
    -   **Galaxy へのログイン:** admin@pitagora-galaxy.org / galaxy

過去の仮想マシン
----------------

### Version 0.3.x

-   Version 0.3.3 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.3.3.ova))
    -   **OS:** Ubuntu 14.04.4
    -   **OS ユーザー:** ubuntu/ubuntu
    -   **Python:** 2.7.11
    -   **Galaxy:** release_16.10
    -   **Tools:** [Tool List 0.3.2](https://docs.google.com/spreadsheets/d/1aVfXp5_dvt7I2EJ5LYeMH1Hzyx1ZAjv1QXwR5xmhT7M/edit?usp=sharing)
    -   **URL:** <http://192.168.56.10/galaxy/>

<!-- -->

-   Version 0.3.2 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.3.2-Mac.ova))
    -   **OS:** Ubuntu 14.04.4
    -   **OS ユーザー:** ubuntu/ubuntu
    -   **Python:** 2.7.11
    -   **Galaxy:** release_16.07
    -   **Tools:** [Tool List 0.3.2](https://docs.google.com/spreadsheets/d/1aVfXp5_dvt7I2EJ5LYeMH1Hzyx1ZAjv1QXwR5xmhT7M/edit?usp=sharing)
    -   **URL:** <http://192.168.56.10/galaxy/>

### Version 0.2.x

-   Version 0.2.4 ([mac](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.2.4-Mac.ova) / [win](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.2.4-Win.ova))
    -   **OS:** CentOS 6.6
    -   **Galaxy changeset:** 17050:6395e7035143
    -   **Galaxy database version:** 125
    -   **Tools:** [Tool List 0.2.4](https://drive.google.com/open?id=15P4Lu4N7GhTgqEaPFyedwWwpiNsqIGuwL-lLAxPXTEc)
    -   Apache によるプロキシは未設定のため、http://192.168.56.10:8080/ にアクセスしてください
    -   ワークフローの追加: [Variant Calling 02](/Workflow_Variant_Calling_02 "wikilink")

<!-- -->

-   Version 0.2.3 ([mac](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.2.3.ova) / [win](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.2.3-Win.ova))
    -   **OS:** CentOS 6.6
    -   **Galaxy changeset:** 17050:6395e7035143
    -   **Galaxy database version:** 125
    -   **Tools:** [Tool List 0.2.3](https://docs.google.com/spreadsheets/d/1dEpn4PoA_cOwD7RDHeGGmQ3dG1khgiF9G2COxEsqlwc)
    -   Apache によるプロキシは未設定のため、http://192.168.56.10:8080/ にアクセスしてください
    -   ワークフローの追加: [BS-seq 01](/Workflow_BS-seq_01 "wikilink"), [ChIP-seq 03](/Workflow_ChIP-seq_03 "wikilink"), [ChIP-seq 04](/Workflow_ChIP-seq_04 "wikilink")

<!-- -->

-   Version 0.2.2 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.2.2.ova))
    -   **OS:** CentOS 6.6
    -   **Galaxy changeset:** 16115:02ddea42faad
    -   **Galaxy database version:** 125
    -   **Tools:** [Tool List 0.2.2](https://docs.google.com/spreadsheets/d/114eKhwlDCUYVAHAMoz_QfaNJZHw2nCp-D5AoqOoAJXw)
    -   Apache によるプロキシは未設定のため、http://192.168.56.10:8080/ にアクセスしてください
    -   ワークフローの追加: [RNA-seq 02](/Workflow_RNA-seq_02 "wikilink")

<!-- -->

-   Version 0.2.1 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.2.1.ova))
    -   **OS:** CentOS 6.6
    -   **Galaxy changeset:** 16115:02ddea42faad
    -   **Galaxy database version:** 125
    -   **Tools:** [Tool List 0.2.1](https://docs.google.com/spreadsheets/d/1oECYODffa-Anrms50-Ktrz6w9RnsAoy5IUcgK7Ysd5Y)
    -   Apache によるプロキシは未設定のため、http://192.168.56.10:8080/ にアクセスしてください

### Version 0.1.x

-   Version 0.1.7 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.1.7.ova))
    -   database version: 120
    -   ツール更新 (bowtie2, tophat2, filebrowser, macs14)

<!-- -->

-   Version 0.1.6 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.1.6.ova))
    -   database version: 120
    -   ツール追加 (bowtie2, download_ref, imp_exp, filebrowser, sql_tools, sam_to_bam, bam_to_sam, macs14)

<!-- -->

-   Version 0.1.5 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.1.5.ova))
    -   database version: 120
    -   データ用ストレージの追加
    -   ユーザーがデータをパージできるように変更
    -   ツール追加 (awk, sed, macs)

<!-- -->

-   Version 0.1.4 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.1.4.ova))
    -   database version: 120
    -   トップ画面のリンクからインデックス・ファイルをダウンロード可能
    -   データ・ファイル (~/galaxy-dist/database) は本体と別のストレージに格納されるように変更
    -   URL を <http://192.168.56.10:8080/> から <http://192.168.56.10/galaxy/> に変更
    -   Galaxy をサービス登録し、OS の起動時に Galaxy が自動起動されるように変更
    -   ツール追加 (macs, bwa, sam_to_bam, bam_to_sam, export_to_sam)

<!-- -->

-   Version 0.1.3 ([download](http://download.pitagora-galaxy.org/data/release/Pitagora-Galaxy-0.1.3.ova))
    -   RNA-Seqのワークフローでtophat2が並行して動作するよう変更
    -   メモリを2GBから4GBに変更
    -   GalaxyのデータベースをSQLiteからMySQLに変更
    -   AWSのインスタンスをt2.Microからt2.Mediumで動作検証
    -   RNA-Seqのサンプルデータをアップロード

<!-- -->

-   Version 0.1.2
    -   Galaxy本体のストレージサイズを128GBに変更
    -   VMをAWSにインポートし、公開テスト用サイトを公開

<!-- -->

-   Version 0.1.1
    -   Galaxy本体のストレージサイズを2TBから1TBに変更
    -   ツール追加（cuffmerge, cuffdiff）
    -   ワークフロー追加（RNA-Seq）
    -   インデックス・ファイル用の空のストレージをマウント

<!-- -->

-   Version 0.1.0
    -   GalaxyのVMイメージを作成
    -   ツール追加（fastqc, tophat2, cufflinks）
