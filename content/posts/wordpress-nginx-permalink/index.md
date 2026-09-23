---
title: "WordPress「返答が正しいJSON レスポンスではありません。」の原因はNginxのパーマリンクが変更不可のため"
url: "/programming/wordpress-nginx-permalink/"
date: 2020-03-29
categories: 
  - "wordpress"
  - "programming"
---

さくらのVPSへNginxでWebサーバを構築し、WordPressをインストールして新しい記事を投稿しようとしたら**「返答が正しいJSON レスポンスではありません。」**のエラーが出た。

環境は以下

- さくらのVPS
- CentOS 7
- Nginx 1.17
- PHP 7.4
- MariaDB

※Webサーバ環境構築については、[【さくらVPS】CentOS7+Nginx+PHP7+MariaDBの環境構築](https://blog.pop-web.net/server/linux/nginx-php-mariadb/)を参照。

## 原因について

調べてみると、**管理画面の「設定」→「パーマリンク設定」で共通設定を基本以外を選択しているとエラー**がでるが、基本にするとちゃんと投稿される。

これはNginxのデフォルトの設定だと、パーマリンクのURL変更が出来ないこと原因らしい。

## Nginxの設定を変更することで解決

Nginxの設定ファイルである`/etc/nginx/conf.d/default.conf`を以下のように変更することで解決できる。

**※各自`default.conf`の設定が異なる場合があるので、以下の内容を使用する場合は、各自の環境に合わせていく必要がある。**

```
server {
    listen 80;
    server_name { ドメイン名 };
    root /home/www/{ ドメイン名 }/wordpress;
    index index.php

    charset utf-8;

    location / {
        try_files $uri $uri/ @wordpress;
    }

    location ~ \.php$ {
        try_files $uri @wordpress;
        fastcgi_index index.php;
        fastcgi_split_path_info ^(.+\.php)(.*)$;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME  /home/www/{ ドメイン名 }/wordpress$fastcgi_script_name;
        include       fastcgi_params;
    }

    location @wordpress {
        fastcgi_index index.php;
        fastcgi_split_path_info ^(.+\.php)(.*)$;
        fastcgi_pass  127.0.0.1:9000;
        fastcgi_param SCRIPT_FILENAME /home/www/{ ドメイン名 }/wordpress/index.php;
        include       fastcgi_params;
    }

    error_page 404 /404.html;
    location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
    }
}
```
