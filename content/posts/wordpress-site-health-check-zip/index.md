---
title: "【WordPressサイトヘルス解決】オプションのモジュールzipがインストールされていないか、無効化されています。"
url: "/programming/wordpress-site-health-check-zip/"
date: 2020-04-17
categories: 
  - "wordpress"
  - "programming"
---

WordPressサイトヘルスチェックの「1つ以上の推奨モジュールが存在しません」の中で、**「オプションのモジュール zip がインストールされていないか、無効化されています。」**と出たのでzipモジュールを入れることにした。

![](images/8700359200f0cb1b1fe038e36a1bc19b-700x196.png)

環境は以下

- さくらのVPS
- CentOS 7
- Nginx 1.17
- PHP 7.4
- MariaDB

※**VPSでなくレンタルサーバーサービスならコントールパネルといった管理画面から簡単に入れられると思う。**

zipモジュールは、`php-pecl-zip`を入れればよい。

```
yum --enablerepo=remi,remi-php74 install php-pecl-zip
```

最後に、php-fpmを再起動。

```
systemctl restart php-fpm
```

WordPressの管理画面に戻って、テスト通過されていることが確認できればOK。

![](images/e737e764cd2a71c9aa37113e5ad701ec.png)
