---
title: "【WordPressサイトヘルス解決】オプションのモジュールimagickがインストールされていないか、無効化されています。"
url: "/programming/wordpress-site-health-check-imagick/"
date: 2020-04-16
categories: 
  - "wordpress"
  - "programming"
---

WordPressサイトヘルスチェックの「1つ以上の推奨モジュールが存在しません」の中で、**「オプションのモジュール imagick がインストールされていないか、無効化されています。」**と出たのでimagickモジュールを入れることにした。

![](images/3b3fb65dc9bf9ccc4ed1b8674222432b-700x196.png)

環境は以下

- さくらのVPS
- CentOS 7
- Nginx 1.17
- PHP 7.4
- MariaDB

※**VPSでなくレンタルサーバーサービスならコントールパネルといった管理画面から簡単に入れられると思う。**

perlコマンドを使えるようにするため、php-pear php-develをインストール。

```
yum install --enablerepo=remi,remi-php74 php-pear php-devel
```

次に、imagickをインストール

```
pecl install imagick
```

/etc/php.d/の下に30-imagick.iniというiniファイルを作成し編集していく。

```
vi /etc/php.d/30-imagick.ini
```

そして、ファイルへ以下の記述をする。

```
; Enable imagick extension module
extension=imagick.so
```

最後に、php-fpmを再起動。

```
systemctl restart php-fpm
```

WordPressの管理画面に戻って、テスト通過されていることが確認できればOK。

![](images/e737e764cd2a71c9aa37113e5ad701ec.png)
