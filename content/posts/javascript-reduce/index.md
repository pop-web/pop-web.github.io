---
title: "【JavaScript入門】reduceについて説明と使い方"
url: "/programming/javascript/javascript-reduce/"
date: 2021-01-26
categories: 
  - "javascript"
---

reduce()の説明と使い方を説明する。

reduce()は、配列の隣り合う値に対して関数を適用させて、その実行結果を返すメソッド。

コードを書いて動きを確認してみる。

```
const arr = [1, 2, 3]

console.log(arr.reduce((acc, cur) => acc + cur)) // 6
```

まず、第2引数のcurには、arrの値が順番に入ってくる。

第1引数accには前回実行時の値が入ってくる。

そして、返値として最後に実行された値を返す。

具体的に実行処理は以下。

- 1回目の実行：cur=1となり、前回実行の値はないので、1が返る
- 2回目の実行：cur=2となり、前回実行の値はacc=1なので、1+2=3が返る
- 3回目の実行：cur=3となり、前回実行の値はacc=1なので、3+3=6が返る
