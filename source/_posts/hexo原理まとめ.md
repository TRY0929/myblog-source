---
title: hexo原理まとめ
abbrlink: 44223
date: 2024-02-29 11:27:01
tags:
category: hexo
---
> [hexoオフィシャルサイト](https://hexo.io/docs/index.html)
>
> ## What is Hexo?
>
> Hexo is a fast, simple and powerful blog framework. You write posts in [Markdown](http://daringfireball.net/projects/markdown/) (or other markup languages) and Hexo generates static files with a beautiful theme in seconds.

簡単に言えば、hexoはブログを作成するフレームワークであり、makedownで文章をレンダーし、画面に表示させされます。

## 1. hero generate：静的なファイルの生成

上記の指令はデータファイルと画面ファイルを合わせ、静的なファイルを生成できます。

まずテーマフォルダーのsourceフォルダー（js、css、imgなどのファイル）を読み、インデックスを建てます。それを基づいてpublicフォルダーのファイルを生成します。

生成したpublicフォルダーの中にはhtml、js、css、imgなどの静的なファイルがあり、index.htmlをエンターファイルとしてサイトにはいれます。



## 2. hero deploy：プロジェクトのデプロイ

_config.ymlファイルの中に配置しているgitレポジトリー及びcodingのurlで、publicフォルダーの内容をgithubかcodingにアップロードします。（githubにpagesというサービスがあり、それで画面を表示できます）

もちろん、生成したpublicフォルダーを自分のサーバーにデプロイしても大丈夫です。



## 3. hexoのテンプレートエンジン

> テンプレートエンジンとは、画面とデータを分離させることです。
>
> 画面の動的な部分を抽出し、データとして他のところで管理することで、画面とデータの分離ができます。

source＋themes -> public

sourceフォルダーがデータベース、themesフォルダーが画面表示として考えられます。

それぞれを作成し、hexo generateを実行し、二つの内容をpublicとして出力します。

テンプレートファイルはthemes/layoutの下に配置されています。例：

```
.
├── _custom                           // 通常レイアウト
├── _layout.swig                      // デフォルトレイアウト
├── _macro                            // プラグインレイアウト
├── _partials                         // パートレイアウト
├── _scripts                          // スクリプトレイアウト
├── _third-party                      // 
├── archive.swig                      // アーカイブレイアウト
├── category.swig                     // 分類レイアウト
├── index.swig                        // インデックスレイアウト
├── page.swig                         // 他のレイアウト
├── photo.swig                        // 写真レイアウト
├── post.swig                         // 文章レイアウト
├── schedule.swig                     // scheduleレイアウト
└── tag.swig                          // タッグレイアウト
```

> 全てのページがレイアウトを指定していない時、デフォルトレイアウト_layout.swigを使用します。ページな中でレイアウトの使用状況を定義ですます（使うかどうか）。

新しくポスト及びページを作成する時にレイアウトも指定できます。`hexo new [layout] <title>`

下記はデフォルトレイアウト_layout.swigを例として説明します。

このレイアウトにも他のレイアウトをインポートし、一部の表示として使われています。

```html
{% extends '_layout.swig' %}
{% import '_macro/post.swig' as post_template %}
{% import '_macro/sidebar.swig' as sidebar_template %}


{% block title %} {{ page.title }} | {{ config.title }} {% endblock %}

{% block page_class %}page-post-detail{% endblock %}


{% block content %}

  <div id="posts" class="posts-expand">
    {{ post_template.render(page) }}

    <div class="post-spread">
      {% if theme.jiathis %}
        {% include '_partials/share/jiathis.swig' %}
      {% elseif theme.baidushare %}
        {% include '_partials/share/baidushare.swig' %}
      {% elseif theme.add_this_id %}
        {% include '_partials/share/add-this.swig' %}
      {% elseif theme.duoshuo_shortname and theme.duoshuo_share %}
        {% include '_partials/share/duoshuo_share.swig' %}
      {% endif %}
    </div>
  </div>

{% endblock %}

{% block sidebar %}
  {{ sidebar_template.render(true) }}
{% endblock %}


{% block script_extra %}
  {% include '_scripts/pages/post-details.swig' %}
{% endblock %}
```

テンプレートの中で使われているデータは`hexo generate`を実行する時に入れられています。

## 4. コンフィグファイルデータの使用

hexoのコンフィグファイル`_config.yml`はyml文法を使い、ブログのタイトルやサブタイトルなどを定義できます。

データのタイプには、数字、文字列、オブジェクト、配列が使えます。

### テンプレートファイルでコンフィグファイルのデータを使用する例

```
{% block title %} {{ page.title }} | {{ config.title }} {% endblock %}
```



## 5. hexoの既存変数

hexoには既にいろんな変数が定義されていて、よく使われている変数は下記となります。

`site`：サイト全体のデータオブジェクト， `site.posts.length` がよくページングで使われています。

- `site.posts` ：全てのポスト
- `site.pages`：全てのページ
- `site.categories` ：全ての分類
- `site.tags` ：全てのタッグ

`page`：今画面にある文章のインフォメーション， `page.posts`は今のもので、  `site.posts`は全ての文章となります。

`config`：`config` 変数**プロジェクト直下**配置しているで`_config.yml`ファイルの中に定義されているデータが取れます。

`theme`：`theme` 変数で**テーマフォルダー**の下に配置している `_config.yml` ファイルの中に定義されているデータが取れます。

`path`：今のパス（ルートパスは含まれていない）。

`url`：ページの前URL。



## ６.まとめ

一言なら、hexoは、我々がmakedownで書いてる内容とテーマテンプレートと合わせて、HTMLファイルなどを自動的に作成するツール（フレームワーク）です。

初心者やゼロからブログサイトを作る方にとってはとても便利で効率高いツールです。

