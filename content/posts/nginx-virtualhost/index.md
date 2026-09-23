---
title: "【さくらVPS】Nginxバーチャルホストの設定"
url: "/server/linux/nginx-virtualhost/"
date: 2020-01-03
categories: 
  - "linux"
---

前回の[【さくらVPS】Nginxのインストール手順](https://blog.pop-web.net/programming/nginx-install/)からの続きで

バーチャルホスト設定を行なっていく。

## Nginxを動作させるシステムユーザーを作成

Nginxを動作させるシステムユーザを新たに作成する。

デフォルトではNiginxユーザとなっているがセキュリティ上、新たにシステムユーザを作成することとする。

wwwというユーザー名で作成する。

シェルとして「/sbin/nologin」を指定しログイン権限を制限。

```
sudo useradd -s /sbin/nologin www
```

パスワードを設定

```
sudo passwd www
```

## コンテンツディレクトリ作成

バーチャルホストを設定していくので、URLに応じた各コンテンツを格納するドキュメントルートとなるディレクトリを作成する。

「/home/www/」の下にドキュメントルートを作成し、ファイル名はドメイン名とする。

今回は「blog.pop-web.net」というディレクトリ名で作成する。

```
sudo mkdir /home/www/blog.pop-web.net
```

作成したドキュメントルートの中に、テスト用のHTMLファイルを作成しておく。

```
sudo vi /home/www/blog.pop-web.net/index.html
test
```

作成したディレクトリは、所有者がrootユーザになっているので、

「www」ユーザへパーミッションの変更をする。

```
sudo chown -R www:www /home/www
```

## nginx.confファイルの変更

Nginxの全般的な設定ファイルは、「nginx.conf」で行う。ちなみに、サイト毎の設定などは、「default.conf」で行う。

「nginx.conf」のバックアップをしておく。

```
sudo cp -p /etc/nginx/nginx.conf /etc/nginx/nginx.conf.org
```

「nginx.conf」を編集していく。

```
sudo vi /etc/nginx/nginx.conf
```

### システムユーザーを変更

システムユーザをwwwへ変更。

```
user nginx;
↓
user www;
```

### バージョンの非表示

バージョンを非表示するため「server\_tokesn off;」を追加

```
http {
    include /etc/nginx/mime.types;
↓
http {
    server_tokens off;
    include /etc/nginx/mime.types;
```

### gzip圧縮の有効

表示速度が早くするため、サイズを圧縮する機能であるgzipを有効化する。

```
#gzip on;
↓
gzip on;
```

### 長いホスト名の対応

長いバーチャルホスト名の場合、エラーになる場合があるのでサイズを大きめに設置する。

gzip on;の下へ追記。

```
server_names_hash_bucket_size 128;
```

上記の設定が終わったら上書き保存。

確認用コマンドで「nginx.conf」に間違いがないかチェック。

```
sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

上記のように、「syntax is ok」「test is successful」と表示されればOK。

## default.confファイルの変更

default.confファイルにバーチャルホスト設定を編集していく。

以下のコマンドでdefault.confファイルを開く。

```
sudo vi /etc/nginx/conf.d/default.conf
```

最終行に以下の内容を追記する。

```
server {
    listen 80;
    server_name blog.pop-web.net www.blog.pop-web.net;
    access_log /var/log/nginx/blog.pop-web.net-access.log main;
    error_log /var/log/nginx/blog.pop-web.net-error.log;
    root /home/www/blog.pop-web.net;
    location / {
        index index.html index.htm;
    }
}
```

各設定項目の説明は以下のよう。

| 項目 | 説明 |
| --- | --- |
| listen | バーチャルホストが待ち受けるポートの指定 |
| server\_name | バーチャルホスト名の設定。www有りとwww無しの両方でアクセスできるようにしておく。 |
| access\_log | アクセスログの出力先の指定 |
| error\_log | エラーログの出力先の指定 |
| root | ドキュメントルートの指定 |
| location | URIのパス単位の設定 |

追記したら、確認用コマンドで「default.conf」に間違いがないかチェック。

```
sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

上記のようにsuccessfulがでればOK。

## Nginx再起動

設定内容を反映させるため、Nginxを再起動する。

```
sudo systemctl restart nginx
```

## ブラウザで確認

ブラウザで「blog.pop-web.net」を入力して表示確認する。

作成したテスト用のHTMLファイルの内容が表示されればOK。

※この動作確認では、DNS設定の必要で、ドメイン名（blog.pop-web.net）からIPが引けるようにしておく。ドメインをお名前ドットコム等のサービスで取得している場合は、ネームサーバ設定変更で、さくらのVPSのネームサーバを指定しておけば良い。
