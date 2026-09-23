---
title: "Vue.jsの開発環境Vue CLIのインストールと初期設定"
url: "/programming/vue-js/vue-cli-install/"
date: 2020-06-08
categories: 
  - "vue-js"
  - "programming"
---

いつもはVue.jsのフレームワークであるNuxt.jsで開発を行なっているが、Vue CLIでの開発環境の構築をしてみる。

事前にNode.jsをインストールしておく。

## Vue CLIのインストール

コマンドラインを起動して、Vue CLIをインストールする。

Vue CLIは基本どこでも使えるようにしたいので、グローバルインストールする。

よって、-gオプションをつけてインストールする。

```
npm install -g @vue/cli
```

終わったらバージョンの確認

```
vue --version
@vue/cli 4.4.1
```

## プロジェクトの作成

以下のコマンドでプロジェクトを作成

```
vue create <プロジェクト名>
```

presetの選択肢がでるので、defaultを選択。

```
Vue CLI v4.4.1
? Please pick a preset: (Use arrow keys)
❯ default (babel, eslint)
  Manually select features
```

インストールが始まるので終わるまで待つ。

## 開発用サーバーの起動

以下の手順のコマンドで開発用サーバーを起動する。

```
cd <プロジェクト名>
npm run serve
```

`http://localhost:8080/`にアクセスし、ブラウザでVue CLIのデフォルト画面が表示されればOK。
