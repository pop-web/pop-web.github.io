---
title: "既存のディレクトリにローカルリポジトリ作成しをGitHubに登録する"
url: "/programming/git/git-regist/"
date: 2019-05-28
categories: 
  - "git"
---

※gitは事前にインストールしているものする。

git管理していない作業ディレクトリや、すでにローカルリポジトリがある場合にGitHubへ登録する手順を書く。

## ローカルリポジトリを作る

現在の作業ディレクトリで、バージョン管理していない場合、ますローカルのリポジトリを作ってディレクトリ内のファイルをコミットしておく。

作業ディレクトリに移動して、git initする。

```
cd 作業ディレクトリ
git init
```

これで、.gitディレクトリができる。

次に、ファイルたちをインデックスさせてコミットする。

```
git add .
git commit -m "first commit"
```

## GitHubでリポジトリを作る

GitHubへ行き、「New Repository」ボタンを押して、リポジトリを作っておく。

できたリポジトリURLをコピーしておく。

![](images/2019-05-29_01h45_32-700x41.png)

## ローカルとリモートを関連付ける

以下のコマンドでローカルとリモートをつなげる。

```
git remote add origin https://github.com/username/sample.git
```

関連付けられているか確認

```
git remote -v
```

## リモートにpushする

```
git push origin master
```
