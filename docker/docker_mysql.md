<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true ページ番号 -->
<!-- $size: 15:15 縦:横-->
<!-- $theme: gaia -->


# docker mysqlについて

----


# 想定する構成

ホストOS　↔　mysql　→　datadir(ストレージ)
- mysql と ストレージ用のコンテナを用意する
- ストレージへのアクセスはコンテナを必ず通す

---

# まずはストレージ用のコンテナを用意する

---

busyboxとよばれるイメージを利用します。
→busyboxは、基本的なLinuxコマンド群を単一のbusyboxコマンドにまとめたものであり、必要最小限のLinuxシェル環境を提供する場合によく利用されています

---

###### busyboxのイメージをdocker pullにより入手
	docker run -it -v /var/lib/mysql:/var/lib/mysql --name storage01 busybox /bin/sh

###### コンテナ側でディレクトリが作成されたことを確認
    ls -l /var/lib/mysql/

このストレージ用のコンテナを使用していく

---

# mysqlとコンテナを連携させる

---

###### mysql のコンテナを用意する
	docker run --volumes-from storage01 --name mysql56 -e MYSQL_ROOT_PASSWORD= -it -p 3306:3306 yukihirochasuke/develop:mysql56

###### mysql を起動させて、何かデータを入れてみる
	/etc/init.d/mysqld start
    
    mysql -u root -p -A

---

###### mysql のコンテナを削除して、もう一度 docker run する
	docker stop mysql56
    docker rm mysql56

###### mysql コンテナでデータがあるか確認する
	docker exec -it php56 /bin/bash

---

# mysql のコンテナに接続する

---

###### dbサーバーのIPアドレスを調べる
	docker inspect <コンテナID or 名前>  | grep IPAddress

IPを元に接続する

###### docker run のときに、IPアドレスを指定することもできる
	docker run --ip *******
    