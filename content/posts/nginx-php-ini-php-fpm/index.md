---
title: "Nginxでphp.iniの更新やモジュールを追加した場合、systemctl restart php-fpmで反映"
url: "/server/linux/nginx-php-ini-php-fpm/"
date: 2020-04-20
categories: 
  - "linux"
  - "server"
---

Nginxでphp-fpmサーバーを使っている場合、php.iniの編集やPHPのモジュールを追加の反映は以下のコマンドでphp-fpmを再起動する。

```
systemctl restart php-fpm
```

**Apacheとは違ってWebサーバー（Nginx）をを再起動しても設定は反映されないので注意。**
