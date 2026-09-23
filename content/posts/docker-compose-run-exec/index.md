---
title: "docker-compose runとdocker-compose execの違い"
url: "/server/docker-compose-run-exec/"
date: 2020-03-23
categories: 
  - "docker"
  - "server"
---

docker-composeコマンドのrunとexecの大まかな違いについて説明。

## run

コンテナを起動し、コマンドを実行できる。

起動していないコンテナに実行することができる。

### 例

```
docker-compose run app ash
```

## exec

起動中のコンテナに対して、コマンドを実行できる。

起動していないコンテナには実行できないが、runよりも高速に動作する。

### 例

```
docker-compose exec app ash
```
