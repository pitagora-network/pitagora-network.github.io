---
title: Galaxyの高度な設定
permalink: /Galaxyの高度な設定/
---

管理のための設定
----------------

管理に必要な設定を記載します。
参考：https://wiki.galaxyproject.org/Admin/Config/Performance/ProductionServer

### 管理者ユーザーの設定

Galaxy上の管理者ユーザーを設定します。 これらのユーザーがログインした際にはメニューバーに「Admin」メニューが表示されます。

    $ vi universe_wsgi.ini

    admin_users = admin01@kagalaxy.com, admin02@kagalaxy.com

### 再起動用スクリプトの作成

Galaxyを再起動する際の手間を省くため、再起動スクリプトを作成します。

    $ cd ~/galaxy-dist
    $ vi restart.sh

    ./run.sh --stop
    ./run.sh --daemon
    tail -f paster.log | grep -v "DEBUG"

    $ chmod +x restart.sh

### Galaxyの自動起動設定

GalaxyをOSの起動後に自動起動するため、起動スクリプトを作成します。

    # vi /etc/init.d/galaxy-init

    #!/bin/sh
    # chkconfig: 2345 99 99
    # description: autorun galaxy

    user=galaxy
    galaxy_run=/home/galaxy/galaxy-dist/run.sh

    do_start(){
      su - $user -c "$galaxy_run --daemon"
      return 0
    }

    do_stop(){
      su - $user -c "$galaxy_run --stop"
      return 0
    }

    case "$1" in
      start)
        do_start
        ;;
      stop)
        do_stop
        ;;
      restart)
        do_stop
        do_start
        ;;
      *)
        echo "Usage: $0 {start|stop|restart}"
        RETVAL=1
    esac

    exit $RETVAL

ユーザー・グループをgalaxyに設定し、実行権限を付与します。

    $ chown galaxy:galaxy /etc/init.d/galaxy-init
    $ chmod +x /etc/init.d/galaxy-init

スクリプトをサービスに追加し、自動起動の設定をします。

    # chkconfig --add galaxy-init
    # chkconfig galaxy-init on

Apache Proxyの利用
------------------

参考：http://wiki.g2.bx.psu.edu/Admin/Config/Apache%20Proxy

### http.confのRewriteルールの設定

最初のrewriteルールは、/galaxy、/galaxy/どちらにアクセスしてもいいようにするためのものです。
続くルールによって静的コンテンツ（CSS、Javascript、画像ファイルなど）へのリクエストをApacheでさばくようにして、最後のルールでそれ以外のリクエストをGalaxyに向けるようにしています。
ここではgalaxy-dist/static以下にある静的コンテンツファイルを/data/users/galaxy/galaxy-dist/staticにコピーしています。

    $ su -
    # vi /etc/httpd/conf/httpd.conf

    RewriteEngine on
    RewriteRule ^/galaxy$ /galaxy/ [R]
    RewriteRule ^/galaxy/static/style/(.*) /data/users/galaxy/galaxy-dist/static/june_2007_style/blue/$1 [L]
    RewriteRule ^/galaxy/static/scripts/(.*) /data/users/galaxy/galaxy-dist/static/scripts/packed/$1 [L]
    RewriteRule ^/galaxy/static/(.*) /data/users/galaxy/galaxy-dist/static/$1 [L]
    RewriteRule ^/galaxy/favicon.ico /data/users/galaxy/galaxy-dist/static/favicon.ico [L]
    RewriteRule ^/galaxy/robots.txt /data/users/galaxy/galaxy-dist/static/robots.txt [L]
    RewriteRule ^/galaxy(.*) http://localhost:8080$1 [P]

### universe_wsgi.iniの設定

~/galaxy-dist/universe_wsgi.iniに記載されている以下の３つの内容を編集します。

    $ vi ~/galaxy-dist/universe_wsgi.ini

-   filter-with = proxy-prefix
    -   proxy-prefixフィルターを有効にします。
        このproxy-prefixフィルターというのは、PasteDeployによって提供されるWSGIミドルウェアのひとつです。
        今回のケースのようにリバースプロキシによってURLを変更している場合にこれを適用することで、PATH_INFOやSCRIPT_NAME、REMOTE_ADDRなどの環境変数を正しい値に置換してくれます。

<!-- -->

