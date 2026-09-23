---
title: "【初心者向け】webpackとは？使い方を解説②-Loader設定"
url: "/programming/webpack/beginner-webpack-02/"
date: 2019-08-10
categories: 
  - "webpack"
---

[前回の記事](https://blog.pop-web.net/programming/webpack/beginner-webpack-01/)の第2弾です。

webpackにJavaScriptファイルをバンドルする機能以外にも様々な便利機能があります。

Loaderについて解説します。

## Loader（ローダー）とは？

JavaScript以外のファイルをJavaScriptとして扱えるようにする機能です。

## LoaderでCSSを扱う

### css-loaderのインストール

「css-loader」でcssファイルをJavaScriptのオブジェクトとしてして扱えるようにできます。

インストールをします。

```
npm info css-loader //最新版の確認
npm i -D css-loader@3.2.0 //インストール
```

webpack.config.jsに設定を記載していきます。

ローダー登録には、moduleプロパティに以下のように書きます。

```
const path = require('path')

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
              test:/\.css$/, //testには正規表現でcss拡張子を設定
              use:['css-loader'] //useには使用するローダーを設定
          }
      ]
    },
    devServer: {
        contentBase:outputPath
    }
}
```

### cssファイルを作成する

```
touch src/style.css
```

以下のように編集

```
.bdy-color{
    background:#b3ff66;
    color: #ff1e74;
}
```

JavaScriptにインポートします。

index.jsへ以下のようにスタイルシートをimportさせます。

```
import $ from 'jquery'
import './style.css' //作成したstyle.cssをインポート

$(function () {
    $('body').prepend('<p>Hello,webpack!</p>')
})
```

### style-loaderをインストール

これで、スタイルシートをimportさせることができたのですが、

このスタイルシートをindex.htmlに適用されるには、もうひとつローダーが必要で「style-loader」というものをインストールします。

```
npm info style-loader //最新版の確認
npm i -D style-loader@1.0.0 //インストール
```

### webpack.config.jsを編集

それでは次に、webpack.config.jsのuseにstyle-loaderを追加します。

ここで、**_注意点としては「css-loader」の上に「style-loader」を書いて下さい。_**

なぜなら、**_ローダーは下から上に順番に実行されていくからです。_**

この場合、「css-loader」でstyle.cssをJavaScriptのオブジェクトとして扱えるようにしてから、「style-loader」でスタイルタグとしてそれを読み込むという順序になります。

逆にすると上手くいかないので注意です。

```
const path = require('path')

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
              test:/\.css$/,
              use:
              [
                  'style-loader', //style-loaderを追記（※css-loaderの上に！）
                  'css-loader'
              ]
          }
      ]
    },
    devServer: {
        contentBase:outputPath
    }
}
```

次に、index.jsを編集します。

以下のように、bodyタグにクラス名「bdy-color」を付与させるようにします。

```
import $ from 'jquery'
import './style.css'

$(function () {
    $('body').prepend('<p>Hello,webpack!</p>')
})

document.body.classList.add('bdy-color') //bodyタグにクラス名「bdy-color」を付与
```

これで、`npm run start`を実行してブラウザで確認します。

CSSが適用されて背景色が緑で、文字がピンクになっていればOKです。

## LoaderでSASSを扱う

### sass-loaderのインストール

sassファイルをJavaScriptオブジェクトとして扱うためローダーです。

インストールします。

```
npm info sass-loader //最新版の確認
npm i -D sass-loader@7.2.0 //インストール
```

次に、sassをcssにコンパイルするためのローダーであるnode-sassインストールします。

```
npm info node-sass //最新版の確認
npm i -D node-sass@4.12.0 //インストール
```

### webpack.config.jsを編集

useに「'sass-loader'」を追記します。

ここでも注意点としては、ローダーは下から上へ処理されていくので、sass-loaderは一番初めに処理させたいので、一番下に記載します。

```
const path = require('path')

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
              test:/\.(sc|c)ss$/, //正規表現でcss、scssのどちらも適用されるように
              use:
              [
                  'style-loader',
                  'css-loader',
                  'sass-loader' //sass-loaderを追記（※一番下に記載）
              ]
          }
      ]
    },
    devServer: {
        contentBase:outputPath
    }
}
```

### scssファイルを作成

```
touch src/style.scss
```

sassでは変数が使用できるので、

以下のように編集しておきます。

```
$big-font:100px;
$font-style:italic;

.bdy-color{
  font-size: $big-font;
  font-style: $font-style;
}
```

次に、index.jsを編集します。

index.jsへ以下のようにsassファイルをimportさせます。

```
import $ from 'jquery'
import './style.css'
import './style.scss' //作成したsassファイルをインポート

$(function () {
    $('body').prepend('<p>Hello,webpack!</p>')
})

document.body.classList.add('bdy-color')
```

それでは、`npm run start`をしてブラウザで確認します。

scssファイルで設定したように、文字が大きくなり斜体文字になっていればOKです。

## Loaderで画像を扱う

### 画像を用意

srcの中に、適当に画像を用意しておきます。

```
src/sample.jpg
```

### url-loaderのインストール

画像ファイルをJavaScriptオブジェクトとして扱うためローダーです。

インストールします。

```
npm info url-loader //最新版の確認
npm i -D url-loader@2.1.0 //インストール
```

次に、sassをcssにコンパイルするためのローダーであるnode-sassインストールします。

### webpack.config.jsを編集

moduleのなかのrulesに追記します。

```
以上、省略…
            {
                test:/\.(gif|jpe?g|png|svg|ico)$/i,
                loader: 'url-loader'
            }
以下、省略…
```

次に、index.jsを編集していきます。

```
import $ from 'jquery'
import './style.css'
import './style.scss'
import photo from './sample.jpg'

$(function () {
    $('body').prepend('<p>Hello, webpack!!</p>')
})

document.body.classList.add('bdy-color')

//imgタグをbodyに挿入
const image = new Image()
image.src = photo
document.body.appendChild(image)
```

それでは、`npm run start`をしてブラウザで確認します。

画像が挿入されていればOKです。

ここで、ソースを見るとimgタグにはbase64でエンコードされた文字列データとして使用されていることが分かります。

base64でエンコードすることでサーバへのリクエストが減り表示速度が速くなる利点がありますが、

**大きな画像になるほど、Base64にエンコードされた文字列は増えていくため、逆にファイルが重くなります。**

そこで、base64ではなくて、通常の画像パスを読み込む形式に変えてくれる「file-loader」がローダーがあります。

### file-loaderをインストール

```
npm info file-loader
npm i -D file-loader@4.2.0
```

### webpack.config.jsを編集

url-loaderの設定内にoptionsを追加し、file-loaderを有効にします。

```
以上、省略…
            {
                test:/\.(gif|jpe?g|png|svg|ico)$/i,
                loader: 'url-loader',
                //optionsを追記
                options: {
                    limit:1024, //閾値は、1キロバイト以上に設定
                    name:'./images/[name].[ext]' //画像ファイルを分離して読込む
                }
            }
以下、省略…
```

limitは容量を指定して、これ以上の容量だったらnameで設定した画像パスにになるといった感じです。

## LoaderでHTMLファイルを扱う

HTMLファイルをローダーで扱うには、「html-loader」を使用します。

インストールします。

```
npm info html-loader
npm i -D html-loader@0.5.5
```

次に、HTMLを生成するためのプラグインをインストールします。

```
npm info html-webpack-plugin
npm i -D html-loader@0.5.5
```

### webpack.config.jsを編集

html-loaderとプラグインの設定を以下のように行います。

```
const path = require('path')
// プラグインの読み込み
const HtmlWebPackPlugin = require('html-webpack-plugin')

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
                  'style-loader',
                  'css-loader',
                  'sass-loader'
              ]
          },
          //html-loaderの設定
          {
            test:/\.html$/,
            loader:'html-loader'
        }
      ]
    },
    devServer: {
        contentBase:outputPath
    },
    // プラグインの設定
    plugins:[
        new HtmlWebPackPlugin({
            template:'./src/index.html',
            filename:'./index.html'
        })
    ],
    devtool:'source-map'
}
```

プラグインの設定では、「template」には、ひな形となるhtmlファイルの設定。

「filename」に出力時のファイル名を設定します。

### テンプレートとなるhtmlファイルを作成

テンプレートとなるhtmlファイルを作成します。

```
touch src/index.html
```

以下のように編集しておきます。

```
<!doctype html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Hello,World!</title>
</head>
<body>
</body>
</html>
```

これで、ビルドします。

```
npm run start
```

これで、ブラウザが起動し、「Hello,webpack!」と表示され、スタイルシートも適用されていればOKです。

## LoaderでESLintを 扱う

ESLintってなにかっていうと、JavaScriptの構文チェックツールです。

導入することいによって、構文エラーをチェックしてくれるので初期段階でバグの発見することができます。

また、コードルールを統一することができるので、開発の手間を省くことできます。

ESLintをインストールします。

```
npm info eslint
npm i -D  eslint@6.1.0
```

eslint-loaderをインストールします。

```
npm info eslint-loader
npm info eslint-loader@2.2.1
```

初期化処理をします

```
npx eslint --init
```

そうすると、設定の選択をいくつかしていきます。

今回は以下のような選択をしました。

```
? How would you like to use ESLint?
To check syntax, find problems, and enforce code style
? What type of modules does your project use?
JavaScript modules (import/export)
? Which framework does your project use?
None of these
? Where does your code run? (Press <space> to select, <a> to toggle all, <i> to invert selection)
Browser
? How would you like to define a style for your project?
Use a popular style guide      
? Which style guide do you want to follow?
Airbnb (https://github.com/airbnb/javascript)
? What format do you want your config file to be in?
JSON
Checking peerDependencies of eslint-config-airbnb-base@latest
The config that you've selected requires the following dependencies:
? Would you like to install them now with npm?
Yes
```

初期化処理の設定が終わると、「.eslintrc.json」というファイルが出来上がっていると思います。

次に、webpack.config.jsを編集して、eslint-loaderの設定を追記します。

```
const path = require('path')
const HtmlWebPackPlugin = require('html-webpack-plugin')

const outputPath = path.resolve(__dirname, 'dist')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: outputPath,
  },
  module: {
    rules: [
    // eslint-loaderの設定追記、enforceは優先的にローダを実行する設定
      {
        enforce: 'pre',
        test: /js$/,
        exclude: /node_modules/,
        loader: 'eslint-loader',
      },
      {
        test: /\.(sc|c)ss$/,
        use:
              [
                'style-loader',
                'css-loader',
                'sass-loader',
              ],
      },
      {
        test: /\.html$/,
        loader: 'html-loader',
      },
    ],
  },
  devServer: {
    contentBase: outputPath,
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: './src/index.html',
      filename: './index.html',
    }),
  ],
  devtool: 'source-map',
};
```

構文チェックは、他のローダーが実行される前に行いたいので、eslint-loaderの優先順位を一番にします。

そこで、「enforce: 'pre'」という設定をします。

これは、一番初めにローダーを実行するという設定です。

次に、babel-eslintをインストールします。

これは、babelのバーサーでbabelによるトランスファイル前のJavaScriptをチェックするために必要になります。

```
npm i -D babel-eslint
```

「.eslintrc.json」でパーサーの設定します。

```
{
    "env": {
        "browser": true,
        "es6": true
    },
    "extends": [
        "airbnb-base"
    ],
    "globals": {
        "Atomics": "readonly",
        "SharedArrayBuffer": "readonly"
    },
    "parserOptions": {
        "ecmaVersion": 2018,
        "sourceType": "module"
    },
    "rules": {
    },
    "parser":"babel-eslint" //パーサー設定を追記
}
```

それでは、構文チェックするためファイルを指定して、eslintを実行します。

```
npx eslint src/index.js
```

構文エラーなどがでてきたら、チェックが正常に行われていることが分かります。

自動で構文エラーをなおす場合は「--fix」のオプションをつけます。

```
npx eslint --fix src/index.js
```

再度、eslintを実行してエラーが出なくなっていればOKです。

## まとめ

とりあえず。Loaderの基礎については以上です。

次に続きます↓

https://blog.pop-web.net/programming/webpack/beginner-webpack-03/
