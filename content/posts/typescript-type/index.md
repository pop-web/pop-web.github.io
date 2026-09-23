---
title: "TypeScriptの型まとめ"
url: "/programming/typescript-type/"
date: 2020-06-11
categories: 
  - "typescript"
  - "programming"
---

TypeScriptの方の書き方をまとめていく。

## boolean型

```
const value: boolean;
```

## number型

```
const num: number;
```

## string型

```
const greet: string;
```

## オブジェクト型

```
const cat: {
  name: string;
  age: number;
} = {
  name: "tama",
  age: 2,
};
```

## 配列型

```
const colors: string[] = ["red", "blue"];
```

配列にstring型やnumber型が混在する場合はanyなどが使える。

```
const colors: any[] = ["red", "blue", 9999];
```

## Tuple型

```
const human: [string, number, boolean] = ["man", 26, true];
```

配列の中身の型を制限する。

上記の例だと、1番目はstring型、2番目はnumber型、3番目はboolean型しか受け付けない。

## Enum型

```
// メダルの色を列挙させておく。
// MedalColorという型ができる。
enum MedalColor {
  GOLD = "金メダル",
  SILVER = "銀メダル",
  BRONZE = "銅メダル",
}

const medal = {
  color: MedalColor.GOLD,
};

// 上で列挙した値以外はエラーとなる。

// NG。
medal.color = "金メダル";

// OK。
medal.color = MedalColor.GOLD;
```

Enumは、Enumeration（列挙）の略。

その名の通り、値を列挙されておいてそれ以外は受け付けない。

## Any型

```
let variable: any = "foo";
variable = {};
variable.bar = "bar";
```

any型は、なんでも入れることができる。

つまり、素のJavaScriptと同じ。

TypeScriptを使う意味があまりなくなるので、基本使わないこと。

## Union型

```
let union: number | string = "Hello";
union = 6;
```

複数の型を受け付けれるようにする。

上の例だと、`|`はor演算子のようにnumber型か、string型かを受け付けることができる。

## リテラル型

```
const foo: "foo" = "foo"; // OK
const foo: "foo" = 2; // NG
const foo: "foo" = false; // NG
```

リテラル型は決まった値しか受け付けない。

上の例のように「foo」という文字列のリテラル型しか受け付けなくなる。

ちなみに、リテラルとは、**直接記述されたデータ値**のこと。

上の例では「foo」が文字列リテラル、「2」が整数リテラル、「false」が真偽値リテラルとなる。

## typeエイリアス

```
type StringNumber = string | number;
const foobar: StringNumber = 3;
```

別名をつけることで、型を変数のように扱うことができる。

## 関数宣言で型を使う場合

```
function numCheck(num1: number, num2: number): boolean {
  if (num1 === num2) {
    return true;
  } else {
    return false;
  }
}
```

引数にそれぞれ型付けはする。戻り値の型付けは引数のあとに指定する。

何も返さない関数の場合は、**void型**を使う。

```
function helloWorld(): void {
  console.log("Hello World!");
}
```

## undefined型とnull型

```
const foo: undefined = undefined; // OK
const bar: null = null; // OK

const baz: undefined = null; // OK
const qux: null = undefined; // OK
```

undefined型とnull型はそれぞれundefinedとnullを変数入れることができる。それ以外はエラーとなる。

## 関数の変数に型付けする関数型

```
function NumCheck(num1: number, num2: number): boolean {
  if (num1 === num2) {
    return true;
  } else {
    return false;
  }
}

// 関数を入れる型をつける（型注釈）
const tmpNumCheck: (num: number, num2: number) => boolean = NumCheck;
```

変数へ関数を代入するときに型付けするときに明示的に記述する。

## コールバック関数に型付け

```
function plusOne(val: number, callback: (val: number) => number): void {
  const result = callback(val + 1);
  console.log(result);
}
plusOne(1, (val) => {
  return val * 2;
});
```

前述の関数の型付け同様に、callback関数の後にセミコロンをつけて型付けを指定する。

## unknown型

```
let unknownValue: unknown;
unknownValue = "foo"; // OK
unknownValue = false; // OK
unknownValue = 1; // OK
let bar = unknownValue; // NG

if(typeof unknownValue === 'string'){
  let bar = unknownValue // OK
}
```

代入するときはany型のようになんにでも入れることができるが、他の変数に代入するなど使うときに注意が必要。

typeofで明示的に型のチェックを行えば、使うことができる。

## never型

```
function error(msg: string): never {
    throw new Error(msg);
}
```

存在する可能性がない、値を返すことがない場合に使う。
