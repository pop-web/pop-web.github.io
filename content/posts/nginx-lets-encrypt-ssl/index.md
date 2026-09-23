---
title: "【さくらVPS】NginxをLet's EncryptでSSL化させる"
url: "/server/linux/nginx-lets-encrypt-ssl/"
date: 2020-04-13
categories: 
  - "linux"
  - "server"
---

さくらのVPSにてLet's EncryptでSSL化させる手順を説明する。

## 環境について

環境については以下の通り。

- さくらのVPS
- CentOS 7
- Nginx 1.17

## Certbotインストール

適切な場所移動してgit cloneする。

```
cd /usr/local
git clone https://github.com/certbot/certbot
```

certbotに移動して確認

```
cd certbot/
./certbot-auto --version
certbot 1.4.0
```

## 証明書の設定

Nginxは一旦停止する。

```
systemctl stop nginx
```

また、80番、443番のポートは開けておく。

以下のコマンドで証明書の設定を実行する。

```
./certbot-auto certonly --standalone -d <SSL化したいドメイン> -m <連絡用のメールアドレス> --agree-tos -n
```

最後に、Congratulations!とでればOK。

## Nginxの設定

Ngixnの設定ファイルを開いて編集する、。

```
vi /etc/nginx/conf.d/default.conf
```

以下の箇所を編集・変更する。

### 編集前

```
server {
    listen 80;
.....
```

### 編集後

```
server {
    listen 443 ssl;
    ssl_certificate     /etc/letsencrypt/live/<SSL化したいドメイン>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/SSL化したいドメイン>/privkey.pem;
.....
```

Nginxを再起動して確認

```
systemctl restart nginx
```

Nginxを再起動してブラウザでSSL化になっていることが確認できればOK。
