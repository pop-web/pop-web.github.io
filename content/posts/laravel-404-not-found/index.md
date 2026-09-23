---
title: "Laravelでルート以外のページが404（Not Found）になっちゃった時の対処"
url: "/programming/laravel/laravel-404-not-found/"
date: 2019-06-07
categories: 
  - "laravel"
---

Laravelで、トップページは表示できるけど他のページに飛ぶとなぜか404になっちゃう時に確認すべきことのメモ。

## httpd.confを確認する

ドキュメントルートのディレクティブ内の「AllowOverride」が「None」になってたら、「All」に変更しておく。

```
<Directory "/var/www/html">
AllowOverride All
</Directory>
```

AllowOverride Noneになっていると、 .htaccessが無効になるので、Allにして有効に。
