---
title: "Laravelで1ページ内に複数のpaginateを分けてページング方法"
url: "/programming/laravel/laravel-multiple-paginate/"
date: 2020-03-16
categories: 
  - "laravel"
  - "programming"
---

Laravelでpaginateを1つのページで複数設置すると、ページングするに際にすべてが同時にページングされてしまう。

以下のようにすると、paginateの第3引数に設定したパラメータでそれぞれでリクエストがなされる。

```
public function show(Request $request)
{
    $tags = Tag::paginate(10, ['*'], 'tags');
    $comments = Comments::paginate(10, ['*'], 'comments');

    return view('show.index', compact('tags','comments'));
}
```

`/show?page=2`だったものが、`/show?tags=2`と`/show?comments=2`とで分けられて同時にページングがされなくなる。

ページングする際に、どちらのパラメータも残しつつページングを行いたい場合は、

以下のように`appends`を使うとよい。

```
public function show(Request $request)
{
    $tags = Tag::paginate(10, ['*'], 'tags')->appends(["comments" => $request->input('comments')]);
    $comments = Comments::paginate(10, ['*'], 'comments')->appends(["tags" => $request->input('tags')]);

    return view('show.index', compact('tags','comments'));
}
```

これで両方でページングを行なっても、`/show?tags=2&comments=2`となりそれぞれのページング状態が保持される。
