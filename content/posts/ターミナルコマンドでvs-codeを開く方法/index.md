---
title: "ターミナルコマンドでVS Codeを開く方法"
url: "/programming/ターミナルコマンドでvs-codeを開く方法/"
date: 2020-09-17
categories: 
  - "programming"
---

コマンドからVS Codeを開くには`code`コマンドを使えば良い。

カレントディレクトリをVS Codeで開きたい場合

```
$ code .
```

ファイルやフォルダを開きたい場合

```
$ code somefolder/some.js
```

`code`コマンドが見つからない場合は、VS Codeのコマンドパレットを開く（⌘ + Shift + P）

shellで検索し、「Shell Command: install 'code' command in PATH」を選択すれば`code`コマンドが使えるようになる
