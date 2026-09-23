---
title: "Vue.jsエラー。gyp: No Xcode or CLT version detected!"
url: "/programming/vue-js/gyp-no-xcode-or-clt-version-detected/"
date: 2020-06-10
categories: 
  - "vue-js"
  - "programming"
---

`vue create <プロジェクト名>`コマンドを入力すると以下のようなエラーがでた。

```
gyp: No Xcode or CLT version detected!
```

XcodeあるいはCLTのバージョンがおかしいらしい。

## 解決方法について

GitHubの[node-gyp](https://github.com/nodejs/node-gyp/blob/master/macOS_Catalina.md#i-did-all-that-and-the-acid-test-still-does-not-pass--)にあったものを参考にした。

```
sudo rm -rf $(xcode-select -print-path)
sudo rm -rf /Library/Developer/CommandLineTools
xcode-select --install
```

これで再度、`vue create <プロジェクト名>`コマンドを入力するとエラーも出ずにプロジェクトが作成された。
