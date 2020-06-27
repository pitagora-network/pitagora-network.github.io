
EC2 API Tools をダウンロード
----------------------------

-   ホストにて、ダウンロードし、展開します。

<https://aws.amazon.com/developertools/351>

(初回のみ) AWS コンソールでIAM、S3作成
--------------------------------------

-   IAMに「vm_import」を作成します。 (Administrator Access)
-   S3に「pitagora」を作成します。

環境変数を設定
--------------

-   ホストに環境変数を設定します

<!-- -->

    $ vi ~/.bash_profile

    export EC2_HOME= <Toolsの展開先>
    export JAVA_HOME=`/usr/libexec/java_home`
    export PATH=$PATH:$JAVA_HOME/bin:$EC2_HOME/bin
    export AWS_ACCESS_KEY= <アクセスキーID>
    export AWS_SECRET_KEY= <シークレットアクセスキー>

    $ . ~/.bash_profile

VirtualBoxのOVFでのエクスポート
-------------------------------

-   /disk/ のストレージを OS 上でアンマウントし、VirtualBox 上でも外しておきます。
    -   EC2 ImportはVMのストレージが2以上は不可能であるため、ストレージを外す必要があります。
-   Oracle VM VirtualBox マネージャーから OVFのエクスポートします。（**拡張子を.ovfに変更する**）
-   OVFと一緒にVMDKが作られるので、これをAWSにインポートします。

VM Import
---------

-   VM Import用のスクリプトを作成し、実行します。

<!-- -->

    $ vi vm-import.sh

    <EC2 API Tools>/bin/ec2-import-instance \
    <エクスポートしたVMDK>.vmdk \
    --owner-akid <アクセスキーID>
    --owner-sak <シークレットアクセスキー>
    --format VMDK \
    --region ap-northeast-1 \
    --bucket pitagora \
    --architecture x86_64 \
    --platform Linux \
    --instance-type t2.micro

    $ ./vm-import.sh

VM Importの進捗確認
-------------------

-   VM Importの進捗確認用のスクリプトを作成します。

`$ vi check_progress.sh`
`./ec2-api-tools-1.7.5.1/bin/ec2-describe-conversion-tasks --region ap-northeast-1`

-   VM Importの進捗状況を確認します。

<!-- -->

    $ watch -d sh check_progress.sh

    TaskType        IMPORTINSTANCE  TaskId  import-i-fgg4piyo       ExpirationTime  2014-09-03T10:01:36Z    Status  active  StatusMessage   Pending InstanceID      i-97a7178e
    DISKIMAGE       DiskImageFormat VMDK    DiskImageSize   1568721408      VolumeSize      128     AvailabilityZone        ap-northeast-1c ApproximateBytesConverted       1470714096      Status  active  StatusMessage   Pending : Downloaded 1268776960
    ↓
    TaskType        IMPORTINSTANCE  TaskId  import-i-fgg4piyo       ExpirationTime  2014-09-03T10:01:36Z    Status  active  StatusMessage   Pending InstanceID      i-97a7178e
    DISKIMAGE       DiskImageFormat VMDK    DiskImageSize   1568721408      VolumeSize      128     AvailabilityZone        ap-northeast-1c ApproximateBytesConverted       1568716784      Status  active  StatusMessage   Progress: 50%
    ↓
    TaskType        IMPORTINSTANCE  TaskId  import-i-fgg4piyo       ExpirationTime  2014-09-03T10:01:36Z    Status  active  StatusMessage   Progress: 6%    InstanceID      i-97a7178e
    DISKIMAGE       DiskImageFormat VMDK    DiskImageSize   1568721408      VolumeId        vol-4c4afb4a    VolumeSize      128     AvailabilityZone        ap-northeast-1c ApproximateBytesConverted       1568716784      Status  completed
    ↓
    TaskType        IMPORTINSTANCE  TaskId  import-i-fg3tguis       ExpirationTime  2014-08-28T09:43:04Z    Status  completed       InstanceID      i-dabb0ec3
    DISKIMAGE       DiskImageFormat VMDK    DiskImageSize   1573807104      VolumeId        vol-5e7bcf58    VolumeSize      128     AvailabilityZone        ap-northeast-1c ApproximateBytesConverted       1573802544      Status  completed

VM ImportしたAMIを作成・起動
----------------------------

-   VM Importでインスタンスが生成された後、「pitagora-galaxy-x.x.x-ami」という名前でAMIを作成します。
-   作成されたAMIを起動し、100GB（このサイズはAMI起動時に変更可）の空のディスクを2つ（reference用、database用）追加します。
-   設定が完了後、インスタンスを「pitagora-galaxy-x.x.x-ami」という名前で起動します。

AMIの鍵認証
-----------

-   AMI起動時に、秘密鍵(\*\*\*\*\*\*\*\*.pem)でログインします。そのためには、AMIのKer Pairで紐付けた公開鍵をインスタンスに埋め込み、紐付けられた秘密鍵でログインできることを確認します。
-   VM Import直後は鍵認証が設定されていないため、sshのパスワード認証でログインします。

