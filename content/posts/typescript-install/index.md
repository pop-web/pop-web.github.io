---
title: "TypeScriptのインストールとJavaScriptへのコンパイル方法"
url: "/programming/typescript-install/"
date: 2020-06-07
categories: 
  - "typescript"
  - "programming"
---

タイトル通りTypeScriptのインストールとJavaScriptのコンパイル方法をやっていく。

## TyepScriptのインストール

Node.jsは事前にインストールされているものとする。

以下のコマンドでTyepScriptをインストールする。

```
npm install -g typescript
```

\-gオプションはクローバルインストールでどこでもTypeScriptを使えるのようにするオプション。

## JavaScriptへのコンパイル方法

適当にtsファイルを作成する。

**・test.ts**

```
const a = "Hello World";
console.log(a)
```

これを以下のコマンドでコンパイルする

```
tsc test.ts
```

実行するとindex.jsファイルが生成されてコンパイルがされていることが確認できる。

**・test.js**

```
var a = "Hello World";
```

## targetオプション

`--target`オプションで指定した仕様にトランスパイルできる。

例えば、ES2015（ES6）仕様にしたい場合、以下のように入力する。

```
tsc index.ts --target es2015
```

ちなみに、`--target`オプションしない場合は、ES3にトラスパイルされる。

## watchオブション

`--watch`オプションで保存と同時に自動コンパイルしてくれる。

```
tsc test.ts --watch
// または
tsc test.ts -w
```

## まとめてコンパイルする場合

複数のtsファイルをコンパイルする場合、まず以下のコマンドでtsconfig.jsonという設定ファイルを作成する。

```
tsc --init
```

設定ファイルを作成してからは、tscコマンドを入力するだけでtsconfig.jsonの設定を見に行ってくれるので複数ファイルでもコンパイルすることができる。

```
tsc
```
