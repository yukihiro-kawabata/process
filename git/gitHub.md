<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->


# Git Hub の使い方

---

# アカウント作成

---

<a href="github.com">git Hub</a>にアクセスして、取得したいユーザー名、メールアドレス、設定したいパスワードを入力し、「Sign up for GitHub」をクリック


個人で使用するならまずは無料版でよいでしょう。「Unlimited public repositories for free.」を選択し、「Continue」をクリック

---

# git をインストール

---
###### インストール
	yum install git

###### ユーザ名登録
	# git config --global user.name "userName"
	# git config --global user.email info@example.com
    
---

# ssh-keygen を発行・登録する

---

###### ssh-keygen を発行
	ssh-keygen -t rsa -C "info@example.com"　←さっき入力したメールアドレス
    パスフレーズを聞かれるが、空で発行しても問題ない

###### 登録する
	cat ~/.ssh/id_rsa.pub

###### 上記で出てきたものを、ブラウザから登録する
	https://github.com/settings/keys

---

###### いい感じに権限とつけておく
	chmod -R 700 ~/.ssh


###### 下記を設定する
	vi ~/.ssh/config
    
    下記追記
    
	HOST GITLAB
	HostName     github.com
	User    info@example.com
	IdentityFile    ~/.ssh/id_rsa.pub


---

# リポジトリ作成

---

new repository をクリックして
リポジトリ名、公開/非公開、Initialize this repository with a README を選択してcreate repository をクリック

###### コンソール側でgitHubにつながるか確認
	ssh github.com

###### 任意の場所でgit clone
	git clone https://github.com/yukihiro-kawabata/shell.git
    
---

# ファイル追加して、コミットする

---

###### クローンしたリポジトリのディレクトリに移動する

###### ブランチ確認
	# git branch
	* master  ←＊が付いているブランチが現在の居場所

###### ブランチ作成
	# git branch develop
    
    # git branch
  	develop
	* master

---

###### ブランチ移動 
	# git checkout develop
	Switched to branch 'develop'
	
    # git branch
	* develop
  	master
    
###### ステージング領域にファイルを上げる
	# vim df_alert.sh
    # git add df_alert.sh

---

###### ステージング領域にファイルが上がったか確認する
	# git status
    
	 On branch develop
	 Changes to be committed:
	   (use "git reset HEAD <file>..." to unstage)
	
	       new file:   df_alert.sh
	
    
---

###### コミットする
	# git commit -m "サーバ容量監視シェル" df_alert.sh

git commit -a で全てをコミットできる

###### 状態を確認する
	# git status
	On branch develop
	nothing to commit, working tree clean

これで、コミットされた

---

###### GitHubへプッシュする
	git push origin develop

###### 初回だとエラーが出るかもしれない
	# git push origin develop
	Password:
	error: The requested URL returned error: 403 Forbidden while accessing https://userName@github.com/yukihiro-kawabata/shell.git/info/refs

	fatal: HTTP request failed

----

###### 上記の解決方法 (プロトコルをsshにする)
	vi .git/config
    
    [remote "origin"]
        url = ssh://git@github.com/username/hoge.git
        
---

###### プッシュされたか、ブラウザで確認する
	GitHubの画面からプルリクエストを出してみましょう。
    「Compare & pull request」
    もしくは「New pull request」をクリックします。

	新たにコミットしたdevelopブランチから、
    masterブランチへ向けてプルリクエストを送るため、
    「base」（リクエスト受信側）が「master」ブランチ
    「compare」（リクエスト送信側）が「develop」ブランチとなります。

	プルリクエストのコメントのやり取りが完了したら、
    プルリクエストを終了させます。
    「Merge pull request」をクリックしましょう。

---

# ブランチを削除する


---

###### master ブランチに移動して、ローカル環境を最新にする
	git checkout master
    git pull

###### ブランチを削除する
	git branch -d develop