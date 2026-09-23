---
title: "Gitリモートリポジトリをすべてのブランチごと別のリモートリポジトリへの移行方法"
url: "/programming/git/remote-repository-migration/"
date: 2020-04-10
categories: 
  - "git"
  - "programming"
---

既存のGitリモートリポジトリを別のGitリモートリポジトリ移行する方法を説明する。

移行する時、すべてのブランチやコミット履歴も一緒に移行する。

まず、移行先のGitリポジトリを作成しておく。

そして、以下のようにGitコマンドの`--mirror`オプションを使って移行をする。

```
git clone --mirror <既存のリモートリポジトリURL> <任意のディレクトリ名>
cd <任意のディレクトリ名>
git push --mirror <移行先のリモートリポジトリURL>
```

これで、移行先のリモートリポジトリをのぞいて、想定通りのの移行できていればOK。
