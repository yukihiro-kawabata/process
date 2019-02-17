# nginxをCentOS7.xにインストールする
参考URL：<a href="https://www.saintsouth.net/blog/install-nginx-on-centos7/" target="_blank" rel="noopener noreferrer">
            https://www.saintsouth.net/blog/install-nginx-on-centos7/
        </a>

# Nginx リポジトリのインストール
````
yum install http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
````

# Nginx パッケージのインストール
````
yum install --enablerepo=nginx nginx
````
