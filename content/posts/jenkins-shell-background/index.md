---
title: "Jenkinsでシェルをバックグラウンドで実行し続ける方法"
url: "/server/jenkins/jenkins-shell-background/"
date: 2019-12-16
categories: 
  - "jenkins"
  - "server"
---

Jenkinsビルド実行時も、モックAPIライブラリのjson-severを起動し続けようと思い、&を最後につけたり、nohupコマンドを使ったりしたが、Jenkinsのジョブが終了するとプロセスがどうしても終了してしまう。

が、**BUILD\_ID=dontKillMe**を前につけるとうまくいった。

以下の感じ。

```
BUILD_ID=dontKillMe nohup json-server db.json -p 3001 -H 192.168.33.10 < /dev/null &
```

BUILD＿IDの変数の値をなにかしらで上書きしないと、ProcessTreeKillerという仕組みで、ジョブの中で起動したプロセスは、ジョブの終了とともに殺されてしまいます。
