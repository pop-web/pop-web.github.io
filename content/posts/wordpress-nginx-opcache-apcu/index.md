---
title: "WordPress高速化、Nginx環境にOPcacheとAPCuをインストール"
url: "/programming/wordpress/wordpress-nginx-opcache-apcu/"
date: 2020-06-12
categories: 
  - "wordpress"
---

当サイトは、さくらのVPS上にWordPressをのせて構築していているが、体感的に表示速度が遅かった。

WordPress高速化するのため、PHPのキャッシュ機能パッケージであるOPcacheとAPCuをインストールする。

因みに、サーバの設定とかせずにVPSやクラウド上のWordPressサイトを高速化したい場合は、[「KUSANAGI」](https://kusanagi.tokyo/)というCMS高速化に特化した実行環境がある。

## インストール前後のPage Speed Insightsスコア比較

OPcacheとAPCuをインストールする前と後のPage Speed Insightsスコア。

### OPcacheとAPCuインストール前

![](images/4a08ab877b4af4e79bcbffdac2874b86-700x239.png)

### OPcacheとAPCuインストール後

![](images/57e717c375ada4b63b1aa14a32f32a45-700x240.png)

スコアが劇的にアップしたわけではないが、体感的にもインストール後の方が表示速度は早くなったと感じられた。

## OPcacheとAPCuをインストール

OPcacheとAPCuをインストールしていく。

環境は、さくらのVPS、CentOS 7、Nginx 1.17、PHP7.4。

以下のコマンドでインストール。

```
yum -y install --enablerepo=remi,remi-php74 php-opcache php-pecl-apcu
```

インストールが完了したら`php -v`で確認。

```
php -v
PHP 7.4.7 (cli) (built: Jun  9 2020 10:57:17) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.7, Copyright (c), by Zend Technologies
```

上記のように、with Zend OPcache v7.4.7となっていればOK。

## php-fpmとnginxの再起動して有効化

php-fpmとnginxを再起動するため、以下のコマンドを入力。

```
systemctl restart php-fpm
systemctl restart nginx
```
