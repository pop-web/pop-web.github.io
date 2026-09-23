---
title: "window.top === window.selfは何をしているのか？"
url: "/programming/javascript/window-top-window-self/"
date: 2022-01-04
categories: 
  - "javascript"
---

プロジェクトで「if (window.top === window.self) {…}」という条件式を初めて見たので、その意味を調べてみました。

この条件式は、開いているページがiframeで読み込んだページを表示しているかどうかを判定するためのものです。

`window.top`は、最上位のコンテキスト（ウィンドウ）を返します。iframe要素を複数埋め込んでいる場合でも、一番上の親ウィンドウを取得することができます。

一方、`window.self`は現在のウィンドウ自身を返します。

したがって、iframeがないページでは`window.self`と`window.top`は等価になります。これを利用して、ページがiframeを含むかどうかを判定することができます。
