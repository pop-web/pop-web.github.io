---
title: "【初心者向け】SASSについて徹底解説"
url: "/programming/css/sass/"
date: 2019-08-22
categories: 
  - "css"
---

# SASSとは

呼び方は「サス」or「サース」

**S**yntactically **A**wesome **S**tyle**S**heets（_シンタクティクリー・オーサム・スタイルシート_）の略。CSSのメタ言語（プリプロセッサ）。

- **S**yntactically = 構文的に
- **A**wesome = すばらしい
- **S**tyle**S**heets = スタイルシート（CSS）

簡単にいうと、すごいCSS。

CSSを作るときに色々と便利にしてくれるもの。

# なぜSASSを使うのか

CSS作成時の効率化のアップのため。

- コード量が少なくてすむので、見やすい
- 保守性・メンテナンス性の向上
- 変数や関数などが使える

# 記法が2種類ある

記法にはSASS記法とSCSS記法があるが、現在はSCSS記法が使われるのが一般的。

## 拡張子の違い

| 記法 | 拡張子 |
| --- | --- |
| SASS | .sass |
| SCSS | .scss |

## 記法の違い

SASSは、{}や;を省略してインデントだけで入れ子にしていく。

一方、SCSSはCSSにかなり似た書き方ができる。

### SASS

```
div
    margin: 0 auto
    p
        padding: 10px 20px
        span
            font-color: #ff0000
```

### SCSS

```
div {
    margin: 0 auto;
    p {
        padding: 10px 20px;
        span {
             font-color: #ff0000;
        }
    }
}
```

# コンパイル方法

コンパイル方法は、様々ある。

Gulpやwebpackを使って、コンパイラーのモジュールをnpm（パッケージ管理ソフト）でインストールしてオートコンパイルさせるのが今では主流

## 【参考】SASSコンパイルツール

- [prepros](https://prepros.io)
- [sassmeister](https://www.sassmeister.com)

## 【参考】css形式からsass形式にしたい

- [css2sass](https://css2sass.herokuapp.com/)

# SASSの基本機能

SASSの8つの基本機能について

## 1.ネスト（入れ子）

入れ子にすることでまとまった書き方ができるので、見通しがよくメンテナンス性が高い

```
div {
    margin: 0 auto;
    p {
        padding: 10px 20px;
        span {
             font-color: #ff0000;
        }
        &:hover{
             text-decoration-line: underline;
        }
    }
}
```

#### 「&」（アンバサンド）セレクタ

「&」の場合は、ネストではなく親セラクタを参照し合成する。

## 2.変数

SASSでは変数が使える。

変数の宣言の書き方は、「$変数名: 値;」

```
$primary_color:#ff0000;
$fontSize:18px;

.container{
    background:$primary_color;
    font-size:$fontSize
}
```

## 3.演算

四則演算ができます。

```
$baseWidth:980px;

.sub_content{
  width:1000px - $baseWidth;
}

.container{
  width: (680px / $baseWidth) * 100%
}
```

## 4.継承（@extend）

セレクタの内容を、他のセレクタにも適用できる。

```
.button01{
  background:#777;
  border:1px solid #ccc;
  color:white;
}
.button02{
  @extend .button01;
  border-radius:10px;
}
```

## 5.ミックスイン（@mixin）

まとめたスタイルを使うことができる。引数の利用も可能。

よく使うパターンとしてはメディアクエリの指定するときに多用される。

・ブレークポイントの設定

```
// ブレイクポイントを変数に格納、PCとSPのメディアクエリを配列で用意しておく
$breakpoint: 767px;
$breakpoints: (
  "sp": "screen and (max-width: #{$breakpoint})",
  "pc": "screen and (min-width: #{$breakpoint})"
);
```

・出力設定

```
// 引数には初期設定ができる。引数がない場合、初期設定の値が適用される
@mixin mq($breakpoint: sp) {
  @media #{map-get($breakpoints, $breakpoint)} {
    @content;
  }
}
```

・mixinの使用する例

```
// PCではフォントサイズ「18px」で、スマホでは「3.6vw」にしたい場合
span{
  font-size:18px;
  @include mq(){
    fonts-size:3.6vw;
  }
}    
```

## 6.インポート（@import）

Sassファイルは、内容によってファイルを分割することができ、最終的に1つのファイルにまとめることができる。

### 分割

Sassファイルを分割する場合、分割するファイルには**\_(アンダースコア)から始まるファイル名**にする。

これは、パーシャルファイルといってコンパイルされない状態にすることができる。

ヘッダーとフッターと分ける場合、以下のようなファイル名になる。

- `_header.scss` → ヘッダーのスタイルが書かれているファイル
- `_footer.scss` → フッターのスタイルが書かれているファイル

### 統合

Sassファイルを一つにまとめるときには、以下のようにインポートしてまとめる。

ファイルを指定するときは、**\_(アンダースコア)と拡張子（.scss）は省略できる。**

```
@import "./common/header"
@import "./common/footer"
```

## 7.条件分岐・繰り返し処理

プログラミングのように、条件分岐や繰り返し処理も使える

### 条件分岐（@if）

```
$basicColor: true;
@if $basicColor {
  .content {
     color: red;
  }
}
@else {
   .content {
      color: white;
   }
}
```

### 繰り返し処理（@for/@while/@each）

```
@for $v from 0 through 100 {
  .mb#{$v} {
    margin-bottom: #{$v}px;
  }
}
```

## 8.関数

プログラミングのように、関数も使うことができる

```
//pxを%表示に変換する関数
$container: 1020px;
@function percent($pixel) {
  @return floor(($pixel / $container) * 100%);
}

.sub_container {
  width: percent(150px);
}
```
