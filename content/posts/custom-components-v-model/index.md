---
title: "カスタムコンポーネントでのv-modelの動作について。中身の:valueと@inputを理解する"
url: "/programming/vue-js/custom-components-v-model/"
date: 2020-04-12
categories: 
  - "vue-js"
  - "programming"
---

v-modelは双方向データバインディングを行うディレクティブであるが、カスタムコンポーネントでv-modelを使う場合、少しややこしい。

## v-modelの中身の動作について

まず、v-modelには、別の書き方がある。

以下の2つのinputタグのプロパティは同じ意味である。

```
<template>
  <div>
    <!-- 以下の2つは同じ動作をする -->
    <input v-model="exampleText" />
    <input :value="exampleText" @input="exampleText = $event.target.value" />

    <p>{{ exampleText }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      exampleText: ""
    };
  }
};
</script>
```

**valueプロパティ（:value）とinputイベント（@input）の記述を一つにまとめたものがv-modelである。**

ちなみに、こんな感じに複雑な構文をシンプルに分かりやすくまとめた構文を糖衣構文やシンタックスシュガーと呼ばれる。

## カスタムコンポーネントでv-modelを使う場合

カスタムコンポーネントでv-modelを使う場合でも、v-modelの中身はvalueプロパティとinputイベントである。

以下の2つはコンポーネントは同じ動作になる。

```
<template>
  <div style="padding:10px;">
    <!-- 以下の2つは同じ動作をする -->
    <someComponent v-model="exampleText" />
    <someComponent
      :value="exampleText"
      @input="exampleText = $event"
    />
  </div>
</template>
```

実際に親コンポーネントと子コンポーネントを作成して動作確認をする。

### 親コンポーネント（index.vue）

```
<template>
  <div style="padding:10px;">
    <someComponent
      :value="exampleText"
      @input="exampleText = $event"
    />
  </div>
</template>

<script>
import someComponent from "../components/someComponent";

export default {
  components: {
    someComponent
  },
  data() {
    return {
      exampleText: ""
    };
  }
};
</script>
```

### 子コンポーネント（someComponent.vue）

```
<template>
  <div>
    <input
      :value="value"
      @input="$emit('input', $event.target.value)"
      type="text"
    />
    <p>{{ value }}</p>
  </div>
</template>

<script>
export default {
  props: ["value"]
};
</script>
```

子コンポーネントは、**親のコンポーネントの:valueからpropsでデータを受け取る。**そして`{{ value }}`で表示。

**$emitの第一引数の「input」はで親コンポーネントにあるカスタムイベント@inputを指していて、これを発火させて「exampleText」に「$event」を代入している。**

**$eventの中身は、子コンポーネントの第二引数の値が入っている。**

これで、子コンポーネントで入力されたデータを親コンポーネントに渡すことができる。

## プロパティ名やカスタムイベント名を変更したい時

カスタムコンポーネントでv-modeを使用した場合、デフォルトでプロパティ名はvalueとなり、カスタムイベント名はinputとなる。

これらを変更したい時は、子コンポーネントの方で**modelオプション**を使う。

さっきのソースコードではテキストボックスだったが、チェックボックスに変更する。

ここでは、プロパティ名をcheckedとし、カスタムイベント名をchangeとした。

### 親コンポーネント（index.vue）

```
<template>
  <div style="padding:10px;">
    <someComponent v-model="checkBox" />
  </div>
</template>

<script>
import someComponent from "../components/someComponent";

export default {
  components: {
    someComponent
  },
  data() {
    return {
      checkBox: false
    };
  }
};
</script>
```

### 子コンポーネント（someComponent.vue）

```
<template>
  <div>
    <input
      :checked="checked"
      @input="$emit('change', $event.target.checked)"
      type="checkbox"
    />
    <p>{{ checked }}</p>
  </div>
</template>

<script>
export default {
  model: {
    prop: "checked",
    event: "change"
  },
  props: ["checked"]
};
</script>
```

まとめると、カスタムコンポーネントでv-modelを使う場合、**親コンポーネントと子コンポーネントとの間で具体的にどういったデータのやり取りをしているかを意識する必要がある**ということ。
