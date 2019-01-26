# dockerをインストールする
バージョン：linux mint 19

# apt-getコマンド拡張する
````
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
````

# GPGキー追加
正しい配布先からパッケージを入手できるようにDockerのGPGキーを追加します
````
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
````

# リポジトリ追加
````
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
````

# apt-getを更新
````
sudo apt-get update
````

# docker-ceをインストールします。
````
sudo apt-get install docker-ce
````

# インストールできるdocker-ceのバージョン確認
````
apt-cache madison docker-ce
````

# インストール
※docker-ce=[バージョン]を入れる
````
sudo apt-get install docker-ce=18.06.0~ce~3-0~ubuntu
````

# dockerを起動する
```
systemctl start docker
```

