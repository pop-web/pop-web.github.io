---
title: "【Docker初心者向け】commitコマンドでイメージを作成"
url: "/server/docker-commit/"
date: 2020-03-05
categories: 
  - "docker"
  - "server"
---

`docker commit`でコンテナ内で作業した内容を、イメージとして保存することができる。

コマンドは以下のよう。

```
docker commit {コンテナID or コンテナ名} {新しいイメージ名}:{タグ名}
```

実際に適当なコンテナを起動してシェルに接続して変更をしてみる。

```
docker run --name nginx-test -it -d --rm nginx /bin/bash
```

シェルに接続する。

```
docker exec -it nginx-test /bin/bash
```

tmpディレクトリに任意サイズのファイルを作成する。

ここでは、1MBファイルを作成

```
cd tmp/
dd if=/dev/zero of=dummy10M bs=1M count=1
```

exitで接続を抜ける

```
exit
```

`docker commit`で新しいイメージを作成する。

```
docker commit nginx-test nginx-test:tag1
```

`docker images`で新しくイメージができていることを確認。

新しいイメージでコンテナを起動してシェルに接続、1MBファイルがあることを確認する。

```
docker run -it nginx-test:tag1 /bin/bash
```

```
cd tmp/
ls -lh
total 1.0M
-rw-r--r-- 1 root root 1.0M Mar  1 08:54 dummy10M
```

また、イメージに過去にどんなレイヤーが追加されたの確認するには`docker history`を使う。

```
docker history {イメージID or イメージ名}
```

ちなみに、`docker history`を確認しても、詳しいコンテナ内のコマンド操作は記載されることはない。

なので、基本的にはDockerfileでコマンド操作を記述しイメージを変更・作成することが推奨されている。
