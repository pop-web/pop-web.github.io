---
title: "VueやNuxtの開発でcommitする前にESLintでチェックするやり方"
url: "/programming/vue-js/vue-nuxt-commit-eslint/"
date: 2020-04-05
categories: 
  - "vue-js"
  - "programming"
---

Gitでコミットする際にESLintを走らせて、エラーの場合はコミットさせないようにする。

## 手順

手順的には以下のよう。

- コミットされる前の操作である「pre-commit」を`husky`で制御する。
- `lint-staged`でGitのステージングエリアにあるファイルを対象にESLintのコマンドを実行する。
- Gitコミット時に、ESLintでチェックしてエラーが出ればコミットされない。

## 必要なモジュール

必要なnpmモジュールをインストールする。

```
npm install -D husky lint-staged
```

## package.jsonを設定

package.jsonへ以下のように追記する。

```
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.{js,vue}": [
      "eslint --fix --ext .js,.vue --ignore-path .gitignore --ignore-pattern /cypress/* ."
      "git add"
    ]
  }
```

lint-stagedのプロパティでは、`*.{js,vue}`として対象ファイルをしています。

ちなみに、eslintのコマンドで`--ignore-path`で対象外のファイル、`--ignore-pattern`で対象外のディレクトリを設定している。

## 実際に試してみる

実際にエラーになるパターンとエラーにならないパターンでファイルを編集して、コミットしてみる。

正常に処理されればOK。
