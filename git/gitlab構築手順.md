
# GitLab手順

### 参考サイト
<a href="https://about.gitlab.com/downloads/">ダウンロード手順</a>
<a href="http://blog.katashiyo515.com/entry/2014/05/10/132205">ブログ</a>


ダウンロード
<a href="https://about.gitlab.com/downloads/archives/">ダウンロード先</a>
<a href="https://packages.gitlab.com/gitlab/gitlab-ce?filter=rpms">ダウンロード一覧</a>

<a href="http://ez-net.jp/article/42/LMtSMqNF/76wV8fNmgSLz/">GitLabのHTTPポート番号を変更</a>


---

### 必要なものをダウンロードする
    yum install postfix
    yum install cronie
    yum install openssh-server
    service postfix start
    chkconfig postfix on
    lokkit -s http -s ssh

----

### 今回はCentOS6xで行う
	cd ~
	curl -O https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/6/gitlab-ce-8.2.0-ce.0.el6.x86_64.rpm
	rpm -i gitlab-ce-8.2.0-ce.0.el6.x86_64.rpm

---


### 設定ファイルの作成
	mkdir /etc/gitlab
	vi /etc/gitlab/gitlab.rb

### URLの設定
	external_url "http://[IPアドレス]:[ポート番号]"

### 設定を反映
	gitlab-ctl reconfigure

---

### gitlabのポート番号を指定する
	vi /etc/gitlab/gitlab.rb
	
    # 下記変更
	external_url 'http://localhost:[ポート番号]'


### 設定を反映させる
	gitlab-ctl reconfigure

---

### 使用するポートを開放する
	vi /etc/sysconfig/iptables
	
    # 下記追加
	-A RH-Firewall-1-INPUT -p tcp -m tcp --dport [ポート番号] -j ACCEPT
	-A RH-Firewall-1-INPUT -p tcp -m tcp --sport [ポート番号] -j ACCEPT

---

### 502エラーが出ることがある
	なんか次の日に見たら映ってた（原因不明）

### IDパスワード
	Username: root
	Password: 5iveL!fe

