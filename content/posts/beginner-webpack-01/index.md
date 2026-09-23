---
title: "【初心者向け】webpackとは？使い方を解説①"
url: "/programming/webpack/beginner-webpack-01/"
date: 2019-08-08
categories: 
  - "webpack"
---

## webpackって？

フロントエンド開発の現場において使用されており、作業の効率化を目的として**_「モジュールバンドラー」_**と呼ばれる便利ツール。

## モジュールバンドラーって？

CSS、JavaScript、画像といったアセットファイルをJavaScriptのモジュールに変換して合体させるものです。

module（モジュール）は「部品」で、bundle（バンドル）は「束ねる」という意味。

webpackの他にも、browserify、RequireJS、rollup.jsといったモジュールハンドラーが存在しますが、webpackがモジュールハンドラーの中で一番人気なので、とりあえずwebpackを知っとけばOKという感じです。

## モジュール化して統合させるのってなんで？

webサイトでは、CSS、JavaScript、画像といった必要なファイルが増えるにつれて、サーバーになんどもリクエストしなければならなくなります。

そうなると、画面表示をするためにコストがかかり表示速度が遅くなってしまいます。

そこで、webpackの登場。

webpackは、様々なアセットファイル（JavaScript、CSS、画像など）を1つのファイルに統合してくれるため、サーバーへのリクエスト数も減らせるわけです。

しかも、**単に統合させるだけでなく依存関係を解決してから統合してくれます。**

## 事前に必要なもの

- Node.jsが必要【LTS(Long Time Support)が推奨】

