---
title: JSイベントバブル＆キャブチャー
category: JavaScript
abbrlink: 3a04c871
date: 2024-03-06 13:48:25
---

## イベント三段階

JavaScriptのイベントが発生する時、三つの段階があります。

+ キャプチャー段階：一番上の要素（ウィンドウ要素）からターゲット要素まで配信する段階；
+ ターゲット段階：ターゲット要素に辿り、リスナーでバインドした関数が実行されます；
+ バブル段階：キャプチャー段階の逆で、ターゲット要素から一番上の親要素まで配信する段階。

![image-20240306140213733](https://cdn.jsdelivr.net/gh/try0929/cdn/img/image-20240306140213733.png)

## JavaScriptのaddEventListener

addEventListener関数はJavaScriptの基本的なイベントバインド方法で、書き方は：

```javascript
addEventListener(type, listener);
addEventListener(type, listener, options);
addEventListener(type, listener, useCapture);
```

### 引数

- `type`

  対象とするイベントの種類を表す文字列です（click、touchなど）。

- `listener`

  指定された種類のイベントが発生するときに通知（[`Event`](https://developer.mozilla.org/ja/docs/Web/API/Event) インターフェースを実装しているオブジェクト）を受け取るオブジェクト。これは `null` であるか、`handleEvent()` メソッドのあるオブジェクトか、JavaScript の関数の何れかでなければなりません。

- `options`(省略可)

  対象のイベントリスナーの特性を指定する、オプションのオブジェクトです。次のオプションが使用できます。

  + `capture`(省略可)：論理値で、この型のイベントが DOM ツリーで下に位置する `EventTarget` に配信 (dispatch) される前に、登録された `listener` に配信されることを示します。指定されていない場合は、既定で `false` になります。
  + `once` (省略可)：論理値で、 `listener` の呼び出しを一回のみのとしたいかどうかを値で指定します。 `true` を指定すると、 `listener` は一度実行された時に自動的に削除されます。指定されていない場合は、既定で `false` になります。
  + `passive`(省略可)：論理値で、 `true` ならば、 `listener` で指定された関数が [`preventDefault()`](https://developer.mozilla.org/ja/docs/Web/API/Event/preventDefault) を呼び出さないこを示します。呼び出されたリスナーが `preventDefault()` を呼び出すと、ユーザーエージェントは何もせず、コンソールに警告を出力します。指定されていない場合、既定で `false` になります。
  + `signal`(省略可)：`AbortSignal` です。指定された `AbortSignal` オブジェクトの `abort()` メソッドが呼び出された時に、リスナーが削除されます。指定されていない場合は、`AbortSignal` がリスナーに関連付けられません。

- `useCapture` (省略可)

  論理値で、この型のイベントが、DOM ツリー内の下の `EventTarget` に配信される*前*に、登録された `listener` に配信されるかどうかを示します。ツリーを上方向にバブリングしているイベントは、キャプチャを使用するように指定されたリスナーを起動しません。イベントのバブリングとキャプチャは、両方の要素がそのイベントのハンドラーを登録している場合に、別の要素内に入れ子になっている要素で発生するイベントを伝播する 2 つの方法です。イベント伝播モードは、要素がイベントを受け取る順番を決定します。指定されていない場合、 `useCapture` は既定で `false` となります。

  > 詳細な説明は [DOM Level 3 Events](https://www.w3.org/TR/DOM-Level-3-Events/#event-flow) と [JavaScript Event order](https://www.quirksmode.org/js/events_order.html#link4) を参照してください。 

### 例

```html
<div id="wrapper">
  <div id="header"></div>
  <div id="footer"></div>
</div>
```

```javascript
const wrapperEl = document.querySelector("#wrapper");
const headerEl = document.querySelector("#header");
const footerEl = document.querySelector("#footer");
function wrapperClickHandler() {
  console.log('wrapper click');
};
function headerClickHandler() {
  console.log('header click');
};
function footerClickHandler() {
  console.log('footer click');
};
wrapperEl.addEventListener('click', wrapperClickHandler); //　useCapture未指定、デフォルトはfalse
headerEl.addEventListener('click', headerClickHandler); //　useCapture未指定、デフォルトはfalse
footerEl.addEventListener('click', footerClickHandler); //　useCapture未指定、デフォルトはfalse
```

結果：

```javascript
// click header
header click
wrapper click

// click footer
footer click
wrapper click
```

useCaptureをtrueにしますと、結果は：

```javascript
// click header
wrapper click
header click

// click footer
wrapper click
footer click
```

上記で、出力順番は逆となっています。



## vueの例

vueはデフォルトで**バブリング**段階で事件を実行しているので、順番は　子->親

```html
<div @click="wrapperClick">
  <div class="header" @click="headerClick"></div>
  <div class="footer" @click="footerClick"></div>
</div>
```

```javascript
const headerClick = () => {
  console.log('header click');
};

const footerClick = () => {
  console.log('footer click');
};

const wrapperClick = () => {
  console.log('wrapper click');
};
```

結果：

```javascript
// click header
header click
wrapper click

// click footer
footer click
wrapper click
```

### バブルの阻止

バブルしてほしくない時(子要素のイベントが終わり、親要素のイベントがいらない時)、`.stop`か`stopPropagation`が使えます。

#### .stop

```html
<div @click="wrapperClick">
  <div class="header" @click.stop="headerClick"></div>
  <div class="footer" @click.stop="footerClick"></div>
</div>
```

#### stopPropagation

```javascript
const headerClick = (event) => {
  console.log('header click');
  event.stopPropagation();
};

const footerClick = () => {
  console.log('footer click');
  event.stopPropagation();
};

const wrapperClick = () => {
  console.log('wrapper click');
};
```

上記の一つで変えたら、出力は下記になります：

```javascript
// click header
header click

// click footer
footer click
```

