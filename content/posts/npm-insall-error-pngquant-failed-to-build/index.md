---
title: "npm insallすると\"Error: pngquant failed to build, make sure that libpng-dev is installed\""
url: "/server/linux/npm-insall-error-pngquant-failed-to-build/"
date: 2020-03-18
categories: 
  - "linux"
  - "server"
---

DockerでLaravel開発環境を構築時にLaravel Mixを使おうと、`npm install`した時に以下エラー発生。

```
Error: pngquant failed to build, make sure that libpng-dev is installed
```

以下のパッケージをインストールすると解決。

```
yum install -y bash gcc make libpng-devel
```

Dockerfileに追記して再度buildすればよい。

または、`docker exec`かなんかでコンテナのシェルに接続して上記のコマンドを直接打ち込んでインストールするか。
