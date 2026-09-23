---
title: "WordPress親テーマのincサブディレクトリ以下のファイルを子テーマで上書きしたい場合"
url: "/programming/child-theme-directory-overwrite/"
date: 2020-04-11
categories: 
  - "wordpress"
  - "programming"
---

当ブログではWordPressのテーマは「Twenty Seventeen」を使っている。

さらに、子テーマを配置してカスタマイズ変更を行っている。

## なぜ子テーマを使うか

基本的なことだが、なぜ子テーマを使用するのかについて。

親テーマを編集しているとテーマのアップデート時にその編集内容がすべて吹っ飛んでしまうためである。

## サブディレクトリ以下のPHPファイルも子テーマで変更制御したい

親テーマのスタイルシート（style.css）を上書きしたい場合は、子テーマディレクトリ直下に同ファイル名のスタイルシート（style.css）配置して、それを編集すればよい。

しかし、親テーマディレクトリの`inc/template-tags.php`の内容を子テーマが側で変更しようと、子テーマディレクトリへ同ファイル名の`inc/template-tags.php`を入れて編集しても変更内容が反映されない。

このような場合は、**子テーマのfunctions.phpへ変更したいソースコードを書く。**

例えば、親テーマディレクトリの`inc/template-tags.php`内の`function twentyseventeen_time_link()`という関数の内容を上書きしたい場合、その該当ソースコードだけを子テーマディレクトリのfunctions.phpにコピペして、変更したい箇所だけ修正すればいよい。

**変更例として、投稿日が更新日より新しい日付の場合は更新日を表示させないというカスタマイズ変更をやっていく。**

まず、子テーマディレクトリのfunctions.phpに`function twentyseventeen_time_link()`の内容を貼り付ける。

### 子テーマのfunctions.phpの編集

```
function twentyseventeen_time_link() {
    $time_string = '<time class="entry-date published updated" datetime="%1$s">%2$s</time>';
    if ( get_the_time( 'U' ) !== get_the_modified_time( 'U' ) ) {
        $time_string = '<time class="entry-date published" datetime="%1$s">%2$s</time><time class="updated" datetime="%3$s">%4$s</time>';
    }

    $time_string = sprintf(
        $time_string,
        get_the_date( DATE_W3C ),
        get_the_date(),
        get_the_modified_date( DATE_W3C ),
        get_the_modified_date()
    );

    // Wrap the time string in a link, and preface it with 'Posted on'.
    return sprintf(
    /* translators: %s: Post date. */
        __( '<span class="screen-reader-text">Posted on</span> %s', 'twentyseventeen' ),
        '<a href="' . esc_url( get_permalink() ) . '" rel="bookmark">' . $time_string . '</a>'
    );
}
```

この中の、

```
if ( get_the_time( 'U' ) !== get_the_modified_time( 'U' ) ) 
```

という箇所を以下のように変更する。

```
if ( get_the_time( 'U' ) < get_the_modified_time( 'U' ) ) 
```

これで投稿日が更新日より新しい日付の場合は更新日を表示できないよう反映できた。

こうすることで、親テーマincサブディレクトリ内のファイルも汚さずに、子テーマ側でカスタマイズ変更がすることができる。
