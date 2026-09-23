---
title: "【初心者解説】React.memo, useMemo, useCallbackとは？"
url: "/programming/react/【初心者解説】react-memo-usememo-usecallbackとは？/"
date: 2021-11-23
categories: 
  - "react"
---

Ractには、メモ化するhookが用意されている。

そもそも**メモ化**とは？

メモ化とは関数実行後の結果をキャッシュか何か保存しておくことによって、再度同じ関数を実行するのではなく、保存しておいた結果を返すこと。それによって実行速度を高速化させるためのものである。

## Reactのパフォーマンスを最適化する

Reactはコンポーネントのpropsやstateが更新されると、その配下の全ての子コンポーネントも再レンダリングされる。

そのため、不要な再計算や再レンダリングを抑えるとことでパフォーマンスを最適化することができる。

その最適化の方法が、メモ化がである。

メモ化を実現する方法として**React.memo**、 **useMemo**、**useCallback**がある。

メモ化の対象するものがそれぞれ異なる。

- **React.memo:** コンポーネントをメモ化するReactのAPI
- **useMemo:** 計算結果をメモ化するHook
- **useCallback:** コールバック関数をメモ化するHook

## React.memo

React.memoを利用することで、コンポーネントの再レンダリングを抑止することができるのでパフォーマンス向上がする。

特に、レンダリング量が多いコンポーネントや再レンダリング頻繁に行われるコンポーネント内の子コンポーネントで有効である

### React.memoの使い方

React.memoの書き方は以下のよう。

```
React.memo(メモ化したいコンポーネント)
```

以下、コードの一例

```
const Hoge = React.memo((props) => <div>{props.name}</div>);
```

[codeSandbox](https://codesandbox.io/s/clever-stonebraker-wjt7m?file=/src/App.js)

React.memoは、渡ってきたpropsが等価であるかをチェックして、等価であれば再レンダリングはされずにメモ化で保存したコンポーネントを再利用する。

**Hogeコンポーネントは、props.nameに変更がなければコンポーネントは再レンダリングされない。**

#### コールバック関数をpropsで渡ってきた場合は再レンダリングされる

関数に関しては再レンダリングされるたびに、新しいオブジェクトを生成するためpropsで渡ってきた場合、等価とは判断されずに再レンダリングされる。

そのため、関数の内容が同じでも再レンダリングされる。

以下がその一例

```
import React, { useState } from "react";

const Hoge = React.memo((props) => {
  console.log("Hoge Render!");  
  return <div onClick={props.handleClick}>{props.name}</div>
});

export const App = () => {
  const [toggle, setToggle] = useState(false);

  //  関数は再レンダリングされるたびに再生成されるため等価評価されない
  const handleClick = () => {
    console.log("Click!");
  };

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
      <button onClick={() => setToggle(!toggle)}>
        Appコンポーネントを再レンダリング！
      </button>
      <br />
      <Hoge name={"fuga"} handleClick={handleClick} />
    </div>
  );
};
```

[codeSandbox](https://codesandbox.io/embed/interesting-fast-4gqko?fontsize=14&hidenavigation=1&theme=dark)

この問題を解消するためには、`useCallback`を使用するとよい。

## useCallback

先述にあるようにReact.memoでメモ化したコンポーネントにコールバック関数が渡ってきた場合は再レンダリングがされる。

そのため、useCallbackを併用する。

useCallbackはメモ化したコールバック関数を返すhookである。

まず、useCallbackでコールバック関数メモ化する。

メモ化したコールバック関数をReact.memoでメモ化したコンポーネントにpropsとして渡すと再レンダリングを抑止することができる。

### useCallbackの使い方

```
useCallback(コールバック関数, 依存配列);
```

依存は配列には、コーバルバック関数が依存している要素を入れる。

依存している要素が更新されれば、関数も新たに生成される。

先程のコードのにuseCallbackを加えてみる。

```
import React, { useState, useCallback } from "react";

const Hoge = React.memo((props) => {
  console.log("Hoge Render!");
  return <div onClick={props.handleClick}>{props.name}</div>;
});

export const App = () => {
  const [toggle, setToggle] = useState(false);

  // 関数をメモ化。等価ななるためHogeコンポーネントは再レンダリングされない
  const handleClick = useCallback(() => {
    console.log("Click!");
  }, []);

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
      <button onClick={() => setToggle(!toggle)}>
        Appコンポーネントを再レンダリング！
      </button>
      <br />
      <Hoge name={"fuga"} handleClick={handleClick} />
    </div>
  );
};
```

[codeSandbox](https://codesandbox.io/s/dark-http-zl0ps?file=/src/App.js:23-712)

注意点は、React.memoでメモ化していないコンポーネントへuseCallbackでメモ化したコールバックス関数を渡しても意味はなく、再レンダリングがされてしまう。

つまり、**useCallbackはReact.memoと併用して使用するものだということ。**

また、useCallbackでメモ化したコールバック関数を、自身のコンポーネントで利用しても動作はするが意味がない。

## useMemo

useMemoは計算結果の値をメモ化して返すhook。

### useMemoの使い方

```
useMemo(() => 計算結果を出すロジック, 依存配列);
```

useMemoは、値が変わらない場合、再計算をさせないことでパフォーマンスを向上することができる。

依存配列には、値を計算する際に依存している要素を入れて、更新された時だけ再計算がされる。

また、React.memoのようにレンダリングもメモ化できる。

以下のコードは2倍にする計算をメモ化している。

Appコンポーネントが再レンダリングされても、doubleが実行されないことが分かる。

```
import React, { useState, useMemo } from "react";

export const App = () => {
  const [toggle, setToggle] = useState(false);

  const double = (count) => {
    console.log("Run double!");
    return count * 2;
  };

　// 2倍する計算結果をメモすることで、再レンダリングされても再計算されない
  const doubleResult = useMemo(() => double(1), []);

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
      <button onClick={() => setToggle(!toggle)}>
        Appコンポーネントを再レンダリング！
      </button>
      <div>{doubleResult}</div>
    </div>
  );
};
```

[codeSandbox](https://codesandbox.io/s/serene-jackson-o78rp?file=/src/App.js)

### レンダリングのメモ化

useMemoはレンダリングもメモ化できる。

以下は、Hogeコンポーネントのレンダリングをメモ化して、不要なレンダリングを抑止している。

注意点として、関数コンポーネント内で、コンポーネントをメモ化したい時にuseMemoでメモ化ができるが、React.memoのようにコンポーネント外でではできない。

逆にコンポーネント内でReact.memoを利用しても意味がない。

```
import React, { useState, useMemo } from "react";

export const App = () => {
  const [toggle, setToggle] = useState(false);

  const Hoge = useMemo(() => {
    console.log("Hoge Render!");
    return <div>fuga</div>;
  }, []);

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
      <button onClick={() => setToggle(!toggle)}>
        Appコンポーネントを再レンダリング！
      </button>
      <br />
      {Hoge}
    </div>
  );
};
```

[codeSandbox](https://codesandbox.io/s/clever-rhodes-lgxbk?file=/src/App.js)

## メモ化をする処理自体のコストについて

useMemoやuseCallbackを使用してメモ化することで、パフォーマンスの向上を見込めるが、何でもかんでもメモ化したらいいわけではない。

なぜなら、メモ化する処理自体にもコストがかかるから。

useCallbackのメモ化のコストは、関数を再生成するコストより大きくなってしまう場合が多いから。

なので、useCallbackをわざわざ利用する必要ないところで使用するとパフォーマンスが悪くなってしまうということ。

明らかに再レンダリングや再計算のコストが高い場合に使用して、適切に使用することがポイントとなる。
