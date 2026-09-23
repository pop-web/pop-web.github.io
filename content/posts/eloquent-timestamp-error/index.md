---
title: "Eloquentモデルでtimestampがないテーブルに保存・更新でエラーが出たときの対処"
url: "/programming/laravel/eloquent-timestamp-error/"
date: 2019-06-19
categories: 
  - "laravel"
---

日付情報が必要がなくタイムスタンプを作ってないテーブルへデータを更新・保存しようとすると、以下のようなエラーが出た。

```
Column not found: 1054 Unknown column ‘updated_at’ in ‘field list’
```

「updated\_atカラムがない」と言っている。

## 対処法

タイムスタンプを無効にするため、Modelクラスに以下のように記述をする。

```
public $timestamps = false;
```
