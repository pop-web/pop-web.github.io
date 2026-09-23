---
title: "【Gitコマンド】git push のオプション -u ってなに？"
url: "/programming/git/git-push-u/"
date: 2019-08-20
categories: 
  - "git"
---

Gitコマンドを学習していて、pushするときに以下のようなコマンドが良く出てくる

```
git push -u origin master
```

このオプションの「-u」なにかということです。

ちなみに、originはリモート名（別名）で、masterはブランチ名（メインブランチ）です。

オプション「-u」をつけると、次回から「git push」と実行するだけでorigin masterへpushできるようになります。

さらに、pullも同様に「git pull origin master」を「git pull」といった感じで省略できるようになります。
