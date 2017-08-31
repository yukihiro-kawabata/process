<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->

# Jenkins 導入

参考URL

https://t32k.me/mol/log/vagrant1-2-centos6-4-jenkins1-5/

http://www.buildinsider.net/enterprise/jenkins/001#forlinux

http://symfoware.blog68.fc2.com/blog-entry-1899.html


----

# Jenkins をブラウザで映す

---

###### リポジトリに追加
	wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
	rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key

###### インストール
	yum install jenkins

###### 起動させる
	service jenkins start

###### 自動起動の設定
	chkconfig jenkins on

----

###### ポート開ける
	vi /etc/sysconfig/iptables

###### 下記一行追加
	-A INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT

###### 設定を反映させる
	/etc/init.d/iptables restart

###### 映してみる
	http://＜IPアドレス＞:8080/


---

###### パスワード確認
	cat /var/lib/jenkins/secrets/initialAdminPassword
    
出てきた数字でログインができる

---

# ジョブ作成
参考URL
http://confluence.sharuru07.jp/pages/viewpage.action?pageId=2883643

