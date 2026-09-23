---
title: "【初心者向け】webpackとは？使い方を解説③-Babel設定"
url: "/programming/webpack/beginner-webpack-03/"
date: 2019-08-11
categories: 
  - "webpack"
---

[前回記事](https://blog.pop-web.net/programming/beginner-webpack-02/)の続きです。

Babelの導入ついてです。

webpackのBabelの設定についは以下の公式サイトでも解説があります。

[https://babeljs.io/setup#installation](https://babeljs.io/setup#installation)

飛び先で「webpack」を選択すれば手順がみれます。

## Babelとは

Babelはトランスパイラというツールで、バージョンが新しいJavaScript（ES6など）の場合、新しい記法で書いたときにそれに対応していないブラウザだと、うまく動作しません。

Babelは、そんなモダンなJavaScriptの記法でも対応できるように変換してくれるツールです。

### 必要なものをインスールしていく

最新版を確認し、以下の3つをインストールします。

```
npm i -D babel-loader @babel/core @babel/preset-env
```

### .babelrcを作成

先ほどインストールした@babel/preset-envを有効するには、.babelrcが必要なので作成します。

```
touch .babelrc
```

以下のように編集。

```
{
  "presets": ["@babel/preset-env"]
}
```

### webpack.config.jsの編集

以下を、module:{}のrules:\[\]内に追記。

```
以上、省略…
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: "babel-loader"
            },
以下、省略…
```

それでは、npm webpackでコンパイルします。

そうすると、main.jsが更新されます。

みてみると、ES2015(ES6)で導入されたconstが、varに変換されていたりとちゃんと変換されているのが分かります。

## まとめ

モダンなJavaScriptフレームワークのReactやVueは、ES2015(ES6)以降の記述で書かれているので、Babelの導入は必須になります。

とりあえず、基本的なBabelの設定は以上です。

また次に続きます↓

https://blog.pop-web.net/programming/webpack/beginner-webpack-04/
