###### 参考URL 
    https://readouble.com/laravel/5.4/ja/installation.html
    https://laravel10.wordpress.com/2015/02/16/%E5%88%9D%E3%82%81%E3%81%A6%E3%81%AElaravel-5-2-%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%AE%E4%BD%9C%E6%88%90/

---------

必要な環境

- PHP >= 7.0
- OpenSSL PHP拡張
- PDO PHP拡張
- Mbstring PHP拡張
- Tokenizer PHP拡張
- XML PHP拡張

---------

###### composer のインストール
    curl -sS https://getcomposer.org/installer | php
    mv composer.phar /usr/local/bin/composer

###### composer でlaravelをインストール
    composer global require "laravel/installer"

---------

###### 環境変数を設定
    PATH=$PATH:~/.composer/vendor/bin
    export PATH

###### プロジェクト作成
    laravel new プロジェクト名

これでフォルダも作成された

---------

###### 上手くいかない場合
    composer create-project --prefer-dist laravel/laravel プロジェクト名


###### サーバ側で設定
ドキュメントルートを publicディレクトリになるように設定

###### 権限設定
    chmod -R 777 storage
    chmod -R 777 bootstrap/cache

---------

###### 環境設定
```
cp .env.example .env
```

###### アプリケーションキーの設定
```
php artisan key:generate
```

###### 初期設定
vi config/app.php

 ### 下記変更
 timezone を Asia/Tokyo
 locale を ja
 
###### 設定を保存
php artisan serve

---------

###### mvcなど作成
###### app/Http/Controllersにコントローラが作成される
    php artisan make:controller TestController

###### app/にモデルが作成される
    php artisan make:model TestModel

###### resources/viewsにビューが作成される

----------

##### バーチャルホスト設定

    <VirtualHost *:80>
        DocumentRoot C:/xampp/htdocs
        ServerName localhost
    </VirtualHost>
        <Directory "C:/xampp/htdocs">
            AllowOverride All
            Require all granted
        </Directory>


    ############################## local.laravel.jp
    <VirtualHost *:80>
        DocumentRoot C:\workspace\home\laravel.jp\public
        ServerName laravel.jp
        ServerAlias local.laravel.jp
        ErrorLog logs/laravel.jp_error.log
    </VirtualHost>
        <Directory "C:\workspace\home\laravel.jp\public">
            Options Indexes FollowSymLinks Includes ExecCGI
            AllowOverride All
            Require all granted
        </Directory>
    
    ------------------  
    
    # Linuxの場合は下記を記載する
    
    <VirtualHost *:80>
        ServerName localhost
        DocumentRoot /var/www/html/
    </VirtualHost>
    <Directory /var/www/html/ >
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>

    <VirtualHost *:80>
        ServerName laravel
        ServerAlias vm.laravel.jp
        DocumentRoot /home/laravel/public
        ErrorLog /var/log/httpd/laravel_error_log
        CustomLog "logs/dos_suspect_log" dos_suspect env=SuspectDoS
        CustomLog /var/log/httpd/laravel_access_log combined env=!no_log
    </VirtualHost>
    <Directory /home/laravel/public >
        Options Indexes FollowSymLinks Includes ExecCGI
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
