---
title: "PHPでMySQLデータベースへPDO接続方法"
url: "/programming/php/php-mysql-pdo/"
date: 2019-05-31
categories: 
  - "php"
---

PHPで、PDOクラスを使ってMySQLのデータベースへ接続するときのソースです。

```js
<?php
    //DBユーザー名
    $user = "testuser";
    //DBパスワード
    $password = "passwd";
    //利用するデータベース名
    $dbName = "testdb";
    //MySQLサーバ名
    $host = "localhost";
    //MySQLのDNS（Data Source Name）文字列
    $dsn = "mysql:host={$host};dbname={$dbName};charset=utf8";

    //MySQLデータベースに接続する
    try{
        $pdo = new PDO($dsn,$user,$password);
        //PDOクラスのエミュレーションを無効にする
        $pdo->setAttribute(PDO::ATTR_EMULATE_PREPARES, false);
        //例外をスローする設定にする
        $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        echo "データベース{$dbName}に接続しました。";
    }catch(Exception $e){
        echo "データベース接続エラーです";
        echo $e->getMessage();
        exit();
    }
?>
```
