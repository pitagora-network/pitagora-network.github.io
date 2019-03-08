---
title: VM配布準備手順
permalink: /VM配布準備手順/
---

[Main Page](/Main_Page "wikilink") &gt;&gt; [Admin](/Admin "wikilink") &gt;&gt;

エクスポート前
--------------

### シェルのヒストリー削除

`shred -u ~/.*history`

### Galaxyのログ削除

`echo > ~/galaxy/paster.log`

### DNSサーバー情報の削除

`sudo vi /etc/resolv.conf`

### ディスクの付け替え

VMをシャットダウンし、バックアップしておいた空のディスクとファイルを置き換えて起動します。

-   disk2.vmdk リファレンス用
-   disk3.vmdk データ用

バックアップを忘れていた場合には、OVAから別のVMを展開し、そのディスクを置き換えます。

### Docker コンテナ／イメージの削除

`$ sudo docker ps -a`
`$ sudo docker rm `<container_id>` `<container_id>` ..`

`$ sudo docker images`
`$ sudo docker rmi `<image_id>` `<image_id>` ..`

### ストレージの圧縮

-   この手順は現在は実施していません。

OVAをインポートする際、ディスクの拡張子をvmdkからvdiに変更しておきます。

ゲストOSでhomeとrootのパーティションでddを実行します。

    $ dd if=/dev/zero of=zero bs=4k; \rm zero
    dd: writing `zero': デバイスに空き領域がありません
    $ sudo dd if=/dev/zero of=zero bs=4k; \rm zero
    dd: writing `zero': デバイスに空き領域がありません

ホストOSで圧縮を実行します。

    $ vboxmanage list hdds (UUIDを調べる)
    $ vboxmanage modifyhd <UUID> --compact

エクスポート
------------

-   この際、USB コントローラは外しておく（VirtualBox にコントローラの Extention がインストールされていない場合があるため）

### Windows 用にエクスポート

-   Mac と Windows ではデフォルトで作成されるホストオンリーネットワークの名前が異なるため、起動時にネットワーク設定の変更が求められる。

1.  Mac で作成したイメージを Windows でインポート（このとき、正しいホストオンリーネットワークが指定されているように見える）
2.  一度、ホストオンリーネットワークのネットワークアダプタを無効化して保存してから、もう一度、有効化する
3.  エラー無しで起動することを確認してから、Windows 用にイメージをエクスポート
