<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->

# ownCloud について

###### 参考URL
	https://www.virment.com/setup-owncloud/#MySQLownCloud

---

###### ownCloud とは
	DropBoxができることは全てできそうです。
    自分のローカルPC内のフォルダをownCloud用フォルダに指定して、
    そのフォルダ内のファイルを更新すれば、
    ownCloudサーバ内のファイルが自動更新されます。
    また、DropBox同様、違うPCにあるownCloud用フォルダも自動で同期されます。
    複数ユーザ用の共有フォルダも作成できます。
    さらに自分でサーバを用意するのでもちろん容量制限はなく、
    ownCloudが動作するサーバのハードディスク容量分使う事ができます。

----

###### 環境を用意する
	Apache2
	MySQL
	PHP

###### PHP をインストール
	yum install apr-devel apr-util-devel
    
	rpm -Uvh http://ftp-srv2.kddilabs.jp/Linux/distributions/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
    
    yum install --enablerepo=remi --enablerepo=remi-php70 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pear php-gd php-pecl-zip php-pecl-xdebu

これで、Apacheも入っているはず

---

###### MySQL をインストール
	yum -y install http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
	yum -y install mysql-community-server

---

###### MySQLでownCloud用のデータベースとユーザを作成 	
	mysql> create database owncloud default character set utf8;
    mysql> grant all on owncloud.* to owncloud@localhost identified by 'passpass';

###### ownCloud のソースをダウンロード
<a href="http://owncloud.org/install/">サイトはこちら</a>
＊＊＊＊.tar.bz2 のものをダウンロード

######
	wget https://download.owncloud.org/community/owncloud-10.0.2.tar.bz2

---

###### 解凍する
	tar -xjf owncloud-10.0.2.tar.bz2

###### ドキュメントルートにフォルダを移動させる
	mv owncloud /var/www/html

###### 権限をつける
	chown -R apache:apache /var/www/html/owncloud/

###### ブラウザで映してみる
	http://＜IPアドレス＞/owncloud

---

### ブラウザに映ったら設定をする

---

- 初めに管理者権限のユーザを設定する
- database は MySQL を指定する

###### MySQLの設定
	
    Database user : owncloud
    Database password : passpass
    Database name : owncloud
    Database host : localhost

---

###### セットアップ中にMySQLでエラーが出たとき
	vi /etc/my.cnf
    
    [mysqld]
    innodb_file_per_table
	innodb_file_format = Barracuda
	innodb_file_format_max = Barracuda

	innodb_large_prefix=ON

	/etc/init.d/mysqld restart

