---
title: hexoでクリックスタイルの変え方
abbrlink: 3bca534c
date: 2024-03-01 15:02:07
tags:
category: hexo
---

マウスでクリックしたら、花火やハートが出てくる動画が実現できるサイトってかっこいいですね〜

と思いながら、自分のサイトでもそういう機能がほしくて作りました。

## 1. クリックスタイルファイルを用意します

今回私が入れたいのはクリックしたら、花火みたいな点が出てくるものなので、まずそれのJSでの実装ファイルを用意します。

実装ファイルは他の方の真似して書いてました。ファイルはローカルか、CDNに置くかどちらでも良いです。

+ Githubに置いて、CDNのURLでインポートします；
+ ローカルに置いて、相対パスでインポートします。

## 2. Layoutで新しいテンプレートファイルを作成します

レイアウトにはいろんなテンプレートがあり、その中に新しいswigでクリックスタイルのjsスクリプトをインポートします。

例：

+ `themes/next/layout/_custom/cursor.swig` ファイルを新規作成します；
+ 中で下記のコードでスクリプトをインポートします：

```
// CDNのURLを使用する場合
{%- if theme.mouse_animation %}
  {%- set mouse_animation_css_url = next_vendors('//cdn.jsdelivr.net/gh/TRHX/CDN-for-itrhx.com@3.0.8/js/maodian.js') %}
  <script src="{{ mouse_animation_css_url }}"></script>
{%- endif %}

//　ローカルファイルを使用する場合（スクリプトファイルは/source/js/cursor.jsに置いてます）
{%- if theme.mouse_animation %}
  {%- set mouse_animation_css_url = next_js('/cursor.js') %}
  <script src="{{ mouse_animation_css_url }}"></script>
{%- endif %}
```

+ `theme.mouse_animation`はテーマコンフィグファイルの中で定義されている変数で、そこで`true`を設定したらスクリプトファイルがインポートされます。

## 3. デフォルトレイアウトで新しいテンプレートをインポートします

さっき新しいテンプレートを作成しましたが、まだ使われていないです。

クリックスタイルはサイトのどこでもあるので、**全ての画面が必ず使うデフォルトテンプレートファイル**`_layout.swig`の中で新しいテンプレートファイルをインポートします。

`_layout.swig`の中で下記のコードを追加します：

```
{%- include '_custom/cursor.swig' %}
```

ちなみに、スクリプトファイルの中で`document.body`を使っているため、`body`タッグの中でインクルードした方がおすすめです。
