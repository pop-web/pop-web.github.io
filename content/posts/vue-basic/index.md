---
title: "【Vue】初心者向け基本機能まとめ①"
url: "/programming/vue-js/vue-basic/"
date: 2019-08-27
categories: 
  - "vue-js"
---

Vue.jsの基本機能をまとめて説明。

# dataオプション

dataオプションは、その名の通りデータを扱うオプション。

定義したデータは、テンプレート側（HTML）でマスタッシュ構文で展開することができる。

オブジェクトや配列も定義できる。

```
<div id="app">
  <p>{{message}}</p>
  <p>{{foods.apple}}</p>
  <p>{{drink[1]}}</p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello,World',
      foods: {
        tomato: 100,
        apple: 300,
        banana: 250
      },
      drink: ['Water', 'Coffee', 'Milk']
    }
  })
</script>
```

# v-bind：属性データバインディング

HTMLタグの属性バインディングには、属性の前に「v-bind:」を付与する。

```
<div id="app">
  <input type="text" v-bind:value="message">
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello,World',
  }
})
</script>
```

# v-if：条件分岐

条件分岐をするときは、v-ifを使用する。

以下ように、displayプロパティをfalseにすると非表示に、trueにすると表示になる。

v-ifで表示・非表示する場合、CSS操作ではなく、DOMレベルで展開と削除がされる。

```
<div id="app">
  <p v-if="display">
    Hello,World
  </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    display: false,
  }
})
</script>
```

# v-show：条件分岐

v-ifとは違い、表示・非表示をCSSのdisplayプロパティを操作して実現する。

```
<div id="app">
  <p v-show="display">
    Hello,World
  </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    display: false,
  }
})
</script>
```

# v-for：繰り返し処理

v-forは、繰り返し処理ができる。

dataオプションに定義したオブジェクトや配列を表示できる。

```
<div id="app">
  <ul>
    <li v-for="(drink,index) in drinks">
      {{ index }}：{{ drink }}
    </li>
  </ul>
  <ol>
    <li v-for="(price,food) in foods">
      {{ food }}:{{price}}
    </li>
  </ol>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    drinks:['Water','Coffee','Milk'],
    foods: {
      tomato: 100,
      apple: 300,
      banana: 250
    }
  }
})
</script>
```

# v-on：イベント

v-onはイベント処理を設定できる。

以下は、ボタンを押すと表示される処理。

```
<div id="app">
  <button v-on:click="action">
    表示する！
  </button>
  <p v-if="display">
    表示されました！
  </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    display: false
  },
  methods:{
      action:function(){
        this.display = !this.display
    }
  }
})
</script>
```

# v-model：双方向データバインディング

v-medelでは、dataオプションで定義した値とテンプレートで展開した値が、双方向に値が変更される。

```
<div id="app">
  <input type="text" v-model="message">
  <p>
    {{message}}
  </p>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
new Vue({
  el: '#app',
  data: {
    message: 'Hello,World'
  }
})
</script>
```

# まとめ

とりあえず、Vue.jsの基本機能は以上。
