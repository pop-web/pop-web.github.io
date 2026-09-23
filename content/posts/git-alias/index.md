---
title: "よく使うGitコマンドにエイリアス設定方法"
url: "/programming/git/git-alias/"
date: 2019-08-28
categories: 
  - "git"
---

よく使うGitコマンドにエイリアス（別名）設定をする方法を説明します。

例えば以下のようにコマンドを短縮したい場合

```
git status →　git st
```

以下の、コマンドでエイリアスを設定します。

```
git config --global alias.st status
```

そうすると、`git st`と入力すると`git status`と同じ動作になります。

その他のコマンドについても、以下のような感じです。

```
git config --global alias.ci commit
git config --global alias.add ad
git config --global alias.br branch
git config --global alias.co checkout
```

こんな感じで、コマンドの入力を短縮して楽にしていきましょう。
