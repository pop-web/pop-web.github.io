---
title: "【Vue】コンポーネントのグローバル登録とローカル登録"
url: "/programming/vue-js/vue-component-global-local/"
date: 2019-08-24
categories: 
  - "vue-js"
---

Vue.jsでコンポーネントを定義する際に、グローバル登録とローカル登録の２パターンある。

それぞれ説明する。

## グローバル登録

グローバル登録するには、「Vue.componet」を使う。

以下のように、Vueイスンスタスの前に定義する。

- **JavaScript**

```
// コンポーネントのグローバル登録
Vue.component('test-component',{
  template:'<p>Hello,Vue.js!</p>'
})

new Vue({
  el: '#app'
})
```

「Vue.componet」では、第一引数にはコンポーネント名、第二引数にはテンプレートの内容を書く。

**DOMテンプレートでコンポーネントを使う場合、コンポーネント名はケバブケースで書くのがルール。**

**これはDOMテンプレートの場合、大文字、小文字をブラウザは区別しないためパスカルケースとった大文字を含むものはすべて小文字として解釈されてしまうため。**

※DOMテンプレートとは、`.html`ファイルに記載されるhtmlテンプレートのこと。逆に`.vue`ファイルに記載されるhtmlテンプレートのことを文字列テンプレートといい、こちらはVue側で大文字、小文字を解釈してくれるので区別がされる。

HTML側でコンポーネントを出力する場合は、以下のようにする。

- **HTML**

```
<div id="app">
  <test-component></test-component>
  <test-component></test-component>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.min.js"></script>
```

これで、「Hello,Vue.js!」が２つ表示できればOK。

## ローカル登録

次にローカル登録を定義する。

こちらも、Vueインスタンスの前に定義するが、**コンポーネントを変数に格納しておく。**

コンポーネントを格納した変数をVueインスタンスのcomponentsオプションで設定する。

これで**設定したVueインスタンスのみ**で利用できるものとなる。

- **JavaScript**

```
const testComponent = {
    template:'<p>Hello,Vue.js!</p>'
}

new Vue({
  el: '#app',
    components: {
    'test-component': testComponent
  }
})
```

- **HTML**

```
<div id="app">
  <test-component></test-component>
  <test-component></test-component>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.min.js"></script>
```

また、注意点としてtemplateで定義するHTML要素は単一でなければならない。

以下、例。

- **NG例：**

```
template:'<p>Hello,Vue.js!</p><p>Hello,Vue.js!</p>'
```

- **OK例：**

```
template:'<div><p>Hello,Vue.js!</p><p>Hello,Vue.js!</p></div>'
```
