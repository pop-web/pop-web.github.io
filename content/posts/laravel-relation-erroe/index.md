---
title: "Laravel リレーション先の値がなくて、Trying to get property of non-objectがでちゃった"
url: "/programming/laravel/laravel-relation-erroe/"
date: 2019-06-10
categories: 
  - "laravel"
---

DBのメインテーブルからサブテーブルへリレーションさせて、メインテーブル側から値を取得しようとしたらエラー**「Trying to get property 'time' of non-object」**が出た。

コントロール側での値の呼び出し方はこんな感じ。

```
function getTime(){
    $time = $this->hasOne('App\Time')->first()->time;
}
```

そして、view側ではforeashでまわす。

```
@foreach($item->getTime() as $yobi => $time)
<dl class="col-sm">
    <dt>{{$yobi}}</dt>
    <dd>{{$time}}</dd>
</dl>
@endforeach
```

## 原因は、リレーション先の値（time）が全て無いため。

foreachで回す場合、メインテーブルとサブテーブルでリレーションは同じだけ用意しないとエラーが出る。

でも、サブテーブルの情報が全てそろってないことがほとんど。

なので、ない場合はNullで返すか、値を呼び出さないようにすればよい。

## 解決方法は２パターン

解決方法は２つある。

- **ヘルパー関数「optional()」を使う**
- **if文でNullでないことを確認する**

### **ヘルパー関数「optional()」を使う**

ヘルパー関数「optional()」は、リレーション先の値がない場合Nullで返してくれる関数。

```
$time = optional($this->hasOne('App\Time')->first())->time;
```

### i**f文でNullでないことを確認する**

こっちの方がよくあるパターン。単純にNullでなかったら、値を返すというもの。

```
$time = $this->hasOne('App\Time')->first()->time;
if($time != null){
    return $time;
}
```

これら2つのどっちかで、non-objectエラーは回避できる。
