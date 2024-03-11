---
title: メディアを使用してレイアウトを自動調整する
abbrlink: 7de6e31e
date: 2024-03-11 14:36:22
tags:
description: 同じWebページがスマホとパソコンでも適切で表示されますように、@mediaを使用してCSSの調整を行うことがおすすめです。
category: css
---

## １、@mediaとは

> `@media`は CSSのアットルールで、1 つまたは複数のメディアクエリーの結果に基づいて、スタイルシートの一部を適用するために使用することができます。これによってメディアクエリーを指定し、そのメディアクエリーがコンテンツの使用される端末に一致する場合にのみ、文書に CSS のブロックを適用することができます。

同じWebページがスマホとパソコンでも適切で表示されますように、@mediaを使用してCSSの調整を行うことがおすすめです。



## ２、メディアクエリー構文

特徴：

+ メディアクエリーは、任意のメディア種別と任意の数のメディア特性の式で構成されます。

+ 論理演算子を使用して、複数のクエリーを様々な形で組み合わせることができます。

+ メディアクエリーは大文字小文字の区別がありません。

mediaの対象は２種類あります：

1. **メディア種別**；

- `all`

  すべての機器に適合します。

- `print`

  ページのある資料や、画面に印刷プレビューモードで表示されている文書向けのものです。（これらの形式に特有の整形上の問題については、[ページ付きメディア](https://developer.mozilla.org/ja/docs/Web/CSS/CSS_paged_media)を参照してください。）

- `screen`

  主に画面向けのものです。

- `speech`

  音声合成向けのものです。

2. **メディア特性**。

ユーザーエージェント、出力装置、環境の特定の特徴を記述します。 メディア特性式は、その存在や値を調べるもので、完全に任意です。それぞれのメディア特性式は、括弧で囲む必要があります。

例えば：height、device-width。



下記の例は、**画面（media種別）の広さ（media特性）が960px以下の際、下記のCSSが効きます**

```css
@media screen and (max-width: 960px){
    body{
        background: #000;
    }
}
```





## ２、使い方

### 1. Metaタグを用意します

下記の`meta`タグをHTMLの最初に書いたら、モバイル設備での表示が良くなり、いろんなウィンドウのサイズに適用できる画面が作られます。

```HTML
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
```

**説明**

+ `width = device-width`：画面の広さは設備の広さと一致します；
+ `height = device-height`：画面の高さは設備の高さと一致します；
+ `initial-scale`：初期のムーズ割合、デフォルトは1.0（おすすめ）；
+ `minimum-scale`：ユーザがムーズできる最小値、デフォルトは1.0（おすすめ）；
+ `maximum-scale`：ユーザがムーズできる最大値、デフォルトは1.0（おすすめ）；
+ `user-scalable`：ユーザが手でムーズできるかどうか、デフォルトは`no`（）おすすめ。

### ２、@mediaを書きます

例：

```css
@media (max-width: 960px){
    body{
        background: #000;
    }
}
```



> 参考　[@media](https://developer.mozilla.org/en-US/docs/Web/CSS/@media)