-   cookie_path = /galaxy
    -   cookie_pathはSet-Cookieヘッダのpath属性に直接代入されます。
        Cookieをサイトワイドに効かせたくない場合（例えば複数のGalaxyを立ち上げてる場合とか）などに設定します。
        これを設定することでユーザ・ログインができなくなる現象が報告されている [1](https://bitbucket.org/galaxy/galaxy-central/issue/346/cant-login) が、これは設定前の値(/のみ)でブラウザにCookieが保存されている場合に発生するようだ。はじめから何かしらの値を設定しておくことにする

<!-- -->

-   static_enabled = False
    -   静的コンテンツはApacheによって配信する設定を行ったので、Galaxy側ではその必要がなくなるので以下のようにして無効化します。
        これで静的コンテンツを返すためのミドルウェアがGalaxyのWSGIミドルウェアスタックから除外されます。

XSendFileの設定
---------------

XSendFile を使用することでファイルのダウンロード速度を向上させることができます。
サイズの大きいデータ・ファイルのダウンロード時にレスポンスが非常に遅くなることを確認しています。

### パッケージのインストール

XsendFileのインストールに必要な以下のパッケージをインストールします。

-   httpd-devel

<!-- -->

    $ yum install httpd-devel

### XsendFileのインストール

URL(https://tn123.org/mod_xsendfile/)のリンク先より「mod_xsendfile.c 」をダウンロードし、 以下のコマンドを実行します。

    $ wget https://tn123.org/mod_xsendfile/mod_xsendfile.c#hash(sha256:8e8c21ef39bbe86464d3831fd30cc4c51633f6e2e0022
    04509e55fc7c8df9cf9)
    $ /usr/sbin/apxs -cia mod_xsendfile.c

### インストール後の設定の変更

XsendFileの設定を有効にするため、httpd.comf、universal_wsgi.iniに以下を追記します。

    $ vi httpd.conf

    <Location "/galaxy">
         XSendFile on
         XSendFilePath /
     </Location>

    $ vi ~/galaxy-dist/universal_wsgi.ini

    apache_xsendfile = True

データベースの変更
------------------

以下の手順により、使用するデータベースをMySQLに変更します。

### MySQLのインストール

#### インストール

MySQL,phpMyAdminをインストールするために、以下の４つをインストールします。

-   php
-   php-mbstring
-   mysql-server
-   php-mysql

<!-- -->

     yum -y install php php-mbstring mysql-server php-mysql

#### MySQLの起動

以下のコマンドよりファイアーの無効化、ApacheとMySQLを起動及び自動起動を設定します。

    # service iptables stop
    # service httpd start
    # chkconfig httpd on
    # service mysqld start
    # chkconfig mysqld on

#### MySQLのrootパスワード設定

MySQLで使用するrootユーザーのパスワードを設定します。

    # mysql
    mysql> SET PASSWORD FOR root@localhost=PASSWORD('xxxxx');
                  Query OK, 0 rows affected (0.00 sec)
    mysql> exit

パスワードが設定されているかのチェックを行います。

    # mysql
    ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)

パスワードを入力し、ログインします。

    # mysql -u root -p
    Enter password:
    ...
    Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
    mysql>

### phpMyAdminのインストール

#### PHPのアップデート

先ほどインストールしたPHPを最新のphpMyAdminが対応しているバージョンにアップデートします。

     $ cd /etc/yum.repos.d
     $ wget http://dev.centos.org/centos/6/testing/CentOS-Testing.repo

        ...`CentOS-Testing.repo' へ保存完了...

     $ ls
     CentOS-Base.repo       CentOS-Media.repo    CentOS-Vault.repo  epel.repo
     CentOS-Debuginfo.repo  CentOS-Testing.repo  epel-testing.repo

    $ yum --enablerepo=c6-testing update php php-mcrypt

#### phpMyAdminのインストール

URL(http://www.phpmyadmin.net/home_page/downloads.php)のリンク先より、 「phpMyAdmin-3.4.9-all-languages.tar.gz 」のアドレスを取得してきます。

     $ cd /var/www/html/
     $ wget http://...
     $ tar xvzf phpMyAdmin-4.0.5-all-languages.tar.gz
     $ mv phpMyAdmin-3.4.9-all-languages phpmyadmin
     $ chown -R root:apache phpmyadmin
     $ mkdir phpmyadmin/config
     $ chmod 777 phpmyadmin/config

### phpMyAdminの接続確認

ブラウザを開き、http://<EC2インスタンスのPublicDNS>/phpmyadmin/setup/ にアクセスし、 以下の設定を行います。

1.  新しいサーバの作成
2.  認証、認証タイプを「cookie」、パスワードを「galaxy」
3.  保存する
4.  設定ファイルを保存

<!-- -->

     $ mv phpmyadmin/config/config.inc.php phpmyadmin/config.inc.php

<http://><EC2インスタンスのPublicDNS>/phpmyadmin/ にアクセスしてログインを確認し、 以下の設定を行います。

1.  新しいデータベース「galaxy」の作成
2.  新しいユーザー「galaxy」を作成
    -   ホスト： localhost
    -   パスワード：galaxy
    -   グローバル特権：ALL PRIVIEGES

### データベースの設定

#### ログイン確認

作成したデータベース「galaxy」にgalaxyユーザーでログインを確認します。

    $ mysql -u galaxy -p -D galaxy

#### データベースをMySQLに変更

Galaxyで使用するデータベースを新しく作成したMySQLに変更するための設定をします。

     $cd /home/galaxy/galaxy-dist
     $vi universe_wsgi.ini

`#database_connection = sqlite:///./database/universe.sqlite?isolation_level=IMMEDIATE`
`database_connection = mysql://galaxy:galaxy@localhost:3306/galaxy?unix_socket=/var/lib/mysql/mysql.sock`

</pre>
MySQLでは、Galaxyにアクセスがないなど、一定時間接続がないと接続が切断され、 「MySQL server has gone away」というエラーが発生します。 これを回避するために一定時間で再接続するよう設定しています。 この時、MySQLのwait_timeoutよりも短い時間を設定する必要があります。

     #database_engine_option_pool_recycle = -1
     database_engine_option_pool_recycle = 7200

Galaxyの再起動を行うと、MySQLに変更したgalaxyデータベースにテーブルがCreate Tableされます。

    $ ./restart.sh

その他
------

### アイコンの変更

    $ cp favicon.ico ~/galaxy-dist/static/
    $ cp galaxyIcon_noText.png ~/galaxy-dist/static/images/