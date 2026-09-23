---
title: "【Vue】computedとmethodsの違いについて"
url: "/programming/vue-js/【vue】computedとmethodsの違いについて/"
date: 2020-04-06
categories: 
  - "vue-js"
  - "programming"
---

## computedがmethodsとは異なる点

computedがmethodsとは異なる点は、**computedの方は依存関係に基づいてキャッシュされるという性質を持っているということ。**

- computedは、依存関係にある値が変更されると処理が走るが、変更されてないとキャッシュを返す。
- methodsは、キャッシュを返さないのでテンプレートが少しでも再描画されるたびに処理が走る。

処理コストを減らすためにも、**キャッシュを使いたくない場合を除いてはcomputedをなるべく使用することが推奨されている。**

## コードを書いてキャッシュされるかの確認

実際にcomputedで定義した処理がキャッシュされるのか確認する。

### HTML

```
<div id="app">
    <p>{{ count }}</p>
    <button type="button" @click="clickCount">+1</button>
    <p>methods: {{ dateMethods() }}</p>
    <p>computed: {{ dateComputed }}</p>
</div>
```

+1するcountするボタンは、ただ再描画するためだけに設置したもの。

### JavaScript

```
new Vue({
    el: '#app',
    data: {
        count: 0
    },
    methods: {
        clickCount(){
            this.count += 1
        },
        dateMethods() {
            return Date.now()
        }
    },
    computed: {
        dateComputed() {
            return Date.now()
        }
    }
});
```

これでcountボタンを何回かクリックすると「methods:」の値は変わるが、「computed:」の値は変わらないことがわかる。

Date.now()は依存関係のあるデータの変更がないため、computedでは常にキャッシュが返されるため値が変わらない。

対してmethodsは、画面のどこが再描画されるたびに処理が走るため値が変化する。
