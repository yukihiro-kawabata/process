# LAMP開発環境を整える
バージョン：Linux Mint19

# Apacheのインストール
````
sudo apt-get install apache2
````
インストール後、ブラウザでlocalhostと入力すると画面が映ります

# phpのインストール
※必要なものを適宜調整して入れる
````
# 今回は以下で行った
sudo apt-get install php
sudo apt-get install php-mysql php-opcache php-mbstring php-pear php-gd
````

# apacheとphpを連携させる
````
sudo apt-get install libapache2-mod-php
````

# MySQLのインストール
````
sudo apt-get install mysql-server
````

# phpでMySQLに接続するためのモジュール
````
sudo apt-get install php-mysql
````

# phpmyadminを入れる
````
sudo apt-get install phpmyadmin
````
※インストール後、ブラウザでlocalhost/phpmyadminにアクセスしても404エラーになります。

### apacheの設定ファイル/etc/apache2/apache2.confに以下を追記します。
````
Include /etc/phpmyadmin/apache.conf
````

# 変更した設定ファイルの内容を反映
````
systemctl restart apache2
````
ブラウザでlocalhost/phpmyadminにアクセスすると、phpmyadminのログイン画面が表示される

ユーザ名はrootで、パスワードはmysqlをインストールした時に設定したパスワードを入力すればOK！