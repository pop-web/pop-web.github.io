---
title: "テキスト下に蛍光色ラインマーカーをCSSで作る（改行対応）"
url: "/programming/css/css-line-marker/"
date: 2020-12-31
categories: 
  - "css"
---

改行されているテキストにも対応できるCSSによるラインマーカーの作成。

![](images/16369e672a034d3e1667498ad55d3677.png)

以下、コード。

##### HTML

```
<p>
  テキストテキストテキストテキストテキスト<br />
  <span class="line-marker">テキストテキストテキストテキストテキスト<br />
    テキストテキスト</span>テキストテキストテキスト<br />
  テキストテキストテキストテキストテキスト
</p>
```

##### CSS

```
.line-marker {
	background: linear-gradient(transparent 70%, #FDDEB3 0%);
	display: inline;
	padding: 0 2px 4px;
}
```
