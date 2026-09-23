---
title: "Firebaseへデプロイしたサイトを削除する方法"
url: "/server/firebase-deploy-remove/"
date: 2020-05-04
categories: 
  - "firebase"
  - "server"
---

Firebase CLIを使ってHostingへデプロイしたサイトを削除したい場合は、以下のコマンドを入力すれば良い。

```
firebase hosting:disable
```

該当ドメインでアクセスし「Site Not Found」になっていることをブラウザで確認する。

Hostingのダッシュボードへ移動して、リリース履歴をみると縦3つのドットのメニューが出現している。

そこの「削除」を実行することでファイルを完全削除できる。

![](images/757254d7c22d4f98bb71a5d9e62a29fb-700x66.png)