`$ ssh galaxy@`<ip_address>

-   cloud-init のインストール

`$ yum install cloud-init`

OSユーザー centos で接続できることを確認

`$ ssh centos@`<ip_address>` -i `<key_name>`.pem `
`$ su - galaxy`

-   \[不要\] galaxyユーザーでログインできる公開鍵の認証情報をインストールします。

<!-- -->

    $ sudo vi /etc/rc.d/rc.local

    # Install Public Key Credentials
    if [ ! -d /home/galaxy/.ssh ]; then
      mkdir -m 0700 -p /home/galaxy/.ssh
      restorecon /home/galaxy/.ssh
    fi
    # Get the root ssh key setup
    ReTry=0
    while [ ! -f /home/galaxy/.ssh/authorized_keys ] && [ $ReTry -lt 10 ]; do
      sleep 2
      curl -f http://169.254.169.254/latest/meta-data/public-keys/0/openssh-key > /home/galaxy/.ssh/pubkey
      if [ 0 -eq 0 ]; then
        mv /home/galaxy/.ssh/pubkey /home/galaxy/.ssh/authorized_keys
      fi
      ReTry=$[Retry+1]
    done
    chmod 600 /home/galaxy/.ssh/authorized_keys && restorecon /home/galaxy/.ssh/authorized_keys
    chown -R galaxy:galaxy /home/galaxy

-   \[不要\] 公開鍵認証を許可設定にします。（これはデフォルトなので設定変更必要なし）

`$ sudo vi /etc/ssh/sshd_config`
`PubkeyAuthentication yes`

-   \[不要\] 設定を反映させるため、再起動します。

`$ sudo reboot`

-   \[不要\] sshのパスワード認証でログインし直し、Key pair nameの秘密鍵名になっているか確認します。

`$ ssh galaxy@`<ip_address>
`$ vi .ssh/authorized_keys`
`$ exit`

-   \[不要\] galaxyでログインできるか確認します。

`$ ssh galaxy@`<ip_address>` -i `<key_name>`.pem `

-   \[不要\] rootのログイン拒否、及び、パスワード認証の拒否設定します。

`$ sudo vi /etc/ssh/sshd_config`
`PermitRootLogin no`
`PasswordAuthentication no`
`$ sudo service sshd restart`

ストレージの追加
----------------

    $ sudo mkfs -t ext4 /dev/xvdb
    $ sudo mkfs -t ext4 /dev/xvdc
    $ sudo vi /etc/fstab
    /dev/xvdb  /disk/reference  ext4  defaults  0  0
    /dev/xvdc  /disk/database  ext4  defaults  0  0
    $ sudo mount -a
    $ sudo chown galaxy:galaxy /disk/*

-   ~/galaxy-dist/database は既に /disk/database へのシンボリック・リンクになっているため、このディレクトリに必要なデータをコピーする。

<!-- -->

    $ cd ~/galaxy-dist
    $ rm database
    $ ln -s /disk/database
    $ cp -r database_bak/* database/

AMI Tools のインストール
------------------------

-   AMI Tools をインストールします。

<!-- -->

    $ sudo yum install -y ruby unzip
    $ wget http://s3.amazonaws.com/ec2-downloads/ec2-ami-tools.zip
    $ sudo mkdir -p /usr/local/ec2
    $ sudo unzip ec2-ami-tools.zip -d /usr/local/ec2
    $ vi .bash_profile

    export EC2_AMITOOL_HOME=/usr/local/ec2/ec2-ami-tools-<version>
    export PATH=$EC2_AMITOOL_HOME/bin:$PATH

    $ . .bash_profile

-   AMI Tools のバージョンを確認します。

<!-- -->

    $ ec2-ami-tools-version

root アクセスの無効化
---------------------

-   sudoの使用には影響はありません。

<!-- -->

    $ sudo passwd -l root

SSH ホストキーペアの削除
------------------------

    $ sudo shred -u /etc/ssh/*_key /etc/ssh/*_key.pub

sshd DNS チェックの無効化
-------------------------

    $ sudo vi /etc/ssh/sshd_config

    UseDNS no

コンソールログの削除
--------------------

    $ shred -u ~/.*history

Galaxyに関するログの削除
------------------------

    $ shred -u ~/galaxy/paster.log
    $ shred -u /var/spool/mail/galaxy

公開鍵認証の削除
----------------

-   .ssh/authorized_keys はAMIを作成する前に必ず削除してください。

<!-- -->

    $ shred -u .ssh/authorized_keys
    $ rm -rf .ssh

AMIの作成と公開
---------------

-   シャットダウンし、AMIを作成します。
-   AMIを公開します。

後処理
------

-   S3に中間ファイルができているので、 ファイルを削除します。

参考サイト
----------

-   Guidelines for Shared Linux AMIs

<http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/building-shared-amis.html>

-   AMI を一般公開する

<http://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/sharingamis-intro.html>