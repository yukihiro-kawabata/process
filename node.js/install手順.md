# node.jsをCentOS7.xにインストールする
参考：URL 
        <a href="https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora" target="_blank" rel="noopener noreferrer">
            https://nodejs.org/en/download/package-manager/#enterprise-linux-and-fedora
        </a>

# yum でインストールする
- nodesourceというサードパーティリポジトリからのインストール
- yumで管理したほうがセキュリティアップデートを扱いやすいみたい

# リポジトリ追加
````
curl -sL https://rpm.nodesource.com/setup_8.x | bash -
````

# インストール
### 今回は他にも必要なものをついでに入れる
````
yum install nodejs gcc-c++ make
````