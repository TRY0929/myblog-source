---
title: ブロック整形コンテキスト（BFC）
category: css
abbrlink: b5806a76
date: 2024-03-07 10:18:49
tags:
---

## １、基本概念

> **ブロック整形コンテキスト** (block formatting context, BFC) は、ウェブページにおける CSS の視覚的なレンダリングの一部です。ブロックボックスのレイアウトが行われ、浮動要素が他の要素と相互作用する領域です。

整形コンテキストはレイアウトに影響を与えますが、通常はレイアウトを変更するのではなく、位置決めと浮動要素の解消のために新しいブロック整形コンテキストを作成します。これは、新しいブロック整形コンテキストを確立する要素が次のようになるからです。

BFCは下記の特徴があります：

1. BFC要素の中、ボックスは上から下まで垂直で並んでます；
2. ボックスの垂直距離はマージンに決められ、同じBFCに含まれている二つの接するボックスはマージン相殺が発生します；
3. BFC要素の中、全てのボックスの左側(margin-left)は容器の左側(border-left)と接する：
   + BFCは浮動の要素と被らない；
   + BFC要素の高さを計算する時、通常フローにいる要素と浮動要素どちらでも計算に入れます。



## ２、作成方法

ブロック整形コンテキストは、以下のうちの少なくとも一つから生成されます。

- 文書のルート要素 (`<html>`)
- 浮動要素 ([`float`](https://developer.mozilla.org/ja/docs/Web/CSS/float) が `none` 以外である要素)
- 絶対位置指定の要素 ([`position`](https://developer.mozilla.org/ja/docs/Web/CSS/position) が `absolute` または `fixed` である要素)
- インラインブロック ([`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: inline-block` である要素)
- 表のセル ([`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: table-cell` を持つ要素。これは HTML の表のセルの既定値です)
- 表のキャプション ([`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: table-caption` を持つ要素。HTMLの、表のキャプションの既定値です)
- [`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: table`, `table-row`, `table-row-group`, `table-header-group`, `table-footer-group` (つまりそれぞれ HTML の表、表の行、表の本体、表のヘッダー、表のフッターの既定値), `inline-table` のついた要素によって暗黙的に生成された無名の表のセル。
- [`overflow`](https://developer.mozilla.org/ja/docs/Web/CSS/overflow) の値が `visible` 以外であるブロック要素
- [`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: flow-root`
- [`contain`](https://developer.mozilla.org/ja/docs/Web/CSS/contain)`: layout`, `content`, `paint` の付いた要素
- フレックスアイテム ([`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: flex` または `inline-flex` である要素の直接の子要素)、[フレックス](https://developer.mozilla.org/ja/docs/Glossary/Flex_Container)でも[グリッド](https://developer.mozilla.org/ja/docs/Glossary/Grid_Container)でも[表](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_table)でもない場合
- グリッドアイテム ([`display`](https://developer.mozilla.org/ja/docs/Web/CSS/display)`: grid` または `inline-grid` である要素の直接の子要素)、[フレックス](https://developer.mozilla.org/ja/docs/Glossary/Flex_Container)でも[グリッド](https://developer.mozilla.org/ja/docs/Glossary/Grid_Container)でも[表](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_table)でもない場合
- 段組みコンテナー ([`column-count`](https://developer.mozilla.org/ja/docs/Web/CSS/column-count) または [`column-width`](https://developer.mozilla.org/ja/docs/Web/CSS/column-width) が `auto` ではない要素、 `column-count: 1` の要素も含む)
- [`column-span`](https://developer.mozilla.org/ja/docs/Web/CSS/column-span)`: all` は、 `column-span: all` の要素が段組みコンテナーに含まれていなくても、常に新たな整形コンテキストを生成します。

## ３、目的（作用）

BFCはいつも下記のために作成されます：

- 内部の浮動要素を収めます。
- 外部の浮動要素を追いやります。
- [マージンの相殺](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing)を抑止します。

### ３.１、内部の浮動要素を収めます

親容器はBFCになれば、内部の浮動要素のコンテンツの高さも親容器に計算されます。

以下の例では、`border`が適用された `<div>` の中に浮動要素があります。その `<div>` のコンテンツは浮動要素の横に並んだ状態になっています。浮動要素のコンテンツは横に並んだコンテンツよりも高さがあるため、`<div>` の境界線が浮動要素を貫通してしまいます。(浮動要素がフローから外れたので)

親容器`div`がBFCになったら、内部の浮動要素も高さの計算に入り、親容器の高さは変わります。

```html
<section>
  <div class="box">
    <div class="float">浮動ボックスです。</div>
    <p>コンテナー内のコンテンツです。</p>
  </div>
</section>
<section>
  <div class="box" style="overflow:auto">
    <div class="float">浮動ボックスです。</div>
    <p><code>overflow:auto</code> のコンテナー内のコンテンツです。</p>
  </div>
</section>
<section>
  <div class="box" style="display:flow-root">
    <div class="float">浮動ボックスです。</div>
    <p><code>display:flow-root</code> のコンテナー内のコンテンツです。</p>
  </div>
</section>
```

```css
section {
  height: 150px;
}
.box {
  background-color: rgb(224 206 247);
  border: 5px solid rebeccapurple;
}
.box[style] {
  background-color: aliceblue;
  border: 5px solid steelblue;
}
.float {
  float: left;
  width: 200px;
  height: 100px;
  background-color: rgb(255 255 255 / 50%);
  border: 1px solid black;
  padding: 10px;
}
```

![image-20240306140213733](https://cdn.jsdelivr.net/gh/try0929/cdn/img/b5806a76_1.png)



### ３.２、外部の浮動要素を除外する

通常フローの要素は、要素以外の浮動要素と重なる確率があります。そのため、通常フローの要素をBFCにしたら、要素以外の要素と重なることはなくなります。

```html
<section>
  <div class="float">外部の浮動要素</div>
  <div class="box"><p>通常</p></div>
</section>
<section>
  <div class="float">外部の浮動要素</div>
  <div class="box" style="display:flow-root">
    <p><code>display:flow-root</code></p>
  </div>
</section>
```

```css
section {
  height: 150px;
}
.box {
  background-color: rgb(224 206 247);
  border: 5px solid rebeccapurple;
}
.box[style] {
  background-color: aliceblue;
  border: 5px solid steelblue;
}
.float {
  float: left;
  overflow: hidden; /* required by resize:both */
  resize: both;
  margin-right: 25px;
  width: 200px;
  height: 100px;
  background-color: rgb(255 255 255 / 75%);
  border: 1px solid black;
  padding: 10px;
}
```

![image-20240306140213733](https://cdn.jsdelivr.net/gh/try0929/cdn/img/b5806a76_2.png)



### ３.３、マージンの相殺を抑止する

通常フローの中にある二つのブロック要素があり、それぞれの垂直マージンは `10px` です。マージンが相殺されるため、両要素間の垂直方向のギャップは 10 ピクセルとなり、期待される 20 ピクセルにはなりません。

一つの要素をBFCに変えたら、新しい BFC を作成し、マージンの相殺を防いでいます。

```html
<div class="blue"></div>
<div class="outer">
  <div class="red"></div>
</div>
```

```css
.blue,
.red {
  height: 50px;
  margin: 10px 0;
}

.blue {
  background: blue;
}

.red {
  background: red;
}

.outer {
  overflow: hidden;
  background: transparent;
}
```

![image-20240306140213733](https://cdn.jsdelivr.net/gh/try0929/cdn/img/b5806a76_3.png)





> 参考　[Block_formatting_context](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_display/Block_formatting_context)
