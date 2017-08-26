<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true ページ番号 -->
<!-- $size: 15:15 縦:横-->
<!-- $theme: gaia -->


# Docker Hub の使い方

---

Docker Hubはユーザーが作成したコンテナをアップロードして公開・共有できるサービスで、ここで公開されているコンテナは自由にダウンロードして自分のサーバーに簡単にデプロイできる。公開されているコンテナは「リポジトリ」という単位で管理され、タグを使ったバージョン管理も可能だ。

Docker HubはDockerを開発するDocker社が営利目的で開発・運営しているサービスだが、無料ですべての機能が利用可能だ。無制限に作成できる公開リポジトリ（パブリックリポジトリ）のほか、一般公開されないプライベートリポジトリも作成できる。無料ユーザーの場合作成できるプライベートリポジトリは1つだけだが、有料プランを利用することで作成できるプライベートリポジトリの数を増やすことができる。

---

# Docker をインストール

---

###### Docker をインストール CentOS6.x
	wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
	rpm -ivh epel-release-6-8.noarch.rpm
	yum install -y docker-io
    service docker start

---

#  Docker Hubのアカウント作成

---

<a href='https://hub.docker.com/'>Docker Hub</a> にWebブラウザでアクセスする
アカウント登録画面（「Create your Docker account」画面）が表示されるので、ここにユーザー名およびパスワード、メールアドレスを入力して「Sign up」をクリックする

登録したメールアドレス宛に確認用のメールが届くので、そこに記載されているURL、もしくは「Confirm Your Email」リンクをクリックすることで認証が完了する。

---

# Docker Hubに作成したコンテナをアップロードする

---

1. コンテナをアップロードするリポジトリ名を決める
2. アップロードするコンテナにというタグを付与する<br />＜Docker Hubのアカウント名＞/＜リポジトリ名＞:＜タグ名＞
3. コンテナをリポジトリにアップロード

---

###### 作成した コンテナ のIDを調べる
	[root@docker ~]# docker ps
	CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS             	  NAMES
	46cb81341b30        d3f83d1ec086        "bash"              49 minutes ago      Up 30 minutes                           d3f83d1ec086

###### 作成したイメージをコミットする
	docker commit -m "コメントを書く" ＜コンテナID＞ ＜リポジトリ名＞:＜タグ名＞

---

###### 作成した image のIDを調べる
	[root@docker ~]# docker images
	REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	centos              centos6             ab14631f6f47        2 weeks ago         194.3 MB


###### コミットしたイメージにタグをつける
	docker tag ＜image ID＞ ＜リポジトリ名＞:＜タグ名＞

###### docker Hubにプッシュする
	docker push ＜リポジトリ名＞:＜タグ名＞

---

###### 設定を確認する
	[root@docker ~]# docker images
	REPOSITORY                TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
	centos                    centos6             ab14631f6f47        2 weeks ago         194.3 MB

	
<font color='red'>※公開リポジトリは、IMAGEの中身に注意すること</font>

アップロードが完了すると、Docker Hubに情報が反映される

###### リポジトリの image を更新する
	このときは、コミットするときに更新したいイメージを指定してやれば
    問題ありません。コミットした後は、docker Hubにプッシュして完了です
