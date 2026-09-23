---
title: "【初心者向け解説】PHPのクラス、抽象クラス、インターフェース、トレイトについて"
url: "/programming/php/php-object-orientation/"
date: 2020-12-02
categories: 
  - "php"
  - "programming"
---

まず、オブジェクト指向とはなにかについて説明。

オブジェクト指向は、ざっくりいうと役割をクラスごとに分けてそれを組み合わせながらプログラミングを行っていくこと。

クラスは、変数、定数、関数を一つにまとめたものである。

オブジェクト指向の3つの要素として以下があげられる。

- クラス継承
- カプセル化
- ポリモーフィズム（多様性）

クラスの種類には、クラス、抽象クラス、インターフェース、トレイトというものがある。

それぞれ使い方や特徴は以下の通り。

| **クラスの種類** | **宣言方法** | **使用方法** | **特徴** |
| --- | --- | --- | --- |
| クラス | class | extends | 一般的なクラス |
| 抽象クラス | abstract class | extends | 継承専用のクラス |
| インターフェース | interface | implements | メソッドのみ |
| トレイト | trait | use | useで複数継承できる |

## クラスとインスタンスについて

まず、クラスからインスタンスを作るという基本的なやり方から。

```
<?php

class Human
{
    private $name = [];

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        echo $this->name;
    }

    public static $friendName = '鈴木くん';
}

$instance = new Human('山田くん');

$instance->getName(); // 山田くん

echo Human::$friendName; // 鈴木くん
```

Humanクラスから、newを使ってインスタンスを生成する。

`__construct`は、インスタンスを生成するときに呼び出される初期化処理を書くことができる。

ここでは、インスタンス生成時に渡した値（'山田くん'）が、Humanクラスの`$name`変数に入る。

そして、インスタンス化されたオブジェクトである$intanceの関数である`getName`で値を表示している。

また、staticを使用することで、インスタンスを生成せずに、静的にプロパティやメソッドを呼び出すこともできる。

呼び出し方は、24行目のように「クラス名::メソッド名」または、「クラス名::プロパティ名」という感じ。

## クラスの継承

クラスの継承は`extend`を使って行う。

```
<?php

class BaseHuman
{
    public function echoFunction()
    {
        echo 'BaseHumanのメソッドです。';
    }
}

class Human extends BaseHuman
{
    private $name = [];

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        echo $this->name;
    }
}

$instance = new Human('山田くん');

$instance->echoFunction(); // BaseHumanのメソッドです。
```

BaseHumanというクラスを作成して、Humanクラスで継承する。

そうすると、Humanクラスのインスタンス化したオブジェクトでBaseHumaクラスのメソッドを呼び出すことができる。

継承元と同じメソッドを定義することで上書き（オーバーライド）することができる。

```
<?php

class BaseHuman
{
    public function echoFunction()
    {
        echo 'BaseHumanのメソッドです。';
    }
}

class Human extends BaseHuman
{
    private $name = [];

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        echo $this->name;
    }

    // 上書きオーバーライド
    public function echoFunction()
    {
        echo 'Humanのメソッドです。';
    }
}

$instance = new Human('山田くん');

$instance->echoFunction(); // Humanのメソッドです。
```

上記では、BaseHumanのメソッドである`echoFunction()`を上書きしている。

### 補足：アクセス修飾子について

クラス内で使用する変数や関数にアクセス修飾子とつけて、どこからアクセスできるかを指定することができる。

| **アクセス修飾子** | **説明** |
| --- | --- |
| private | クラス外がアクセスできない、自分のクラスからのみアクセスできる |
| protected | 自分のクラスか、継承したクラスからアクセスできる |
| public | クラス外でも自分のクラスからでもアクセスできる |

## 抽象クラスのついて

抽象クラスは継承される前提のクラスで、継承先で設定するメソッドやプロパティを強制する役割を持つ。

抽象クラスにはclassの前にabstractを付ける。そして、抽象化したいメソッドの前にもabstractを付けることで継承先で強制ができる。

この時、abstractを付けて、abstractメソッドにするとメソッドの中身は書けないことになっている。

abstractメソッドと普通のpublicメソッドも混在して定義することもできる。

継承するには`extend`を用いる。

```
<?php

// 抽象クラス
abstract class humanAbstract
{
    public function echoFunction(){
        echo '抽象クラスのメソッドです。';
    }
    abstract public function getName();
}

// 具象クラス
class Human extends humanAbstract
{
    private $name = [];

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName() // 抽象クラスでabstract設定したため必ず必要なメソッド
    {
        echo $this->name;
    }
}
```

## インターフェースについて

インターフェースと抽象クラスの違いは、すべて強制するメソッドしか描けないこと。

定義する場合は、先頭に`interface`として、使用する場合は`implements`を使う。

抽象クラスの場合、1つの抽象クラスしか継承できないが、インターフェースは複数適用することが可能。

```
<?php

// インターフェース
interface humanInterface
{
    public function getName();
}

// インターフェース
interface foodInterface
{
    public function getFood();
}

// 具象クラス
class Human implements humanInterface,foodInterface
{
    private $name = [];

    public function __construct($name)
    {
        $this->name = $name;
    }

    public function getName() // 抽象クラスでabstract設定したため必ず必要なメソッド
    {
        echo $this->name;
    }
    public function getFood()
    {
        echo '食べ物です';
    }
}

$instance = new Human('山田くん');
$instance->getName(); // 山田くん
$instance->getFood(); // 食べ物です
```

## トレイトについて

先頭に`trait`とつけてclassと同じように定義していく。

使いたいclassの中に`use`を使うことで`trait`の内容を反映することができる。

traitも複数呼び出すことが可能で、上書き（オーバーライド）もできる。

```
<?php

trait HumanTrait
{
    public function getName(){
        echo '名前です';
    }
}

trait FoodTrait
{
    public function getFood(){
        echo '食べ物です';
    }
}

class Human{
    use HumanTrait;
    use FoodTrait;
    public function echoFunction()
    {
        echo 'Humanクラスです';
    }
}

$instance = new Human();

$instance->echoFunction(); // Humanクラスです
$instance->getName(); // 名前です
$instance->getFood(); // 食べ物です
```

以上、クラス、抽象クラス、インターフェース、トレイトの説明終わり。
