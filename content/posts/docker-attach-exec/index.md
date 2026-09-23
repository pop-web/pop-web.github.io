---
title: "【Docker初心者向け】シェルの接続（attach,exec）"
url: "/server/docker-attach-exec/"
date: 2020-03-04
categories: 
  - "docker"
  - "server"
---

Dockerのコンテナにシェル接続する方法は2つある。

`docker attach`と`docker exec`。

# docker attach

```
docker attach {コンテナID or コンテナ名}
```

exitコマンドで接続を抜けると、コンテナが停止してしまう。

コンテナを停止させずに接続から抜けたい場合は、「Ctrl + P」「Ctrl + Q」とする。

# docker exec

```
docker exec -it {コンテナID or コンテナ名} /bin/bash
```

exexは、コンテナ内の任意のコマンドを実行できるコマンド。

exitコマンドで接続を抜けても、コンテナは停止しない。
