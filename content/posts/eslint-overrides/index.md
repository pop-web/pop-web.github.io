---
title: "eslintのルールで特定ファイルのみ適用外にしたい場合の書き方"
url: "/programming/javascript/eslint-overrides/"
date: 2021-05-28
categories: 
  - "javascript"
---

.eslintrcのrulesに「defalut export」の記述を禁止するルールを追加したとする。

```
module.exports = {
...
  rules: {
...
    "import/no-default-export": "error",
...
  }
};
```

このとき、sotrybookのstories.tsxにはこのルールは適用外にしたい。

その場合にはどうするか？

overridesを使って、適用したいファイル名とルールを無効化する記述すればよい。

```
module.exports = {
...
  rules: {
...
    "import/no-default-export": "error",
...
  },
  overrides: [
    {
      files: ["**/*.stories.tsx"],
      rules: {
        "import/no-default-export": "off",
      },
    },
  ],
};
```

## 参考文献

https://eslint.org/docs/user-guide/configuring/configuration-files#configuration-based-on-glob-patterns
