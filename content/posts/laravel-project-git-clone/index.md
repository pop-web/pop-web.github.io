---
title: "共有するLaravelプロジェクトをgit cloneしてきた後の設定手順"
url: "/programming/laravel/laravel-project-git-clone/"
date: 2019-05-28
categories: 
  - "laravel"
---

Laravelのチーム開発で、途中から参加する場合、リモートリポジトリからプロジェクトデータをgit cloneすることはよくあります。

Laravelは、元から「.gitignore」ファイルが存在している為、gitリポジトリからデータを落としてきても不完全なわけです。

よくあるのが、「vendor」ディレクトリや「.env」ファイルがなくて動かないよってやつです。

## git cloneでプロジェクトを落としてくる

まず、はじめに開発するLaravelプロジェクトをgit cloneします。

```
git clone [gitリモートリポジトリのURL]
```

## composerでライブラリを入れる

ライブラリ群をインストールします。

「composer.lock」ファイルを読み込みに行かせるために、以下のコマンドを入力。

```
composer install
```

これで、「vendor」ディレクトリができて中に、ライブラリ群が入っている状態になります。

## .envファイルの作成

設定ファイルである「.env」ファイルを作成します。

すでに、ある「.env.example」ファイルをリネームして作成します。

```
cp .env.example .env
```

アプリケーションキーの設定

「.env」ファイルの中の、APP\_KEY値がデフォルトままなので、以下のコマンドで値をセットします。

```
php artisan key:generate
```

この設定で、ユーザーセッションや他の暗号化済みデーターは安全に守られるようになります。

これで、Laravelプロジェクトをブラウザで確認してちゃんと表示できているか確認します。

別途、ドキュメントルートの設定が必要な場合がありますので、その場合も各自設定しておきましょう。
