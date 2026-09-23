---
title: "MacにComposerをインストール手順"
url: "/programming/laravel/mac-composer/"
date: 2019-10-01
categories: 
  - "laravel"
---

MacOSにComposerをインストールする手順を説明する。

OpenSSLがないと、composerコマンドでライブラリダウンロードをするとエラーを起こすので手動でOpenSSLをインストールする。

OpenSSLをインストールするには、Homebrewが必要なのでインストールする。

## Homebrewのインストール

OpenSSLをインストールするために、Homebrewが必要。

以下コマンドで、Homebrewをインストール

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## OpenSSLをインストール

```
brew install openssl
```

## Composerをインストール

Download Composerにあるコマンドをそのまま実行する。

アップデートでコマンドの内容が変わることがあるので、直接公式ページに行きコマンドをコピペして実行する。

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

## パスを通す

```
sudo mv composer.phar /usr/local/bin/composer
```

## バージョンを確認

```
composer --version
```

バージョンが確認できたらOK！
