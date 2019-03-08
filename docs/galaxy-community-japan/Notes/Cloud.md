---
title: Cloud
permalink: /Cloud/
---

最新のAMI
---------

-   Version 0.2.3
    -   pitagora-galaxy-0.2.3-20150423 (ami-1a747974)

過去のAMI
---------

-   現在、過去のAMIは保管していません。VMをお使いください。

AMIの起動方法
-------------

-   [Amazon EC コンソール](https://console.aws.amazon.com/ec2/)を開き、ユーザー名とパスワードを入力し、\[Sign in using our secure server\]をクリックします。

[700px](/File:Cloud_01_m.png "wikilink")

-   Amazon EC2 ダッシュボードで、\[Launch Instance\] をクリックします。

[700px](/File:Cloud_02_m.png "wikilink")

-   \[Choose an Amazon Machine Image (AMI)\] ページで、\[Community AMIs\] をクリックします。

[1000px](/File:Cloud_03_m.png "wikilink")

-   検索ボックスに \[pitagora\] を入力し検索し、\[Select\] をクリックします。

[1000px](/File:Cloud_04_m.png "wikilink")

-   \[Choose an Instance Type\] ページで、起動するインスタンスのハードウェア設定とサイズを選択します。終了しましたら、\[Next: Configure Instance Details\] をクリックします。
    -   推奨は、\[t2.Medium\] の vCPUs: 2、Memory (GiB): 4 以上です。

[1000px](/File:Cloud_05_m.png "wikilink")

-   \[Configure Instance Details\]、\[Add Storage\] ページは\[Next\] をクリックします。

[1000px](/File:Cloud_06_m.png "wikilink") [1000px](/File:Cloud_07_m.png "wikilink")

-   \[Tag Instance\] ページは Value にInstance名として表示される名前を付け、\[Next\] をクリックします。

[1000px](/File:Cloud_08_m.png "wikilink")

-   \[Configure Security Group\] ページで、\[Create a new security group\] を選択し、\[security group\]、\[Description\]に入力します。
-   デフォルトのSSHの他に、HTTP、HTTPS、Custom TCP Rule（Port Range: 8080）を \[Add Rule\] をクリックします。
-   設定しましたら、\[Review and Launch\] をクリックします。

[1000px](/File:Cloud_09_m.png "wikilink")

-   \[Review Instance Launch\] ページで、設定を確認後、\[Launch\] をクリックします。

[1000px](/File:Cloud_10_m.png "wikilink")

-   \[Create a new key pair\] を選択し、\[key pair name\] に入力し、\[Download Key Pair\] をクリックし、認証キー(.pem)をダウンロードします。
-   認証キーをダウンロードしましたら、\[Launch Instances\] をクリックします。

[700px](/File:Cloud_11_m.png "wikilink")

-   \[View Instances\] をクリックします。

[1000px](/File:Cloud_12_m.png "wikilink")

-   \[Instances\] で、作成したインスタンスの Instance State が \[running\] になれば正常起動しています。

[1000px](/File:Cloud_13_m.png "wikilink")

-   \[Elastic IPs\] で、静的なIPを割り当てるIPを作成します。
-   \[Allocate New Address\] をクリックします。

[1000px](/File:Cloud_14_m.png "wikilink")

-   \[Associate Address\] をクリックし、作成したIPを割り当てます。
-   IPを割り当てたいインスタンス名を入力します。

[700px](/File:Cloud_16_m.png "wikilink")

-   \[Associate\] をクリックします。

[700px](/File:Cloud_17_m.png "wikilink")

-   \[Instances\] に戻り、\[Public IP\] を割り当てた \[Elastic IP\] が表示されていることを確認します。

[1000px](/File:Cloud_18_m.png "wikilink")

-   Webブラウザを起動し、\[割り当てたElastic IP/galaxy/\] にアクセスします。
    -   例）54.64.191.121/galaxy/

[700px](/File:Cloud_19_m.png "wikilink")