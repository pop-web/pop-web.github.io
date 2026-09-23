---
title: "【コンソールエラーの原因】front.js:8 Uncaught ReferenceError: browser is not defined at HTMLDocument.<anonymous>"
url: "/programming/javascript/browser-is-not-defined-at-htmldocument/"
date: 2020-05-29
categories: 
  - "javascript"
  - "programming"
---

Google Chromeのデバッグコンソールに以下のようなエラーが出た。

```
front.js:8 Uncaught ReferenceError: browser is not defined at HTMLDocument.<anonymous> (front.js:8)
```

色々調べた結果、今回の場合は**Google Chromeの拡張機能が原因**だった。

シークレットウィンドウで再度表示してみると、エラーはでない。

どの拡張機能が原因でエラーが出ているかは各自で調べて頂きたい。

結論→あまり気にすることではなかった。
