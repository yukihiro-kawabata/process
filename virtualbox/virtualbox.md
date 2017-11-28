# 初期設定

前提として、LinuxのCentOS6xのマシンで進める


### 下記のシェルを叩けば、基本的なWEBサーバができる

    #!/bin/sh

    # gcc
    yum -y install gcc gcc-c++ glibc-devel.x86_64 glibc-devel.i.686
    # php7
    yum -y install epel-release
    wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    rpm -Uvh remi-release-6*.rpm
    yum -y install --enablerepo=remi --enablerepo=remi-php70 php
    yum install -y --enablerepo=remi --enablerepo=remi-php70 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pear php-gd
    chkconfig httpd on
    # mysql56
    yum -y install http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
    yum -y install mysql-community-server
    chkconfig mysqld on
    #phpMyAdmin
    wget https://files.phpmyadmin.net/phpMyAdmin/4.6.3/phpMyAdmin-4.6.3-all-languages.tar.bz2
    tar xvf phpMyAdmin-4.6.3-all-languages.tar.bz2
    rm -rvf phpMyAdmin-4.6.3-all-languages.tar.bz2
    mv phpMyAdmin-4.6.3-all-languages/ phpMyAdmin/
    mv phpMyAdmin/ /var/www/html/phpMyAdmin/
    # composer
    curl -sS https://getcomposer.org/installer | php
    mv composer.phar /usr/local/bin/composer
    # xhpof
    mkdir /var/log/xhprof/
    chmod -R 777 /var/log/xhprof/
    yum -y install graphviz
    cd /tmp
    git clone https://github.com/Yaoguais/phpng-xhprof.git
    cd phpng-xhprof/
    phpize
    ./configure
    make && make test && make install
    mkdir /home/xhprof.com
    chmod -R 777 /home/xhprof.com
    chmod -R a+x /home/xhprof.com
    cd /home/xhprof.com/
    composer require facebook/xhprof dev-master
    cp vendor/facebook/xhprof/xhprof_html/ ./ -rf
    cp vendor/facebook/xhprof/xhprof_lib/ ./ -rf
    #mysql
    service mysqld restart
    # sshpass
    yum install sshpass --enablerepo=epel


----------------

### php7をインストール

    yum -y install epel-release
    wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    rpm -Uvh remi-release-6*.rpm
    yum -y install --enablerepo=remi --enablerepo=remi-php70 php

    php -vで以下が出たら成功

    PHP 7.0.0 (cli) (built: Dec 3 2015 18:05:30) ( NTS )
    Copyright (c) 1997-2015 The PHP Group
    Zend Engine v3.0.0, Copyright (c) 1998-2015 Zend Technologies

    ##phpで足りないものを入れる
    yum install -y --enablerepo=remi --enablerepo=remi-php70 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pear php-gd

------------

