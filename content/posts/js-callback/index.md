---
title: "【JS初心者向け】コールバック関数について解説"
url: "/programming/javascript/js-callback/"
date: 2019-09-05
categories: 
  - "javascript"
  - "programming"
---

JavaScriptをやっていて、「async/await」ってなんやって思って、ググったら「Promise」の進化版ってことらしい。

今度は「Promise」をググったら、コールバック関数の概念が必要ってことが分かった。

とういうことで、コールバック関数とコールバック関数の必要性について説明する。

## コールバック関数について

コールバック関数とは、「**引数として渡される関数**」こと。

そして、渡された先で関数を実行処理する。

「**あの処理が終わったタイミングで、この関数を実行したい**」といった場面で使われる。

まず、**setTimeout関数**を使って簡単にコールバック関数について説明する。

以下のように、setTimeoutという関数の引数として関数を渡して、setTimeout内で実行させている。

```
setTimeout(() => {
  console.log('Hello,world!')
}, 1000);
```

ここで、**コールバック関数は、「() => { console.log('Hello,world!')}」。**

コールバック関数を受け取る側の関数は、setTimeout()ということになり、この**「コールバック関数を受け取る側の関数」**は、**「高階関数」**と呼ばれる。

### JavaScriptでは「関数」も「値」

関数の引数へ、違う関数を渡すことができると説明した。

これはJavaScriptでは、**関数を値として扱うことができるからだ。**

どういうことかというと、関数を変数に入れることができるということ。

関数を変数に入れるには、関数名の後ろにカッコをつけない。

以下のよう。

```
function hello(){
  console.log('Hello,world!')
}

// 関数を変数へ代入するときは、カッコをつけない
const helloFunc = hello

// 関数を呼び出す場合はカッコをつける
helloFunc() // Hello,world!とコンソールに表示
hello() // Hello,world!とコンソールに表示
```

### 関数に関数を渡して実行することで順序を制御

関数は値なので、関数の引数へ渡すことができる。

そして、引数で渡した関数を渡した関数の中で実行ができる。

さっきのコードにbye()とう関数を追加して、それをhello()関数の引数として渡す。

そして、渡した関数を渡された関数の中で実行する。

```
function hello(func){
  console.log('Hello,world!')
  func()
}

function bye(){
  console.log('Bye!Bye!')
}

hello(bye)
// 表示結果
// Hello,world!
// Bye!Bye!
```

みてわかる通り、**hello()関数の内容が実行された後からではないと、bye()関数が処理されないようになっており、実行順序を制御している。**

このように、「あの関数処理の後に、この関数を実行する」といった処理にしたいときに使うのがコールバックだ。

この場合、**bye()がコールバック関数、hello()が高階関数**になる。

前述したように、高階関数はコーバック関数を受け取る側の関数のこと。

また、bye()の内容をそのまま、hello()の引数に入れて、以下のように書くこともできる。

```
function hello(func){
  console.log('Hello,world!')
  func()
}

hello(function(){
  console.log('Bye!Bye!')
})
// 表示結果
// Hello,world!
// Bye!Bye!
```

さらに、渡した関数に値を渡して実行もできる。

```
function hello(func){
  console.log('Hello,world!')
  func('Bye!Bye!')
}

hello(function(msg){
  console.log(msg)
})
// 表示結果
// Hello,world!
// Bye!Bye!
```

ちなみに、この場合だと**function(msg)がコールバック関数、hello()が高階関数**になる。

## コールバック関数の必要性と使い所について

JavaScriptでコールバック関数の使い所はというと、**「非同期処理」と組み合わせて使われる。**

まず、**JavaScriptは非同期処理を前提として設計されたスクリプト**であることを理解しておく。

非同期処理では、実行順序がコードの通りにならない場合がある。

例えば、APIを呼び出してそのデータを扱いたい場合、APIから渡してもらうデータを全て読み込んだ後に処理を実行しなければ、データを扱うことはできなくなる。

つまり、実行順序を制御する必要がある。

そのため、コールバック関数で制御処理するという方法が用いられる。

次で例を出す。

### コールバック関数と非同期処理の組み合わせ例

非同期処理をする関数であるsetTimeoutを使って、コールバック関数と非同期処理の組み合わせ例と出す。

まずは、コールバック関数なしだとうまくいかない例を出す。

#### うまくいかない例（コールバック関数なし）

```
function getHello(){
  setTimeout(function(){
    return 'Hello,World!'
  },2000)
}

const hello = getHello()
console.log(hello) // undefinedになってしまう
```

これの例だと、getHello()の中の処理が、2秒後にしか実行されないので、console.log(hello)は、undefinedになる。

#### うまくいく例（コールバック関数あり）

先ほどのうまくいかない例に、コールバック関数をつかって書き直す。

```
function getHello(callback){
  setTimeout(function(){
    callback('Hello,World!')
  },2000)
}

getHello(function(msg){
  console.log(msg)
})
```

まず、function(msg){console.log(msg)}というコールバック関数を作成して、getHello関数の引数として渡してあげる。

そして、setTimeoutの中でコールバック関数を実行させてあげることで、2秒後に「Hello,World!」と表示されるようになる。

つまり、**非同期処理をする関数でも、コールバック関数を使えば、実行順序を思い通りに実行することが可能になる。**

しかし、コールバック関数の処理が増えていくと、見た目の構造がどんどん複雑化していく傾向にある。

この複雑化をコールバック地獄という。

次に、コールバック地獄とはなにかについて説明する。

### コールバック地獄とは

コールバック地獄について、簡単に説明する。

以下のように、**2秒おきにメッセージを上から順番に出したい場合。**

- こんにちは、太郎。
- やぁ、こんにちは、花子。
- いい天気だね。
- 今日はどこに行こうか。
- そうだね。とりあえずスタバに行こう。

これを2秒ごとに出してくとなると以下のようなコードになる。

```
function func_01(callback) {
  setTimeout(function () {
    console.log("こんにちは、太郎。");
    callback();
  }, 2000);
}

function func_02(callback) {
  setTimeout(function () {
    console.log("やぁ、こんにちは、花子。");
    callback();
  }, 2000);
}

function func_03(callback) {
  setTimeout(function () {
    console.log("いい天気だね。");
    callback();
  }, 2000);
}

function func_04(callback) {
  setTimeout(function () {
    console.log("今日はどこに行こうか。");
    callback();
  }, 2000);
}

function func_05() {
  setTimeout(function () {
    console.log("そうだね。とりあえずスタバに行こう。");
  }, 2000);
}

func_01(function () {
  func_02(function () {
    func_03(function () {
      func_04(function () {
        func_05();
      });
    });
  });
});
```

最後の呼び出している関数が**入れ子の入れ子の…状態**になっている。

とても分かりにくくする。

これを俗に、**コールバック地獄**という。

そして、コールバック地獄対策のために登場したのが**「Promise」**で、非同期処理を簡潔に書くことが可能になる。

Promiseについては下記↓

https://blog.popweb.dev/programming/javascript/js-promise/
