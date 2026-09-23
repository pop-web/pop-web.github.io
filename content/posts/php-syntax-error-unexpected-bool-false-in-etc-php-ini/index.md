---
title: "【エラー解決】PHP:  syntax error, unexpected BOOL_FALSE in /etc/php.ini"
url: "/programming/php/php-syntax-error-unexpected-bool-false-in-etc-php-ini/"
date: 2020-06-14
categories: 
  - "php"
---

さくらのVPSにPHP7.4をインストールして使用しているが、PHPを実行するたびに以下のようなエラーを吐いていた。

```
php -v
PHP:  syntax error, unexpected BOOL_FALSE in /etc/php.ini on line 1008
```

実際にphp.iniファイルの1008行目のぞいてみると、コメントアウトされている箇所で、エラーが出るような設定はなかった。

```
    106 ; error_reporting
    107 ;   Default Value: E_ALL & ~E_NOTICE & ~E_STRICT & ~E_DEPRECATED
    108 ;   Development Value: E_ALL
    109 ;   Production Value: E_ALL & ~E_DEPRECATED & ~E_STRICT
```

調べてみると、エラーで示されている行番号でエラーが出ているわけではなかった。

それより以前の行でシンタックスエラーでているらしい。

今回の場合、923行目のタイムゾーンのダブルクォートが抜けている箇所がエラーだった。

```
    920 [Date]
    921 ; Defines the default timezone used by the date functions
    922 ; http://php.net/date.timezone
    923 date.timezone = Asia/Tokyo"
```

ダブルクォートを付け加えて修正保存。

```
    923 date.timezone = "Asia/Tokyo"
```

最後に修正を有効にするため、再起動させておく。

```
systemctl restart php-fpm
systemctl restart nginx
```
