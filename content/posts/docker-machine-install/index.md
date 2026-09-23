---
title: "【Docker Machine】docker-machine command not foundとなるので手動でインストール"
url: "/server/docker-machine-install/"
date: 2020-03-06
categories: 
  - "docker"
  - "server"
---

Docker for Macをインストールしたが、`docker-machine`とコマンド入力してもそんなもんは見つからないと言われ、Docker Machineが使えなかった。

調べてみると、以前はDockerインストールと同時に一緒インストールされていたが、今はされない模様。

なので、以下のURL先を参考に手動でインストールする。

[https://github.com/docker/machine/releases/](https://github.com/docker/machine/releases/)

URL先を参考に以下のコマンドを入力。

```
curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/usr/local/bin/docker-machine && \
  chmod +x /usr/local/bin/docker-machine
```

これで、Docker Machineが使えるようになる。