[公式サイト](https://nodejs.org/ja/)へ行って、「推奨版」てやつをダウンロードしておけばOK。

## webpackインストールの準備

webpackをインストールするための準備をします。

プロジェクトフォルダを作成して、npmの初期化処理をしておきます。

```
mkdir webpack-test
npm init -y
```

## webpackのインストール

最新バージョンを確認

```
npm info webpack
```

「latest:〇〇〇」と書かれているところが最新版なので、それをインストールします。

webpackの後に@マークを足して、そのあとにバージョンを書きます。

```
 npm i -D webpack@4.39.1
```

「i」は、installの省略で、「-D」は--save-devの省略です。

開発環境のみ使用するパッケージの場合は、「-D」（--save-dev）オプションを付けます。

つづて、コマンドラインからwebpackを利用するためのパッケージである「webpack-cli」もインストールします。

同様に、最新パージョンを確認してインストールします。

```
npm info wepack-cli
```

```
npm i -D webpack-cli@3.3.6
```

package.jsonを確認してみます。

```
  "devDependencies": {
    "webpack": "^4.39.1",
    "webpack-cli": "^3.3.6"
  }
```

devDependenciesに、webpackとwebpack-cliがあればOK。

## ディレクトリを作成する

次に、バンドルする前のソースファイルを置くsrcディレクトリと、バンドルされて吐き出されるdistディレクトを作成しておきます。

```
mkdir src
mkdir dist
```

### index.htmlを作成

distディレクトリにindex.htmlを作成しておきます。

```
touch dist/index.html
```

index.htmlのソースは以下のように適当でOK。

あとで、バンドルされて生成されるmain.jsをここで読み込んでおきます。

```
<!doctype html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <title>Hello,World!</title>
</head>
<body>
    Hello,World!
    <script src="main.js"></script>
</body>
</html>
```

### JavaScriptを作成

srcディレクトリにindex.jsを作成します。

```
touch src/index.js
```

今回は、jQueryを使用してスクリプトを作っていこうと思うので、

npmでjQueryをインストールします。最新版を確認したらそれをインストールします。

```
npm info jquery
```

```
npm i -S jquery@3.4.1
```

「-S」は、--saveの省略です。jQueryは開発環境だけでなく、本番環境でも必要なパッケージなのでこのようにします。

index.jsにjQueryをインポートしてjQueryのスクリプトを書いていきます。

以下のように書いてみました。

bodyタグの中にpタグをを挿入して「Hello, webpack!!」という文字を出力するjQueryのスクリプトです。

```
import $ from 'jquery'

$(function () {
    $('body').prepend('<p>Hello, webpack!!</p>')
})
```

## webpackでバンドルを実行

webpackコマンドを実行して、バンドルしていきます。

以下のようにコマンドを入力してwebpackを実行します。

```
npx webpack --mode development
```

「 --mode development」のオプションは、モードオプションを設定する必要があり、今回は開発中ということで「development」を指定します。

本番環境の場合は、「--mode production」とします。

distディレクトリを確認してみてください。

main.jsが生成されているのが分かると思います。

### ブラウザで確認

それでは、index.htmlをブラウザで開いて確認します。

「Hello, webpack!!」と表示されればOKです。

これからわかるように、**_jQueryのライブラリと自分で書いたindex.jsとが合体してmain.jsになった_**ということが分かると思います。

## webpack.config.jsの作成と設定

`npx webpack`を実行すると、srcディレクトリのjsファイルをバンドルして、distディレクトリにmain.jsとして出力するという流れになっていました。

こういった、どこのファイルをバンドルして、どこへ出力するのかを設定する設定ファイルが「webpack.config.js」というものです。

### entryとoutputの設定

「webpack.config.js」を作成して編集します。

```
touch webpack.config.js
```

以下のように編集します。

```
const path = require('path') //現在のパスを変数へ

const outputPath = path.resolve(__dirname,'dist') //distまでの絶対パスの生成

module.exports = {
    entry:'./src/index.js', //バンドルする対象ファイルの指定
    output:{
        filename:'main.js', //バンドルされたファイル名の指定
        path:outputPath //バンドルされたファイルを出力する場所の指定
    }
}
```

コードの意味はコメントアウトで書いた通りです。

編集したら、再度webpackを実行してバンドルされていることを確認しておきます。

```
npx webpack --mode development
```

コマンドを実行する同じ階層に「webpack.config.js」がなく、他の階層にある場合、オプションを付けてパス指定で実行することができます。

```
npx webpack --mode development --config (path)/webpack.config.js
```

## ライブサーバのパッケージで快適に

ファイルの更新されたら自動リロードしてくれる「webpack-dev-server」をインストールします。

最新版を確認してからインストールします。

```
npm info webpack-dev-server
npm i -D webpack-dev-server@3.7.2
```

それでは、実行します。

```
npx webpack-dev-server
```

http://localhost:8080/へアクセスしてください。

そしてdistをクリックしてindex.htmlの表示を確認してください。

注意点としては、webpack-dev-server起動中にファイルが変更されると自動リロードされて更新されますが、

dist内のファイルへのビルドはされないで、適宜、npx webpackでビルドする必要があります。

### ドキュメントルートを変更したい場合

今の状態だと「/dist」にアクセスしないとindex.htmlは表示できません。

http://localhost:8080/でindex.htmlを表示されるようにするには、webpack.config.jsを編集すればできます。

webpack.config.jsへ、以下のようにdevServerプロパティを追記をしましょう。

```
const path = require('path')

const outputPath = path.resolve(__dirname,'dist')

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'main.js',
        path:outputPath
    },
    devServer: {
        contentBase:outputPath //内部サーバーのドキュメントルートをdistディレクリに設定
    }
}
```

設定し終わったら、再度`npx webpack-dev-server`を実行してブラウザでindex.htmlがhttp://localhost:8080/で確認できることを確認しておきましょう。

## package.jsonのタスクの登録で快適に

package.jsonにタスク登録というものをしておくことで、より効率的に開発が行えるようになります。

package.jsonの「"scripts":」のところを以下のように編集しておきます。

```
  "scripts": {
    "start": "npx webpack-dev-server --open"
  },
  以下省略・・・
```

このように登録しておくことで、npm run 〇〇という形でツールを実行することができます.

では、実際に実行します。

```
npm run start
```

同様に、内部サーバーが立ち上がっていることを確認してください。

ちなみに、「--open」のオプションを付けることで、自動でブラウザを起動してくれます。

## ソースマップの作成

コードにエラーがあった時に、ブラウザ上からどこでエラーが発生しているか分かりやすくするためにソースマップの作成を行います。

webpack.config.jsを編集します。

devtool:source-mapを追記します。

```
const path = require('path')

const outputPath = path.resolve(__dirname,'dist')

module.exports = {
    entry:'./src/index.js',
    output:{
        filename:'main.js',
        path:outputPath
    },
    devServer: {
        contentBase:outputPath
    },
    devtool:'source-map' //ここに追記
}
```

これで、エラーなどをブラウザ上で確認する際には、ビルド前のファイルの状態で確認することができるようになり、開発がしやすくなります。

## まとめ

とりあえず、webpackの基本的な機能は以上です。

次に続きます↓

https://blog.popweb.dev/programming/webpack/beginner-webpack-02/
