---
title: "【Docker初心者向け】コンテナのステータスについて"
url: "/server/docker-status/"
date: 2020-03-02
categories: 
  - "docker"
  - "server"
---

Dockerコンテナの各ステータスについて説明する。

## createdステータス

コンテナが作成された状態。コンテナは起動していない。

`docker create`をつかってコンテナを作成する。

```
docker create --name ubuntu-test -it ubuntu /bin/sh
```

「-it」はコンテナ内でコマンドの入出力をするためのオプション。

**最後につけた「/bin/sh」はコンテナ起動時に最初に実行するコマンド。**

「/bin/sh」でシェルを実行することでその後のコマンドを打てるようにするため。これがないとコマンドが打てない。

### ステータスを確認

以下のコマンドでステータスを確認。

```
docker inspect ubuntu-test
```

「Status」が「created」ステータスになっていること確認できる。

`docker ps -a`でも確認できる。

## runningステータス

createdステータスのコンテナを`docker start`で「running」ステータスにすることができる。

```
docker start ubuntu-test
```

### ステータスを確認

`docker inspect ubuntu-test`で確認すると「Status」が「running」ステータスになっていること確認できる。

`docker ps`でも確認できる。

`docker run`コマンドは、**「create」と「start」を同時に行う。**

## pausedステータス

`docker pause`でコンテナを一時停止させる。

```
docker pause ubuntu-test
```

`docker inspect ubuntu-test`で確認すると「Status」が「paused」ステータスになっていること確認できる。

`docker ps -a`でも確認できる。

この一時停止は`docker unpause`で解除できる。

```
docker unpause ubuntu-test 
```

## restartingステータス

`docker restart`でコンテナを再起動できる。再起動中に「restarting」ステータスとなる。

## exitedステータス

「exited」ステータスはコンテナが終了した状態のこと。

コンテナは終了してもの「exited」ステータスとして残り続ける。

`docker run`でフォアグラウンドで実行させるオプションを付けずに実行すると、そのまま終了となり、「exited」ステータスとなる。

```
docker run --name ubuntu-test ubuntu
```

また、「running」ステータスのコンテナへは`docker stop`とコマンドを使うと終了とさせることができる。

```
docker stop ubuntu-test
```

## removingステータス

「removing」ステータスは、コンテナ削除中のステータス。

削除中のステータスなので削除されてしまった後の状態を見ることはあまりない。

コンテナの削除は`docker rm`でおこなう。停止中のコンテナにしか実行できない。実行中のコンテナを強制削除する場合は`-f`オプションを付与する。

## deadステータス

「dead」テータスは、正常に終了できなかったコンテナの状態。削除するか、強制削除をする。
