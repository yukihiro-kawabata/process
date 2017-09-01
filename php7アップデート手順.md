<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->

# PHP56 → PHP7 <br />アップデート手順

---

###### 今のモジュールを確認
	php -m

###### php.iniのバックアップ
	cp /etc/php.ini{,.update70}

###### サービスを止める
	/etc/init.d/httpd stop

---

###### httpd-develがあるかを確認する
	yum list installed | grep httpd-devel
    # 無ければインストール
	yum install -y httpd-devel

###### apxsが必要
	yum install apr-devel apr-util-devel

###### httpd-devel に失敗したときは、手動でPHPモジュールを記入するのでコピーをとっておく
	cp /etc/httpd/conf/httpd.conf{,.`date +%Y%m%d`}

---

###### 既存のPHP確認
	php -v

###### CentOSのバージョンを確認する
	cat /etc/redhat-release

###### CentOS6.xのとき
	rpm -Uvh http://ftp-srv2.kddilabs.jp/Linux/distributions/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

---

###### CentOS7.xのとき
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
	rpm -Uvh remi-release-7*.rpm

###### yum listで提供されているパッケージを確認
	yum list --enablerepo=remi --enablerepo=remi-php70 | grep php

###### PHP7.0がない場合は yum update
	# リポジトリがあるか確認
	rpm -qa | grep remi
	rpm -qa | grep epel
	# アップデート
	yum update -y --enablerepo=remi --enablerepo=remi-php70

---

###### PHP7.0がある場合は入れなおす
	# 既存のPHP削除
	yum remove -y php php-devel php-pear php-cli php-common php-gd php-ldap php-mbstring php-mysql php-odbc php-pdo php-xml

	# PHP7.0をインストール （xhprofがあるとエラーになった）
	yum install --enablerepo=remi --enablerepo=remi-php70 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pear php-gd php-pecl-zip php-pecl-xdebug ImageMagick php-pecl-imagick php-mhash

###### imagick が入らない場合
	pecl install imagick
    ・
    ・
    （中略）
    installation [autodetect] : autodetect ← 入力する

---

###### php.iniを戻す
	cp /etc/php.ini.update70 /etc/php.ini


###### php.iniに imagick が使えるように記述する
	# 下記記述
	extension=imagick.so
※ pecl install imagick で入れたときのみ

---

###### imagic の動作確認
	convert -version

###### ライブラリ確認
	ldconfig -p | grep libmagic

###### モジュールを確認、バージョンアップ前と比較
	php -m


###### アパッチを起動させる
	/etc/init.d/httpd start


---

###### ----- 必要があれば php.ini も編集する ----------
	vi /etc/php.ini

error_reporting = E_ALL & ~E_NOTICE
など

