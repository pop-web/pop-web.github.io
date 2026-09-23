---
title: "【初心者向け】JavaScriptモジュール使い方（import/export/export default）"
url: "/programming/javascript/javascript-module/"
date: 2019-08-30
categories: 
  - "javascript"
---

JavaScriptのモジュールとは、そのままで部品というイメージ。

JavaScriptで書いたファイルを部品として、他のファイルに取り込むことで機能を拡張することができるのがメリット。

実際に、モジュールの使い方を簡単に説明する。

## モジュールバンドラーをインストール

まず、モジュールを作成した後にそれを束ねる作業が出てくる。

そのため、モジュールハンドラーというツールが必要。

モジュールハンドラーは色々とあるが、今回は人気のwebpackを使う。

作業環境を作っていく。

プロジェクトフォルダを作って、webpackをインストールする。

以下のようにコマンドを実行して初期設定を行う。

```
$ mkdir js-module-test
$ cd js-module-test
$ npm init -y
```

webpackとともにwebpack-cliをインストールする。

webpackだけでは、コンマンドから実行できなくなるため。

```
$ npm i -D webpack webpack-cli
```

バージョンの確認を一応行う。

```
$ npx webpack -v
```

## モジュールを作成

簡単なモジュールを作成する。

plusone.jsという数値を+1だけするJSモジュールを作成。

functionを関数を作成したら、funtionの前に「**export**」をつける。

これで他のファイルからこのモジュールを呼び出す（エクスポート）準備できる。

```
$ vim plusone.js
```

```
export function PlusOne(num){
    return num + 1
}
```

## メインファイルを作成して、モジュールを取り込む

モジュールを取り込む側の、ファイルを作成。

```
$ vim index.js
```

モジュールを取り込む（インポート）にはimportを使う。

```
import { PlusOne } from './plusone'

console.log(PlusOne(10))
```

{}の中に取り込みたい関数名、fromにはファイル名を指定する。拡張子（.js）は必要ない。

実は、**モジュール機能に対応していないブラウザがある**ため、これだけではインポートできない。

**webpackでバンドル（束ねる）する必要**がある。

## バンドルする

index.jsへwebpackを実行させる。

```
$ npx webpack index.js
```

実行すると、distディレクトリが生成され、その中にmain.jsがある。

これを実行する。

```
$ node ./dist/main.js
```

すると、「10」に+1された「11」が出力される。

これで、index.jsへ、plusoneのモジュールがインポートされたこととなる。

## 別名でインポートもできる

取り込む際に、別名「plus\_one」にする

```
import { PlusOne as plus_one } from './plusone'

console.log(plus_one(10))
```

## 定数も取り込める

関数以外にも定数でもインポートできる。

以下のよう。

### エクスポート側

```
export const Const = '定数'
```

### インポート側

```
import { Const } from './plusone'
```

## export defaultをつかう

モジュール側でexportをexport defaultに変更すると、デフォルトでそれがインポートされる。

デフォルトでインポートされるため、{}で囲う必要がなく、好きな名前をつけることができる。

以下のよう。

### エクスポート側

```
export default function PlusOne(num){
    return num + 1
}
```

### インポート側

```
import Plus from './plusone'

console.log(Plus(10))
```

## まとめ

export defaultを使う場合が多いので、覚えておくこと。

以上、モジュールについての説明終わり。
