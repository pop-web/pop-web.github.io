---
title: "【JavaScript】プリミティブ型（基本型）とオブジェクト型（参照型）について"
url: "/programming/javascript/primitive-object/"
date: 2021-10-16
categories: 
  - "javascript"
  - "programming"
---

JavaScriptにのデータ型には、プリミティブ型（基本型）とオブジェクト型（参照型）がある

## プリミティブ型（基本型）

```
真偽値（boolean）
数値（number）
文字列（string）
null
undefined
シンボル（symbol）
```

## オブジェクト型（参照型）

プリミティブ型（基本型）以外のデータ型

```
オブジェクト（object）
配列（array）
関数（function）
```

## プリミティブ型とオブジェクト型の違い

プリミティブ型は変数をコピーする時、変数の中身の値がそのままコピーされる。

オブジェクト型は変数をコピーする時、その値がコピーされるのではなく、そのオブジェクトが持っているメモリ上のアドレスがコピーされる。

以下がその例

### プリミティブ型の場合

```
let a = 20;
let b = a;
a = 80;
console.log(a); // 80
console.log(b); // 20
```

bにaを代入した後にaに新たに100の値を代入しているが、bに影響は受けない。

さっきも説明したように、メモリ参照せずに変数の値をそのままコピーしていることが分かる。

### オブジェクト型の場合

```
let obj1 = {
  name: "ken",
  age: 20
};
let obj2 = obj1;
obj1.age = 80;
console.log(obj1.age); // 100
console.log(obj2.age); // 100
```

obj2にobj1を代入することで、obj1もobj2も同じメモリ上のアドレスと保持するようになったため、片方のageの値を変更すると、もう片方の値も同じになる。
