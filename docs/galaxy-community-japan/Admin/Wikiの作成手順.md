
`$ sudo vi /etc/php5/apache2/php.ini`
`#session.gc_maxlifetime = 1440 (= 24 min)`
`session.gc_maxlifetime = 86400 (= 1 day)`

### アップロード設定変更

`$ vi /var/www/wiki/LocalSettings.php`
`$wgFileExtensions = array('png','gif','jpg','pdf','docx','xlsx','pptx');`
`$ sudo vi /etc/php5/apache2/php.ini`
`post_max_size = 128M`
`upload_max_filesize = 128M`
`$ sudo /etc/init.d/apache2 restart`

### favicon

-   /var/www/wiki/ 内に favicon.ico を置く

`$ vi /var/www/wiki/LocalSettings.php`
`$wgFavicon = `“`../favicon.ico`”`;`

外部画像の許可
--------------

URL を書くだけで画像が貼れるようになります。

`$wgAllowExternalImages = true;`

<http://wiki.pitagora-galaxy.org/wiki/images/thumb/a/ae/Workflow.png/180px-Workflow.png>

### Google Analytics

`$ vi extensions/googleAnalytics/googleAnalytics.php`
`$wgGoogleAnalyticsAccount = 'UA-52746843-2';`
`//$wgGoogleAnalyticsIgnoreSysops = true;`
`//$wgGoogleAnalyticsIgnoreBots = true;`

`$ vi LocalSettings.php`
`require_once `“`$IP/extensions/googleAnalytics/googleAnalytics.php`”`;`

### iframe

    $ cd /var/www/wiki
    $ git clone https://gerrit.wikimedia.org/r/p/mediawiki/extensions/Widgets.git
    $ cd Widgets
    $ git submodule init
    $ git submodule update
    $ vi LocalSettings.php
    require_once "$IP/extensions/Widgets/Widgets.php";

-   これを下のページにコピー <http://www.mediawikiwidgets.org/w/index.php?title=Widget:Iframe&action=edit>
    -   [Widget:Iframe](/Widget:Iframe "wikilink")
