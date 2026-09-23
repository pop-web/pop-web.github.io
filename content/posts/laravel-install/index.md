---
title: "Laravelのインストール方法、方法は2パターン"
url: "/programming/laravel/laravel-install/"
date: 2019-05-24
categories: 
  - "laravel"
---

事前にComposerをインストールしておいてください。

Laravelをインストール方法は、2パターン。

1. **Laravelインストーラを使う方法**
2. **Laravelインストーラを使わない方法**

## **Laravelインストーラを使う方法**

composerコマンドで、Laravelインストーラをダウンロードします。

コマンドプロンプト、または好きなターミナルを立ち上げて、以下を入力。

```
composer global require "laravel/installer" --prefer-dist

※バージョン指定の場合
composer global require "laravel/installer=~1.1" --prefer-dist
```

インストールに少し時間がかかります…

### 環境変数PATHを設定する

インストールが完了したら、環境変数PATHを設定して、Laravelコマンドを使用できるようにします。

Windowsの環境変数PATHの設定は、Windowsの検索窓の「環境変数」と入力すれば、「システム環境変数の編集」がヒットするので選択し、さらに現れたウィンドウの中の「環境変数」をクリックします。

![](images/2019-05-24_17h53_58-700x565.png)

表示された環境変数のウィンドウのリストから「Path」を選択して「編集」ボタンをクリックして編集していきます。

そして、以下のパスを追加します。

```
%USERPROFILE%\AppData\Roaming\Composer\vendor\bin
```

これでLaravelコマンドつかえるようになりましたので、以下のコマンドで新規のLaravelプロジェクトを作成します。

```
laravel new プロジェクト名
```

これでインストールが完了したら、フォルダの中にLaravelフレームワークのファイル一式が入っている状態になります。

## Laravelインストーラを使わない方法

インストーラを使わない方法では、composerで以下のように入力してプロジェクトを作成します。

```
composer create-project --prefer-dist laravel/laravel プロジェクト名

※バージョン指定の場合
composer create-project --prefer-dist "laravel/laravel=5.7.*" プロジェクト名
```

ちなみに、オプションの「--prefer-dist」はZIPでダウンロードするって意味。

指定なしだと、 git cloneでダウンロードで、ZIPダウンロードの方が高速らしい。

## どっちがいいのか

色々なサイトで調べてみても、インストーラを使わない方法が多いみたい。

なので、私はインストーラを使わない方法でやっています。
