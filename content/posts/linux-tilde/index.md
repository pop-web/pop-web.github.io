---
title: "Linuxコマンドの「~」チルダの意味は"
url: "/server/linux/linux-tilde/"
date: 2019-05-23
categories: 
  - "linux"
---

Linuxコマンドで 「～」チルダの意味は、**ホームディレクトリを意味**します。

試しに使ってみるとわかりやすい。

agreeというユーザーでログインしており、自身のagreeディレクトリのpublic\_htmlディレクトリに移動したい場合は以下のようになる。

```
$ cd ~/public_html
$ pwd // 現在のディレクトリを表示する「pwd」コマンド
/c/Users/agree/public_html
```

単にホームディレクトリに移動するだけなら、「cd」と入力するだけでよい。

```
$ cd
$ pwd
/c/Users/agree
```

ちなみに、「/」はルートディレクトリを意味するので、「cd /」だとルートディレクトリに移動する。

```
$ cd /
$ pwd
/
```
