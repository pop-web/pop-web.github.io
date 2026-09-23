---
title: "【初心者向け】webpackとは？使い方を解説④-css分離化&圧縮化"
url: "/programming/webpack/beginner-webpack-04/"
date: 2019-08-15
categories: 
  - "webpack"
---

[前回記事](https://blog.pop-web.net/programming/beginner-webpack-03/)の続きです。

## スタイルシートを分離するプラグイン

これまでは、スタイルシートはHTML内のヘッダー内に記述されてきました。

ファイルを分けて分離させたいときは、「mini-css-extract-plugin」というプラグインを使っていきます。

インストールしていきます。

```
npm info mini-css-extract-plugin
npm i -D mini-css-extract-plugin@0.8.0
```

### webpack.config.jsを編集

webpack.config.jsを編集します。

requireでプラグインを読み込んで、plugins:の配列に記述します。

```
const path = require('path')
const MiniCssExtractPlugin = require('mini-css-extract-plugin') //プラグインを読込

const outputPath = path.resolve(__dirname,'dist')

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'main.js',
        path:outputPath
    },
    module:{
      rules:[
          {
              test:/\.(sc|c)ss$/,
              use:
              [
                  MiniCssExtractPlugin.loader, //style-loaderの代わりにMiniCssExtractPluginのloaderを追記
                  'css-loader',
                  'sass-loader'
              ]
          }
      ]
    },
    "devServer": {
        contentBase:outputPath
    },
    // プラグインに追加
    plugins: [
        new MiniCssExtractPlugin({
            filename: '[name].[hash].css'
        })
    ]
    op
}
```

「filename: '\[name\].\[hash\].css'」の\[name\]にはmain、\[hash\]には適当なハッシュ値が入ります。

ハッシュ値はキャッシュ対策として入れます。

## ビルドする

それでは、ビルドしてcssファイルが生成されるが確認します。

以下のコマンドを入力して実行

```
npx webpack --mode development
```

そうすると、distディレクトリの中にcssファイルが出力されていると思います。

ファイル名にもハッシュ値が付与されていることが確認できればOKです。

## CSSファイルを圧縮するには？

CSSファイルを圧縮するには「optimize-css-assets-webpack-plugin」というプラグインを使っていきます。

インスールします。

```
npm i -D optimize-css-assets-webpack-plugin
```

### webpack.config.jsを編集

webpack.config.jsを編集します。

以下のように、

const OptimizeCssAssetsPluginと、optimization:のところに追記してあげます。

```
const path = require('path')
const MiniCssExractPlugin = require('mini-css-extract-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
//インストールしたプラグインを取り込む
const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin')

const outputPath = path.resolve(__dirname,'dist')

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'main.js',
        path:outputPath
    },
    module:{
      rules:[
          {
              test:/\.(sc|c)ss$/,
              use:
              [
                MiniCssExractPlugin.loader,
                'css-loader',
                'sass-loader'
              ]
          },
          {
            test:/\.html$/,
            loader:'html-loader'
          }
      ]
    },
    devServer: {
        contentBase:outputPath
    },
    plugins:[
        new HtmlWebpackPlugin({
            template:'./src/index.html',
            filename:'index.html'
        }),
        new MiniCssExractPlugin({
            filename:'[name].[hash].css'
        })
    ],
    //optimizationに追記
    optimization:{
        minimizer:[
            new OptimizeCssAssetsPlugin({})
        ]
    }
}
```

設定が終わったらビルドして確認します。

ビルドするときは、productionモードでビルドしてあげます。

```
npx webpack --mode production
```

そうすると、distディレクトリに圧縮されたcssファイルが生成できています。

## まとめ

webpackでcssファイルを別出力する&圧縮する方法は以上です。
