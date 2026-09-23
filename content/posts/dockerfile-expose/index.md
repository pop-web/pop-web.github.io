---
title: "DockerfileのEXPOSE命令は実際にポートをあけない"
url: "/server/dockerfile-expose/"
date: 2020-03-25
categories: 
  - "docker"
  - "server"
---

```
EXPOSE 5000
```

DockerfileのEXPOSEに外部に公開するポートを記述するが、これを記述することで**実際にポートをあけて公開するわけではない。**

開発者がDockerfileを見たときに「コンテナ起動時にどのポートを使用するのか」を確認するためものであり、公開されると想定されるポートを記述しているだけである。

実際にコンテナ起動時にポートを外部へ公開する場合は、docker run に-pオプションを使う。

```
docker run -p 5000:5000 sample-image
```

また、-Pオプションをつけた場合は全ポート公開となる。

```
docker run -P sample-image
```
