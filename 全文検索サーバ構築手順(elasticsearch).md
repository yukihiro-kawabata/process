<!-- 参考URL marpの設定 http://qiita.com/pocket8137/items/27ede821e59c12a1b222 -->
<!-- page_number: true -->
<!-- $size: 1:5 縦:横-->
<!-- $theme: gaia -->

# 全文検索サーバ（Elasticsearch編）

---

###### 参考URL
	https://eure.jp/2014/06/27/fluentd_elasticsearch_kibana/
	http://www.karakaram.com/installing-fluentd-elasticsearch5-kibana5#install-kibana
	https://www.skyarch.net/blog/?p=9668

###### ダウンロード
 	https://www.elastic.co/jp/downloads/elasticsearch

----

###### レポジトリの追加
	vi /etc/yum.repos.d/elasticsearch.repo

###### 下記追加
	[elasticsearch-2.x]
	name=Elasticsearch repository for 2.x packages
	baseurl=http://packages.elastic.co/elasticsearch/2.x/centos
	gpgcheck=1
	gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
	enabled=1

----

###### Elasticsearch をインストール
	yum install elasticsearch

###### Elasticsearch の自動起動
	chkconfig elasticsearch on
    
---

###### Elasticsearchのメモリ設定
	cp /etc/sysconfig/elasticsearch /etc/sysconfig/elasticsearch.`date '+%Y%m%d'`
    
	vi /etc/sysconfig/elasticsearch

 	# メモリ量を指定する
	ES_HEAP_SIZE=8g  
上記は8GBに設定する場合の例

実サーバのメモリサイズの半分が理想

Elasticsearch 専用サーバなら実サーバのメモリサイズの1～2GB下回るくらいが理想

---

###### elasticsearchのバージョンが5系以上になると設定項目がないので、調整する
	vi /etc/elasticsearch/jvm.options

---

###### 何処からでも繋げるように設定する
	vi /etc/elasticsearch/elasticsearch.yml

###### 下記追加
	http.cors.allow-origin: "*"
	http.cors.enabled: true
    
    # IP指定で繋げたい場合は、該当IPを設定する
	network.host: 0.0.0.0

###### 再起動する
	/etc/init.d/elasticsearch restart

---

###### 上記のnetwork.host: をいじってとエラーが出たら
	vi /etc/security/limits.conf

###### 下記追加
	elasticsearch             soft    nproc           2048
	elasticsearch             hard    nproc           2048

	elasticsearch - memlock unlimited
	root - memlock unlimited

----

###### java をインストール
	yum install java-1.8.0-openjdk

###### Elasticsearchの起動
	/etc/init.d/elasticsearch start
	chkconfig elasticsearch on


###### kuromoji を入れる
	/etc/init.d/elasticsearch stop
	/usr/share/elasticsearch/bin/plugin install analysis-kuromoji
	/usr/share/elasticsearch/bin/plugin install mobz/elasticsearch-head

---

###### kuromoji をデフォルトにする
	vi /etc/elasticsearch/elasticsearch.yml

###### 下記編集または、追記
	# set kuromoji as default Tokenizer
	index.analysis.analyzer.default.type: custom
	index.analysis.analyzer.default.tokenizer: kuromoji_tokenizer

----

###### java で mysql を繋げられるように


###### ダウンロード元はこちら 		
	https://dev.mysql.com/downloads/connector/j/

##### ドライバーをインストール
	wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.42.zip
	unzip mysql-connector-java-5.1.42.zip
	rm mysql-connector-java-5.1.42.zip

---

###### バスを通す
	cp mysql-connector-java-5.1.42-bin.jar /usr/share/java
	
    vi /etc/bashrc
    # もしくは
    vi /root/.bashrc

###### 下記追加
	export CLASSPATH=$CLASSPATH:/usr/share/java/mysql-connector-java-5.1.42-bin.jar

###### 反映させる
	source /etc/bashrc
    # もしくは
    source /root/.bashrc

---

#### elasticsearch の plugin をインストール <br />River がなんかできないから飛ばす
	#/usr/share/elasticsearch/bin/plugin --install jdbc --url http://xbib.org/repository/org/xbib/elasticsearch/plugin/elast#csearch-river-jdbc/1.2.1.1/elasticsearch-river-jdbc-1.2.1.1-plugin.zip

	#cp /usr/share/java/mysql-connector-java-5.1.42-bin.jar /usr/share/elasticsearch/plugins/jdbc/

---

# River プラグインの代わりに logstash を使う

---

###### リポジトリ追加
	vi /etc/yum.repos.d/logstash.repo

###### 下記追加
	[logstash-2.3]
	name=Logstash repository for 2.3.x packages
	baseurl=http://packages.elastic.co/logstash/2.3/centos
	gpgcheck=1
	gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
	enabled=1

---

###### インストールする
	yum install logstash
	service logstash start
	chkconfig logstash on
	chkconfig logstash --list

###### 試験的にデータをmysqlから入れてみる ファイルの名前は何でも良い
	vi /usr/local/logstash/＜テキトーな名前をつける＞.conf

---


###### 下記追加
	input {
  		jdbc {
	    		jdbc_driver_library => "/usr/share/java/mysql-connector-java-5.1.42-bin.jar"
    		jdbc_driver_class => "com.mysql.jdbc.Driver"
    		jdbc_connection_string => "jdbc:mysql://localhost:3306/＜インデックス名＞"
    		jdbc_user => "MySQLのユーザ"
    		jdbc_password => "MySQLのパスワード"
    		statement => "SELECT * from tableName"
  		}
	}

		output {
  			elasticsearch {
			hosts => ["localhost"]
    		index => "＜インデックス名＞"
  		}
	}


---

###### データを入れる
	/opt/logstash/bin/logstash -f /usr/local/logstash/<テキトーな名前をつける>.conf

###### 成功したか、確認する
	curl localhost:9200/_cat/indices



