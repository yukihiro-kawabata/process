# pythonインストール手順
参考URL：https://pythondatascience.plavox.info/python%e3%81%ae%e3%82%a4%e3%83%b3%e3%82%b9%e3%83%88%e3%83%bc%e3%83%ab/anaconda-ubuntu-linux
### 環境
- ubuntu19

anacondaを利用してインストールします。<br />
<a href="https://www.continuum.io/downloads">anacondaの公式サイト</a>

上記のサイトでダウンロードしたものを実行します
````
/bin/sh ./Anaconda3-4.1.0-Linux-x86_64.sh
````
実行中に出てくるものは、「ライセンスに同意」「インストール場所」「Pathを通すか」を尋ねられます。<br />
こだわりがなければすべて yes でいいでしょう

# インストールの確認
````
conda -V
````
バージョンが出てくればOK
```
jupyter notebook
```
ブラウザ起動して jupyter の画面が立ち上がればOK