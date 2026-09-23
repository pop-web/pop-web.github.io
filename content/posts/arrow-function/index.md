---
title: "JavaScript（ES2015）アロー関数の省略の書き方とthisの束縛について"
url: "/programming/javascript/arrow-function/"
date: 2020-05-09
categories: 
  - "javascript"
  - "programming"
---

ES2015（ES6）から追加されたアロー関数だが、省略した書き方が色々あるのでまとめてみる。

## アロー関数の基本形

アロー関数は基本的には、無名関数（匿名関数）の省略記法で、その名の通り`=>`を使って関数リテラルを定義する。

まずは基本形から。

### 普通の無名関数の記述

```
function(params) {
  // 処理の内容...
}
```

### アロー関数を使った無名関数の記述

```
(params) => {
  // 処理の内容...
}
```

## アロー関数の省略系

### 引数が1つの時 引数の`()` を省略できる

```
params => {
  // 処理の内容...
}
```

引数がない時は、`()` を書く必要がある。

```
() => {
  // 処理の内容...
}
```

### 処理の内容が1つだけの場合、`{}` を省略できる

```
params => returen value
```

### 値を返すだけの時、`return` を省略できる

```
params => value
```

## オブジェクトを返す時は`()` で囲む必要がある

オブジェクトリテラルを返す時は、`{}` が関数ブロックと解釈されてしまうため `()` で囲む必要がある。

```
const f = param => ({foo: "bar"})
console.log(f()) // { foo: "bar" }
```

`()` で囲むルールが逆に見づらい場合は、素直に `return` でオブジェクトリテラルを返せばよい。

```
const f = param => { return { foo: "bar" } }
console.log(f()) // { foo: "bar" }
```

## アロー関数におけるthisの束縛について

アロー関数内のthisと従来の無名関数の中のthisは違うところを指していることに注意する。

以下のように、コードがあるとする。

```
function Count() {
  this.count = 0 // 注①
  setInterval(function() {
    this.count++ // 注②
  }, 1000)

}

const counter = new Count()

// ３秒後に呼び出すので「3」と出力されるはずだが…
setTimeout(() => console.log(counter.count), 3000) 
```

`Count()`は、1秒ごとに+1するコンストラクタである。

それを3秒後に呼び出すので「3」と出力されるはずだが、実際は「0」のままになる。

注①のthisは、`Count()`インスタタンスのthisとして定義されるが、注②のthisは、グローバルオブジェクトのthisとして定義されるため、注①のthis.countはずっと「0」のままになる。

これを回避するための方法としてよく用いいられるのが、`var self = this`というコードを書いて呼び出し先のthisを制御する。

```
function Count() {
  this.count = 0
  var self = this // 一旦、インスタタンスのthisをselfに入れる。
  setInterval(function() {
    self.count++ // selfに変え、呼び出し元をインスタンスの方を参照するようにする。
  }, 1000)

}

const counter = new Count()
// 期待通り「3」と出力される。
setTimeout(() => console.log(counter.count), 3000)
```

ここで、アロー関数に書き方を変更すると、`var self = this`は必要なくなる。

なぜなら、アロー関数だと**thisを束縛できる**からである。

### **「thisを束縛」とは何か？**

従来のアロー関数ではない中のthisは関数が**呼び出された時点でthisが決まっていく。**

つまり、thisが確定していない。thisを束縛していないことになる。

上のソースの例だとsetIntervalのコールバック関数として関数が実行されるためthisはクローバルオブジェクトのthisを指すことになる。

アロー関数の場合、**アロー関数内のthisは定義されて時点で確定（=束縛）する。**

そのため、アロー関数内のthisは`Count()`インスタタンスを指すことになる。

先ほどコードは、以下のようにアロー関数へ書き方を変えることで期待の動作をしてくれる。

```
function Count() {
  this.count = 0
  setInterval(() => {
    this.count++　// インスタンスのthisを参照する。
  }, 1000)

}

const counter = new Count()
setTimeout(() => console.log(counter.count), 3000)
```

以上のように、従来の無名関数の書き方より、アロー関数を使った方がthisの扱いが楽になり、混乱することがなくなるので基本的にはアロー関数を使うよう慣れた方良い。
