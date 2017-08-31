<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->

# Jenkins 導入(docker編)

参考URL
http://knjname.hateblo.jp/entry/2015/02/10/040833

---

###### 永続ディレクトリをマウントさせる
	docker run -p 8080:8080 -v /home/jenkins:/var/jenkins_home jenkins

データはコンテナ内の /var/jenkins_home に作成されます

あとは、基本的に自力でJenkinsをインストールしたときと同じ

---

###### 起動させる
	service jenkins start

###### 自動起動の設定
	chkconfig jenkins on



###### ポート開ける
	vi /etc/sysconfig/iptables

###### 下記一行追加
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT

---

###### 変更を反映させる
	/etc/init.d/iptables restart

###### 映してみる
	http://＜IPアドレス＞:8080/

###### パスワード確認
	cat /var/lib/jenkins/secrets/initialAdminPassword
    
出てきた数字でログインができる

---

# ジョブ作成
参考URL
http://confluence.sharuru07.jp/pages/viewpage.action?pageId=2883643

