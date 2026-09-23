---
title: "【初心者用】Reduxを分かりやすく解説"
draft: true
---

Reduxはアプリケーション状態管理のJavaScriptライブラリで、React.jsでよく使用される。

まずは、Reduxの主な機能である「State」「Action」「Reducer」「Store」について説明する。

## State

Stateは、アプリケーションで保持するデータのこと。

## Action

Actionは、アプリケーション内でどのような処理をするのかを示すオブジェクトデータ。

Actionを返す関数のことを、ActionCreaterと呼ぶ。

## Reducer

Reducerは、Action実行時にActionで記述されているtypeに応じて、状態（State）をどのように変更するかを定義し、その結果を返す関数。

## Store

アプリケーション内のすべてのStateを保持するオブジェクトデータ。
