---
title: "【Laravel】中間テーブルのカラムに対して検索をかけたい時【whereHas】"
url: "/programming/laravel/【laravel】中間テーブルのカラムに対して検索をかけ/"
date: 2021-03-17
categories: 
  - "laravel"
---

タイトル通りだが、まずはやりたいことの説明

## やりたいこと

リレーション先の中間テーブルのカラムの値に対して検索をかけたい。

## 状況

menus、stores、menu\_store（中間テーブル）とうテーブルを作成しており、

モデルは以下のような感じで中間テーブルを置いてリレーションしている。

```
class Menu extends Model
{    
    public function stores()
    {
      return $this->belongsToMany('App\Store','menu_store','menu_id','store_id')->withTimestamps();
    }
}
```

```
class Store extends Model
{    
    public function menus()
    {
      return $this->belongsToMany('App\Menu');
    }
}
```

## 解決方法

**whereHasを使う。**

以下だと、中間テーブルであるmenu\_storeのstore\_idでwhereInで条件を絞りたい時の場合。

```
$storeId = [1,2,3]
$query = Menu::query();
$query->whereHas('stores', function($q) use($storeId)  {
    $q->whereIn('menu_store.store_id', $storeId);
});
```

コールバック関数の中でリレーション先である中間テーブルの検索条件を指定してあげれば良い。

中間テーブルのカラムではなく、中間テーブルを介して結合しているテーブル（storesテーブル）のカラムで検索することも可能。

その場合も同様でコールバック関数の中の詳細条件を変更すれば良い。

```
$q->whereIn('store.store_id', $storeId)
```
