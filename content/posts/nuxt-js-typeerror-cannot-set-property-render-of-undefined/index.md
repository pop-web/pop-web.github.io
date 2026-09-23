---
title: "Nuxt.jsで「TypeError: Cannot set property 'render' of undefined」のコソールエラー対処法"
url: "/programming/vue-js/nuxt-js-typeerror-cannot-set-property-render-of-undefined/"
date: 2020-10-28
categories: 
  - "vue-js"
---

## エラー内容

Nuxt.jsで開発中、以下のコンソールエラーが出て画面が表示されなくなった。

```
TypeError: Cannot set property 'render' of undefined
```

## 原因と対処法

**<script>タグの閉じタグがなかったり、空の<script>タグが存在**しているとエラーとなるため、ちゃんと閉じタグをしてあげるか、空タグを削除してあげるとよい。
