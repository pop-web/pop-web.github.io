---
title: "Laravelのやっておいた方がいい初期設定"
url: "/programming/laravel/laravel-initial-setting/"
date: 2019-06-28
categories: 
  - "laravel"
---

Laravelをインストール後の、やってほい方がいい初期設定

- **タイムゾーンを日本時間に**
- **エラーメッセージを日本語化**

## **タイムゾーンを日本時間に**

「/config/app.php」を編集します。

```
'timezone' => 'UTC',
↓
'timezone' => 'Asia/Tokyo',
```

## **エラーメッセージを日本語化**

「/config/app.php」を編集します。

```
'locale' => 'en',
↓
'locale' => 'ja',
```

### resources/lang/ja/validation.php作成

エラーメッセージの日本語ファイルはデフォルトで設置されていないので、手動で設定していきます。

[https://gist.github.com/syokunin/b37725686b5baf09255b](https://gist.github.com/syokunin/b37725686b5baf09255b)

上記、URLから「validation.php」をダウンロードしてきます。

「resources/lang/ja/validation.php」という感じで設置します。

## キャッシュをクリア

```
$ php artisan config:clear
または
$ php artisan config:cache
```

/config/\*.phpを編集した後に反映されない場合は、「php artisan config:cache」をしていてキャッシュを読み込んでいるのが原因。

なので、上の2つのどちらかのコマンドを実行する必要がある。

使い分けとしては、開発環境では`php artisan config:clear`しておいて、本藩環境では`php artisan config:cache`とするこが推奨されている。