### mysql5.7のインストール、設定

    ##mysqlのダウンロードと解凍
    wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.12-1.el6.x86_64.rpm-bundle.tar
    tar xvf mysql-5.7.12-1.el6.x86_64.rpm-bundle.tar
    rm -rvf mysql-5.7.12-1.el6.x86_64.rpm-bundle.tar

    ##余分なものを消す 行う順番が前後しているかもしれません。↑↑
    rpm -ivh \
    mysql-community-common-5.7.12-1.el6.x86_64.rpm \
    mysql-community-libs-5.7.12-1.el6.x86_64.rpm \
    mysql-community-libs-compat-5.7.12-1.el6.x86_64.rpm  \
    mysql-community-client-5.7.12-1.el6.x86_64.rpm  \
    mysql-community-server-5.7.12-1.el6.x86_64.rpm \
    mysql-community-devel-5.7.12-1.el6.x86_64.rpm  \
    mysql-community-embedded-5.7.12-1.el6.x86_64.rpm \
    mysql-community-embedded-devel-5.7.12-1.el6.x86_64.rpm

    ##MySQL設定ファイル編集 参考サイト https://centossrv.com/mysql.shtml
    vi /etc/my.cnf
    ## 下記追加

    ### 5.6以前と同じ動きにする
    sql_mode = NO_ENGINE_SUBSTITUTION
    ### ロードできるファイルパスを制限
    secure_file_priv = ''
    ### レプリケーションを SBR にする? デフォは ROW 現状は MIXED
    binlog_format = MIXED
    ### 正常停止時にバッファプールの中身を ib_buffer_pool へ記録
    innodb_buffer_pool_dump_at_shutdown = 1
    ### 起動時に ib_buffer_pool を読まない
    innodb_buffer_pool_load_at_startup = 0
    ### ウォームアップでバッファプールを100%使い切る
    loose-innodb_buffer_pool_dump_pct = 100
    ### UTC時刻からシステム時刻へ
    loose-log_timestamps = SYSTEM

    # パスワードのセキュリティーレベルを下げる
    validate_password_policy=LOW

    ###################################################################ここまで MySQL設定ファイル


    # mysqlの初期設定

    mysql_secure_installation


    ## 設定ログ ########

    # mysql_secure_installation

    Securing the MySQL server deployment.

    Enter password for user root: 初期パスワードを入力する

    The existing password for the user account root has expired. Please set a new password.

    New password: 新しいパスワードを入力する

    Re-enter new password: 再度同じ新しいパスワードを入力する

    VALIDATE PASSWORD PLUGIN can be used to test passwords
    and improve security. It checks the strength of password
    and allows the users to set only those passwords which are
    secure enough. Would you like to setup VALIDATE PASSWORD plugin?

    Press y|Y for Yes, any other key for No: y

    There are three levels of password validation policy:

    LOW Length >= 8
    MEDIUM Length >= 8, numeric, mixed case, and special characters
    STRONG Length >= 8, numeric, mixed case, special characters and dictionary file

    Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 0
    Using existing password for root.

    Estimated strength of the password: 100
    Change the password for root ? ((Press y|Y for Yes, any other key for No) : y

    New password: ポリシーに沿った新しいパスワードを入力

    Re-enter new password: 再度新しいパスワードを入力する

    Estimated strength of the password: 50
    Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
    By default, a MySQL installation has an anonymous user, a user account created for them. This is intended only for
    testing, and to make the installation go a bit smoother.
    You should remove them before moving into a production
    environment.

    Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
    Success.


    Normally, root should only be allowed to connect from
    'localhost'. This ensures that someone cannot guess at
    the root password from the network.

    Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
    Success.

    By default, MySQL comes with a database named 'test' that
    anyone can access. This is also intended only for testing,
    and should be removed before moving into a production
    environment.


    Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y Success.

    - Removing privileges on test database...
    Success.

    Reloading the privilege tables will ensure that all changes
    made so far will take effect immediately.

    Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
    Success.

    All done! 


-------

### MySQLのユーザ設定など

    ##DBを作る
    mysql> create database [**]

    ##DBにデータを入れる。 
    mysql -u root -p --default-character-set=utf8 [DB] < [filePath]

    ##全て流し終えたら sqlファイルを消す
    rm -rvf sql/*



    #MySQL5.7はデフォルトで validate_password のプラグインが有効になっているので、パスワード無し設定をしたい場合は無効にする

    ##rootユーザーを作成する
    mysql> use mysql
    mysql> grant all privileges on *.* to root@"%" identified by 'パスワード' with grant option;

    ##rootユーザーのパスワードを無にする
    mysql> grant all privileges on *.* to root@"%" identified by '';

    ## 設定が上手くできているのか確認(ユーザーにroot、ホストに%がついていれば成功)
    mysql> select user,host from user;

--------------


### SElinux の設定

    vi /etc/selinux/config
    SELINUX=enforcing  →   SELINUX=disabled

---------

### ポート開放

    vi /etc/sysconfig/iptables
    以下の２つを加える
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
    -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT ※(必要があれば)

--------

### php.iniを編集

    vi /etc/php.ini

    以下のように書き換える

    short_open_tag = ON
    max_execution_time
    max_input_time
    memory_limit
    display_errors = Off
    error_reporting = E_ALL & ~E_NOTICE & ~E_STRICT
    post_max_size = 256M
    upload_max_filesize = 256M
    date.timezone = Asia/Tokyo
    session.gc_divisor = 1000
    session.gc_maxlifetime = 10800
    mbstring.language = Japanese
    mbstring.internal_encoding = UTF-8
    mbstring.http_input = auto
    mbstring.http_output = UTF-8
    mbstring.encoding_translation = On
    mbstring.detect_order = auto
    mbstring.substitute_character = none;


---------


### phpmyadminをブラウザ上で見る方法
    参考ページ http://mrs.suzu841.com/tebiki/centos55/phpmyadmin.html
    phpmyadmin をダウンロードするページ → https://www.phpmyadmin.net/downloads/
    
    
    wget https://files.phpmyadmin.net/phpMyAdmin/4.6.3/phpMyAdmin-4.6.3-all-languages.tar.bz2
    tar xvf phpMyAdmin-4.6.3-all-languages.tar.bz2
    rm -rvf phpMyAdmin-4.6.3-all-languages.tar.bz2
    mv phpMyAdmin-4.6.3-all-languages/ phpMyAdmin/
    mv phpMyAdmin/ /var/www/html/phpMyAdmin/

    mysql -u root -p -A mysql
    mysql> INSERT user SET user='hogehoge';
    mysql> grant all privileges on *.* to hogehoge@"%" identified by '';
    mysql> UPDATE user SET authentication_string=password('passpass') WHERE user='hogehoge';
    mysql> exit

    service mysqld restart
    service httpd restart

    ID    hogehoge
    pass  passpass55  で入れる


-------------


### virtualboxのネットワーク設定(Basic Server)

    ##eth0の設定
    vim /etc/sysconfig/network-scripts/ifcfg-eth0
    ################################ 中身
    DEVICE=eth0
    HWADDR=08:00:27:7D:B8:30
    $TYPE=Ethernet
    $UUID=f1f37dc8-0cfe-4d9c-976f-2dffb8365821
    ONBOOT=on
    NM_CONTROLLED=yes
    BOOTPROTO=dhcp
    
    
    ##eth1の設定
    vim /etc/sysconfig/network-scripts/ifcfg-eth1
    ###############################中身
    DEVICE=eth1
    BOOTPROTO=static
    ONBOOT=yes
    IPADDR=192.168.56.102
    NETMASK=255.255.255.0
    NETWORK=0.0.0.0
    
    
    ##eth2の設定
    vim /etc/sysconfig/network-scripts/ifcfg-eth2
    ################################中身
    CE=eth2
    HWADDR=08:00:27:c4:d4:3b
    $TYPE=Ethernet
    $UUID=f1f37dc8-0cfe-4d9c-976f-2dffb8365821
    ONBOOT=on
    NM_CONTROLLED=yes
    BOOTPROTO=dhcp



### 上記の設定方法(解説)
    コマンドで ifconfig と打つと

    [root@localhost ~]# ifconfig
    eth0      Link encap:Ethernet  HWaddr 08:00:27:7D:B8:30
              inet6 addr: fe80::a00:27ff:fe7d:b830/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:123033 errors:0 dropped:0 overruns:0 frame:0
              TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:10491097 (10.0 MiB)  TX bytes:2148 (2.0 KiB)

    eth1      Link encap:Ethernet  HWaddr 08:00:27:F2:BC:1F
              inet addr:192.168.56.102  Bcast:192.168.56.255  Mask:255.255.255.0
              inet6 addr: fe80::a00:27ff:fef2:bc1f/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:572779 errors:0 dropped:0 overruns:0 frame:0
              TX packets:147171 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:716363345 (683.1 MiB)  TX bytes:22715209 (21.6 MiB)

    eth2      Link encap:Ethernet  HWaddr 08:00:27:C4:D4:3B
              inet addr:10.0.4.15  Bcast:10.0.4.255  Mask:255.255.255.0
              inet6 addr: fe80::a00:27ff:fec4:d43b/64 Scope:Link
              UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
              RX packets:1046 errors:0 dropped:0 overruns:0 frame:0
              TX packets:923 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:1000
              RX bytes:85126 (83.1 KiB)  TX bytes:202993 (198.2 KiB)

    lo        Link encap:Local Loopback
              inet addr:127.0.0.1  Mask:255.0.0.0
              inet6 addr: ::1/128 Scope:Host
              UP LOOPBACK RUNNING  MTU:65536  Metric:1
              RX packets:0 errors:0 dropped:0 overruns:0 frame:0
              TX packets:0 errors:0 dropped:0 overruns:0 carrier:0
              collisions:0 txqueuelen:0
              RX bytes:0 (0.0 b)  TX bytes:0 (0.0 b)


以上が出てくる。NATとブリッジアダプターはeth0とeth2。ホストオンリーはeth1。(最初の起動画面での設定によって異なる)それぞれに該当するものを入れる。


### ネットがつながらない場合

    vi /etc/udev/rules.d/70-persistent-net.rules

ここで、上から古いものなのでいらないものを消す


### 無線でつなぎたい場合
    ブリッジアダプター接続の設定をする、そのあとに以下を実行
    ifconfig usb0 on
    dhclient -r -v usb0
    service network restart




### cronの設定
    
    参考サイト http://oxynotes.com/?p=6912

    yum install cronie-noanacron
    service crond start
    chkconfig crond on

    ##フォルダの中身を１時間ごとに消す
    tmpwatch -all 1 /var/log/httpd/




### xhprofの設定
    参考サイト -->  http://qiita.com/morisuke/items/49f17bda2c764ae8f725
    
    
    cd /tmp
    git clone https://github.com/Yaoguais/phpng-xhprof.git
    cd phpng-xhprof/
    phpize
    ./configure
    make && make test && make install
    
    # php.iniに追加
    vi /etc/php.ini
    
    ---------------------------- ここから
    [xhprof]
    extension=phpng_xhprof.so
    xhprof.output_dir="/tmp"
    ----------------------------- ここまで
    
    
    mkdir /var/log/xhprof/
    chmod -R 777 /var/log/xhprof/

    yum install graphviz


### composer がインストールされていなかったら
    
    curl -sS https://getcomposer.org/installer | php
    # 確認 なにか文字が出てきます
    ./composer.phar
    
    # 移動させる
    mv composer.phar /usr/local/bin/composer
    
    ------
    
    
    mkdir /home/xhprof.com
    chmod -R 777 /home/xhprof.com
    chmod -R a+x /home/xhprof.com
    cd /home/xhprof.com/
    composer require facebook/xhprof dev-master
    
    # コマンドを打つ
    cp vendor/facebook/xhprof/xhprof_html/ ./ -rf
    cp vendor/facebook/xhprof/xhprof_lib/ ./ -rf

    # httpd.confを編集
    vim /etc/httpd/conf/httpd.conf

    <VirtualHost *:80>
        ServerName vm.xhprof.com
        DocumentRoot /home/xhprof.com/xhprof_html
    </VirtualHost>
    <Directory /home/xhprof.com/xhprof_html >
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
    
    

###  apacheを再起動
    service httpd restart


------



### virtualboxで rootユーザーでmaintenanceモードに入ったら
    参考URL
    http://d.hatena.ne.jp/kanonji/20120206/1328511612
    http://www.atmarkit.co.jp/flinux/rensai/linuxtips/974fsck.html
    http://www.drk7.jp/MT/archives/001365.html

    fsck  -t  ext3  [コンソールに出てくるファイルパス]

    全て yes で進む
    電源オフにして、起動する

