---
title: "npm installしたがキャッシュとパッケージ（node_modules）を削除して再インストールする方法"
url: "/programming/javascript/npm-install-cache-clean/"
date: 2019-11-01
categories: 
  - "javascript"
---

npm installでパッケージをインストールしたが、再インストールして入れなおすには以下のようにする。

```
npm cache clean
rm -rf node_modules/
npm install
```
