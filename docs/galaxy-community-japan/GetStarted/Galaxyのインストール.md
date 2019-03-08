---
title: Galaxyのインストール
permalink: /Galaxyのインストール/
---

OSユーザーの作成
----------------

EC2インスタンスにははじめにec2-userユーザーが作成されていますが、 わかりやすくするためGalaxyのインストールのためのgalaxyユーザーを作成します。

### rootユーザーの設定

ユーザーをSSH接続時に使用したec2-userからrootに変更し、 rootユーザーのパスワードを設定します。

     $ sudo su -
     $ passwd

### galaxyユーザーの作成

Galaxyをインストールし実行するユーザーとしてgalaxyユーザーを作成します。

    $ su -
    # useradd galaxy
    # passwd galaxy
    # exit

galaxyユーザーにスイッチし、パスワードが設定されていることを確認します。 この後の作業はgalaxyユーザーを使用します。

    $ su - galaxy

Pythonのインストール
--------------------

Galaxyは起動時にPythonのバージョンを見て対応するEggをダウンロードするため、 適切なバージョンのPythonをインストールする必要があります。

### 既存のPythonの確認

まず、現在バンドルされているPythonのバージョンを確認します。

     $ python --version
     Python 2.6.9

上記のようにAWSのAmazonLinuxではPython2.6.9がバンドルされているためGalaxyがインストールできます。 しかし、Galaxyで使用するツールがPythonにライブラリを追加する事があるため、バンドルされているものとは別に、Galaxyで使うPythonを作成しておきます。

### パッケージのインストール

Python(2.7.5)をインストールするために必要な以下の４つのパッケージをインストールします。

-   gcc
-   zlib-devel
-   bzip2-devel
-   openssl-devel
-   sqlite-devel

<!-- -->

     # yum install -y gcc zlib-devel bzip2-devel openssl-devel sqlite-devel

### Pythonのコンパイル

galaxyユーザーでPython(2.7.5)をコンパイルします。

    # exit
    $ mkdir galaxy-python
    $ cd galaxy-python
    $ wget http://www.python.org/ftp/python/2.7.5/Python-2.7.5.tgz
    $ tar xvzf Python-2.7.5.tgz
    $ mv Python-2.7.5 Python-2.7.5_src
    $ mkdir Python-2.7.5
    $ cd Python-2.7.5_src
    $ ./configure --prefix=/home/galaxy/galaxy-python/Python-2.7.5
    $ make
    $ make install
    $ which python
       /usr/bin/python

### 環境変数の設定と確認

Pythonに対する追加事項がある際の記載先を、新しくインストールしたPythonにするため、 環境変数の設定を変更します。

    $ vi ~/.bash_profile

    export PATH=$HOME/galaxy-python/Python-2.7.5/bin:$PATH
    export PYTHONPATH=$HOME/galaxy-python/Python-2.7.5/lib/python2.7/site-packages

    $ . ~/.bash_profile

Pythonの設定の変更を確認します。

    $ which python
        ~/galaxy-python/Python-2.7.5/bin/python
    $ python --version
    　Python 2.7.5

### その他の設定

後々の作業のため、easy_install をインストールします。

    $ cd ~/galaxy-python
    $ wget http://python-distribute.org/distribute_setup.py
    $ python distribute_setup.py
    $ which easy_install
       ~/galaxy-python/Python-2.7.5/bin/easy_install

Galaxyのインストール
--------------------

GalaxyはMercurialで配布されています。

### Mercurialのインストール

Mercurialをインストールします。

    $ easy_install mercurial

### Galaxyのインストール

Galaxyをクローンします。galaxy-distディレクトリが作成され、この下にGalaxyがインストールされます。

    $ cd ..
    $ hg clone https://bitbucket.org/galaxy/galaxy-dist/

### Galaxyの起動

Galaxyを起動します。 初回起動時は進捗を見るためログをコンソールに表示させます。 初回起動時に設定ファイル ~/galaxy-dist/universe_wsgi.iniが作成されます。

    $ cd galaxy-dist
    $ ./run.sh

ログを確認し、正常に起動されているかを確認します。 次のメッセージが表示されると正常に起動が行われています。

     $ tail -f paster.log
        .
        .
     serving on 0.0.0.0:8080 view at http://127.0.0.1:8080

次のようなメッセージが出力されている場合には、そのスクリプトを実行します。

    Please backup your database and then migrate the schema by running 'sh manage_db.sh upgrade'.

    $ sh manage_db.sh upgrade

### Galaxyのバージョンアップ

必要に応じて、インストールしたGalaxyを最新の安定版にバージョンアップします。

     $ hg update stable

### IPアクセス制限の解除

作成したGalaxyにどのIPアドレスからでも接続できるように、universe_wsgi.iniを編集します。 IPアドレスによるアクセス制限は、AWS上でポート単位で設定します。

     $ vi universe_wsgi.ini
     host = 0.0.0.0

### .python_eggs ディレクトリの権限変更

.python_eggs ディレクトリの権限を変更します。

    $ cd ~/
    $ chmod 744 .python_eggs

ブラウザを開き、「<EC2インスタンスのPublicDNS>:8080」のURLより作成したGalaxyのページに接続します。