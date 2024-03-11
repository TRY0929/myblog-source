---
title: jsdelivrの使用
abbrlink: 36779
date: 2024-02-29 17:55:48
category: cdn
---
jsdelivrは無料でgithubの静的なファイルを早くとってくれるサイトで、自分のサイトでインポートする時はとても便利です。

## 1. jsdelivrのURLフォーマット

jsdelivrでファイルをインポートする時のURLは下記のフォーマットとなります：

https://cdn.jsdelivr.net/gh/<Githubユーザ名>/<レポジトリー名>@バージョン名/ファイルのパス

バージョン名は必須ではないです、バージョン名がない時は最新の資源をインポートします。

例：

```
https://cdn.jsdelivr.net/gh/jquery/jquery@3.2.1/dist/jquery.min.js
```



## 2. 資源ファイルをgithubに置きます

githubに新しいレポジトリーを作り、静的資源を置くことがおすすめです。

例えば、cdnという名前のレポジトリーを作成し、その中には下記のフォルダーがあります。

```
.
├── css               
├── fonts               
├── img               
└── js
```

imgの下に、textCur.curという名前のファイルが置いてあります。



## 3. jsdelivrのURLでファイルをプロジェクトにインポートします

下記のパスでファイルをインポートします：

```
https://cdn.jsdelivr.net/gh/try0929/cdn/img/textCur.cur
```



## 4. キャッシュ更新

jsdelivrに置いてるファイルにはキャッシュ機能があります。

簡単に言うと、githubのファイルを更新しても、CDNファイルがすぐ更新されない可能性があるため、jsdelivrのURLでインポートしたファイルが古いファイルになるかもしれません。

解決方法は、強制的にキャッシュを更新すること：ファイルの名前を変えてもう一回PUSHします。

例えば：

+ githubレポジトリーにold.jpgがあり、old.jpgを更新してpushして、Githubのファイルが新しくなり、`https://cdn.jsdelivr.net/gh/.../old.jpg`でインポートするファイルが変わらないです；
+ 同じファイルを`new.jpg`をリネームし、pushしたら、Githubもjsdelivrに置いてるCDNファイルも更新されます。

