---
title: "【Nuxt.js コンポーネント間の循環参照】Unknown custom elementとなる原因と解決方法"
url: "/programming/vue-js/nuxt-js-unknown-custom-element/"
date: 2020-06-03
categories: 
  - "vue-js"
  - "programming"
---

Nuxt.jsで開発中に以下のエラーが出てハマった。

```
[Vue warn]: Unknown custom element: <コンポーネント名> - did you register the component correctly? For recursive components, make sure to provide the "name" option.
```

エラーメッセージにあるように、nameオプションで指定してもダメだった。

今回の場合、原因は**コンポーネント間の循環参照の記述方法だった。**

Vue.jsのドキュメントの[コンポーネント間の循環参照](https://jp.vuejs.org/v2/guide/components-edge-cases.html#%E3%82%B3%E3%83%B3%E3%83%9D%E3%83%BC%E3%83%8D%E3%83%B3%E3%83%88%E9%96%93%E3%81%AE%E5%BE%AA%E7%92%B0%E5%8F%82%E7%85%A7)に書いてあるように、記述方法には2通りある。

`beforeCreate`で`require`を使ってコンポーネント登録するか、`components`フィールド内で `import`を使ってコンポーネント登録するかだ。

Nuxt.jsの場合では、`require`の方でやると前述のエラーが出るが、`import`の方でやるとエラーは出ないようだ。

#### Nuxt.jsでエラーが出る記述

```
<script>
export default {
  // Nuxt.jsではエラー
  beforeCreate() {
    this.$options.components.Component = require("~/components/modal/Component.vue").default;
  }
};
</script>
```

#### Nuxt.jsでエラーが出ない記述

```
<script>
export default {
  // Nuxt.jsでもOK
  Component: () => import("~/components/modal/Component.vue")
};
</script>
```
