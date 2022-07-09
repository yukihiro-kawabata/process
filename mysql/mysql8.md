# mysql8.0をCentOS7にインストールする

### 既存のMariaDBがあれば削除 
```
# yum -y remove mariadb-libs
# rm -rf /var/lib/mysql
```

### リポジトリに追加してインストール
```
# yum localinstall -y https://dev.mysql.com/get/mysql80-community-release-el7-2.noarch.rpm
# yum install -y mysql-community-server
```

### MySQLの起動と自動起動設定
```
# systemctl start mysqld.service
# systemctl enable mysqld.service
```

### 初期パスワードはこちら
```
# grep 'temporary password' /var/log/mysqld.log
```

### セキュリティ設定
```
# mysql_secure_installation
```

### 必要であれば、認証方式を変更
```
# vi /etc/my.cnf
# 最下部に追記する
default-authentication-plugin=mysql_native_password
```

### 初回ログインできない場合

#### セーフモードでログインする
```
## mysqld_safeコマンドは、CentOS7ではインストールされない
## https://dev.mysql.com/doc/refman/8.0/en/using-systemd.html#systemd-mysql-configuration

# systemctl set-environment MYSQLD_OPTS="--skip-grant-tables"
# systemctl start mysqld
# mysql -u root
```

#### 一旦、パスワードのリセットして退出
```
# UPDATE user SET authentication_string=null WHERE User='root';
# FLUSH PRIVILEGES;
# exit 
```

#### MySQLを再起動する前に環境変数の --skip-grant-tables を削除する
```
### 現状を確認して、削除されていればOK
# systemctl show-environment
# systemctl unset-environment MYSQLD_OPTS
# systemctl show-environment
```

#### MySQLを再起動して、mysqlに入る
````
# systemctl start mysqld
# mysql -u root
````

#### パスワードの再設定
```
> ALTER USER 'root'@'localhost' identified BY 'password';
```

### phpMyAdminが使用できなかったので次のように変更
```
[mysqld]
default_password_lifetime=0
validate_password.length=4
validate_password.mixed_case_count=0
validate_password.number_count=0
validate_password.special_char_count=0
validate_password.policy=LOW
default_authentication_plugin=mysql_native_password


### 全文検索用
ft_min_word_len = 2
innodb_ft_min_token_size = 2
innodb_ft_enable_stopword = OFF
```


### MySQLユーザを追加
```
> CREATE USER 'username'@'localhost' IDENTIFIED BY 'passpass';
```
