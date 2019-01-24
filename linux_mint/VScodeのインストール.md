# linux mint に VScodeをインストールする
### バージョン : linux mint 19

# 公式ページにある通りにリポジトリ追加
<a href="https://code.visualstudio.com/docs/setup/linux">https://code.visualstudio.com/docs/setup/linux</a>
`````	
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
`````

# インストールする
````
sudo apt-get update
sudo apt-get install code # or code-insiders
`````

# 起動する
````
# バージョンを調べる
code --version
# 起動する
code
`````
