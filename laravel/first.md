###### 参考URL 
    https://readouble.com/laravel/5.4/ja/installation.html
    https://laravel10.wordpress.com/2015/02/16/%E5%88%9D%E3%82%81%E3%81%A6%E3%81%AElaravel-5-2-%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%AE%E4%BD%9C%E6%88%90/

---------

必要な環境

- PHP >= 5.6.4
- OpenSSL PHP拡張
- PDO PHP拡張
- Mbstring PHP拡張
- Tokenizer PHP拡張
- XML PHP拡張

composer のインストール
    curl -sS https://getcomposer.org/installer | php
    mv composer.phar /usr/local/bin/composer


composer global require "laravel/installer"

###### 環境変数を設定
PATH=$PATH:~/.composer/vendor/bin
export PATH


###### プロジェクト作成
laravel new プロジェクト名

これでフォルダも作成された

###### 上手くいかない場合
composer create-project --prefer-dist laravel/laravel プロジェクト名


###### サーバ側で設定
ドキュメントルートを publicディレクトリになるように設定

###### 権限設定
chmod -R 777 storage
chmod -R 777 bootstrap/cache

###### アプリケーションキーの設定
.env で設定する

###### 初期設定
vi config/app.php

 ### 下記変更
 timezone を Asia/Tokyo
 locale を ja
 
###### 設定を保存
php artisan serve

###### mvcなど作成
Controller

app/Http/Controllers
php artisan make:controller TestController


Model

app/
php artisan make:model TestModel


View

resources/views

