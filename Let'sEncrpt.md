<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->

# Let's Encript について

---

###### Let's Encript とは
	Let's Encrypt は無料で自動化されたオープンな認証局 (CA) で、
    公共の利益の為に運営されています。
    Internet Security Research Group (ISRG) が提供するサービスです。

	つまり、
	ドメイン認証の SSL サーバ証明書が無料で簡単に発行できるサービスです

---

###### インストールする
	cd /usr/local/src/
	git clone https://github.com/certbot/certbot

###### テスト実行
	cd certbot/
	./certbot-auto

< NO > を選択して、SSL/TLS サーバ証明書の取得 に進みます。

---

###### Certbot クライアントの実行
	cd /usr/local/src/certbot/
    
	service httpd stop
	
    ./certbot-auto certonly --standalone -d example01.com -d example02.com
	
    service httpd start

###### 下記に公開鍵、中間証明書、秘密鍵が保存される
	/etc/letsencrypt/archive/ドメイン名

---

###### バーチャルホスト設定をする
	sudo cp -p /etc/httpd/conf/httpd.conf /etc/httpd/conf/httpd.conf.`date '+%Y%m%d'`

	vi /etc/httpd/conf/httpd.conf

###### 下記を追加する
    <VirtualHost *:443>
        ServerName example01.com:443
        DocumentRoot /home/example01.com/public
        ErrorLog /var/log/httpd/example01.com_error_ssl_log
        CustomLog /var/log/httpd/example01.com_access_ssl_log common
    <Directory /home/example01.com/public >
    Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
        SSLEngine on
        SSLProtocol all -SSLv2 -SSLv3
        SSLCipherSuite ALL:!ADH:!EXPORT:!SSLv2:RC4+RSA:+HIGH:+MEDIUM:+LOW
        SSLCertificateFile /etc/letsencrypt/live/example01.com/cert.pem
        SSLCertificateKeyFile /etc/letsencrypt/live/example01.com/privkey.pem
        SSLCertificateChainFile /etc/letsencrypt/live/example01.com/chain.pem
    </VirtualHost>
	sudo /etc/init.d/httpd restart

---

###### 設定を反映させる
	service httpd restart
    
---

# 自動更新の設定をする

---

###### 自動更新設定をする
	vi /etc/cron.weekly/01-certbot

###### 下記を追加する
	#!/bin/sh
    
    # 実行日を記録
	/bin/date >> /var/log/certbot.log
    
    # 更新させる
    /etc/init.d/httpd stop
	/usr/local/src/certbot/certbot-auto renew >> /var/log/certbot.log 2>&1
	/etc/init.d/httpd start
    
---

###### 権限をつけて、自動実行されることを祈る
	chmod +x /etc/cron.weekly/01-certbot

90日で期限切れになるので、その前後で更新されているか確認すればいいと思います