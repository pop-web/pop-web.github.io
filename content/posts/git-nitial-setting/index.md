---
title: "Gitの初期設定、やったほうがいい設定など"
url: "/programming/git/git-nitial-setting/"
date: 2019-08-17
categories: 
  - "git"
---

Gitをインストールしたら、まずやっておくべき初期設定です。

# ユーザ情報の登録

```
git config --global user.name "user name"
```

```
git config --global user.email "mail@example.jp"
```

設定内容の確認

```
git config --global -l
```

とえあえず、はじめにこれをやっておきましょう。
