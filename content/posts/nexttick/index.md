---
title: "$nextTickでタイミングを遅延させて子コンポーネントにアクセスする"
url: "/programming/vue-js/nexttick/"
date: 2020-06-15
categories: 
  - "vue-js"
---

mountedやupdatedを使用してもでは子コンポーネントを全てを**マウントしたことは保証しない**ため、子コンポーネントへアクセスがうまくいかない時がある。

そこ解決するために、$nextTickを使用してDOMの更新サイクル後に処理実行させるようにする。

```
  this.$nextTick(function () {
    // DOMの更新サイクル後に実行する処理
  })
```
