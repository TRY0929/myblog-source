---
title: Nodejsでサーバー＆BFF
abbrlink: 1a9c3610
date: 2024-03-07 22:37:18
tags:
category: Nodejs
published: false
---

## １、Nodejsとは？

> Node.js runs the V8 JavaScript engine, the core of Google Chrome, outside of the browser. This allows Node.js to be very performant.
>
> A Node.js app runs in a single process, without creating a new thread for every request. Node.js provides a set of asynchronous I/O primitives in its standard library that prevent JavaScript code from blocking and generally, libraries in Node.js are written using non-blocking paradigms, making blocking behavior the exception rather than the norm.

**ブラウザ以外でも実行できるJavaScriptの実行環境**：現在フロントエンドで使われているJavaScriptはブラウザでしか実行できないが、Node.jsは、Google ChromeのコアであるV8 JavaScriptエンジンを**ブラウザの外で実行します**。

+ python.exeがPythonコードを実行するアプリケーションであるように、node.exeはJavaScriptコードを実行するアプリケーション＝JavaScript実行環境です。

**非同期(ex.I/O)イベント駆動のサーバーサイドJavaScript環境**：Node.jsアプリは、リクエストごとに新しいスレッドを作成せずに**単一のプロセス**で実行されます。Node.jsは、標準ライブラリで非同期I/Oプリミティブのセットを提供しており、JavaScriptコードがブロックされるのを防ぎます。

## ２、Nodejsは何できる？

上記の文章の中で、NodejsはサーバーサイドJavaScript環境だと書いてあり、JavaやGoなどの言語みたいにバックエンドの仕組みも取り込めるはずです。

実際もその通りです。

### ２.１、バックエンド（ウェブサーバー）

Node.jsは**大量の同時接続をさばけるネットワークアプリケーションの構築**を目的に設計されたJavaScript環境です。

