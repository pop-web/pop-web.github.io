---
title: "プログラムがどのポート番号を使っているか確認のLinuxコマンド"
url: "/server/linux/port/"
date: 2020-03-21
categories: 
  - "linux"
  - "server"
---

Docker等を使っていて以下のようなエラーが出る場合がある。

```
ERROR: for web Cannot start service web: Ports are not available: listen tcp 0.0.0.0:80: bind: address already in use
```

そんな場合、以下のコマンドでLISTENしているポートを確認できる。

```
lsof -i -n -P | grep "LISTEN"
```
