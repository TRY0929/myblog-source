---
title: イベントループ
abbrlink: a8dc2f28
date: 2024-03-08 11:11:13
tags:
category: Nodejs
description: JavaScriptは一つのプロセスしかない、なんで？
---

## １、JavaScriptは単一プロセス

JavaScriptは一つのプロセスしかない、なんで？

想像してみて：二つのプロセスが同時に一つのDOM要素を操作しようとしている、削除か変更か、順番が違ったら全く違う結果が出るんじゃないですか。

```
one thread == one call stack == one thing at a time
```

単一プロセスのため、うまく使えないならブロックは起こりやすい。

<! --more-->

## ２、同期と非同期処理

JavaScriptの操作は同期処理と非同期処理に分かれていて、同期処理はプロセススタックで順次呼び出され、実行されますが、非同期処理は最初そこに入ってない。

IO操作やWebAPIなどの時間のかかる操作は、メインプロセスをブロックさせないように、いつも非同期で操作され、コールバック関数で処理します。

+ 同期処理：メインプロセスで順次実行される
+ 非同期処理：メインプロセスに入らず、タスクキューに入り、自身のある操作が完了した時、メインプロセスにお知らせをして、メインプロセスに呼び出される

下記コードの出力順番は簡単に言えますが、原理部分を詳しく説明したらJavaScriptの原理に理解できると思います。

例：

```javascript
console.log(1);

setTimeout(function cb() {
  console.log(2);
}, 100);

console.log(3);

// 結果
1
3
2
```

実行の流れは大体：

1. `console.log(1);`と`console.log(1);`は同じくメインプロセスに入っており、順次に処理されます。

2. メソッド`cb`の`console.log(2)`は、タスクキューに入って、タイマーが終わってからメインプロセスで実行されます。

タイマーの設置は前にあるのに、なぜ`console.log(3);`は100msのタイマーが終わって、`console.log(2);`が実行してから出力してないですか？

もしそうであれば、タイマーが終わるまで`console.log(3);`はブロックされているが、現実はそうでもない。

上記の例でまとめること：

1. 全ての同期処理はメインープロセスで実行され、それはコンテキストスタック（execution context stack）となります；
2. メインプロセス以外、タスクキュー（task queue）が存在します。非同期処理が終わり、イベント（非同期処理のコールバック）がそこに入ります；
3. メインプロセスのタスクが全て終わり、タスクキューからタスクをとって実行します。
4. これからメインプロセスは３を繰り返します。

### タスクキュー(task queue)概念まとめ

+ FIFOデータストラクチャで、最初入れたタスクが最初に実行されます；
+ 非同期処理が終わったらそれのイベントをタスクキューに入れます；
+ メソッドに関数を渡した時、 メソッドに紐づいたイベントが発生していた場合に、この関数を*コールバック*するのです。



## ３、イベントループ

メインプロセスが実行される時、ヒープ(heap)とスタック(stack)が生成されます。

+ heap：データを置くところ
+ stack：メインプロセスで実行する函数列

スタックの函数は他のWebAPIを呼び出し、非同期処理がある場合、イベントがタスクキューに入ります。

スタックのコードが全て実行され、タスクキューの中からタスクを取って実行する（コールバックする）。

![Event Loop](https://www.ruanyifeng.com/blogimg/asset/2014/bg2014100802.png)



## 4、マイクロ(micro)とマクロ(macro)タスク

ブラウザの非同期タスクはマイクロとマクロタスクに分かれています。

**マクロタスク**：

- setTimeout
- setInterval
- setImmediate (Node Jsだけある)
- requestAnimationFrame (ブラウザだけある)
- I/O
- UI rendering (ブラウザだけある)

**マイクロタスク**：

- process.nextTick (Node Jsだけある)
- Promise
- Object.observe
- MutationObserver

基本の流れはさっき説明した通り、**同期タスクが終わってから非同期タスク**を処理する。

ただ、**マクロとマイクロタスクにも順番があります**（同期処理が全て終わってる状態）：

ブラウザ上：

1. マイクロタスクがある限り、マクロタスクは処理されない；
2. 一つマクロタスクの処理が終わって、すぐマイクロタスクのキューをチェックし、あるなら処理する。

Nodejs環境（同期処理が全て終わり）：

1. process.nextTickを実行する；
2. 全てのマクロタスクを処理する；
3. 全てのマイクロタスクを処理する。



理解できましたかな？簡単（ではない）なテストで試してみよう〜

下記のコードを実行したら、出力の順番をを教えてください。

```javascript
console.log(1);

setTimeout(() => {
  console.log(2);
  Promise.resolve().then(() => {
    console.log(3)
  });
});

new Promise((resolve, reject) => {
  console.log(4)
  resolve(5)
}).then((data) => {
  console.log(data);
  
  Promise.resolve().then(() => {
    console.log(6)
  }).then(() => {
    console.log(7)
    
    setTimeout(() => {
      console.log(8)
    }, 0);
  });
})

setTimeout(() => {
   console.log(9)
}, 0);
console.log(10)
```

正解：

```
1 4 10 5 6 7 2 3 9 8
```



## ５、Nodejs

NodejsはJavaScriptのサーバーサイドの実行環境でありますが、単一のスレッドでしか実行していません。それはブラウザ上のJSと同じです。

下記の図はNodejsの取り組みとなります：

1. V8エンジンがJavaScriptスクリプトを解析します。
2. 解析されたコードは、Node APIを呼び出します。
3. libuvライブラリがNode APIの実行を担当します。それは異なるタスクを異なるスレッドに割り当て、イベントループを形成し、タスクの実行結果を非同期にV8エンジンに返します。
4. V8エンジンは結果をユーザーに返します。

![Node.js](https://www.ruanyifeng.com/blogimg/asset/2014/bg2014100803.png)

### イベントループの各段階（phases of the event loop）

Nodejsでコードを実行する時、下記の流れがあります。

+ `timers`：`setTimeout`と`setInterval`時間切れのタイマーのコールバックメソッドを実行する。
+ `pending callbacks`：設定されたタイマー以外のコールバックメソッドを実行する（EX.HTTPリクエストの異常中止）。
+ `Idle, prepare`：Nodejs内部処理。
+ `poll`：非同期タスク（IO、HTTPリクエスト）を処理して、コールバックメソッドを実行する。
  + 非同期処理（タイマー以外）が終わってない限り、ずっとこの段階でポーリングする。
+ `check`：`setImmediate`のコールバックメソッドを実行する。
+ `close callbacks`：`socket.close()`とかのコールバックメソッドを実行する。

```
   ┌───────────────────────────┐
┌─>│           timers          │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │     pending callbacks     │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
│  │       idle, prepare       │
│  └─────────────┬─────────────┘      ┌───────────────┐
│  ┌─────────────┴─────────────┐      │   incoming:   │
│  │           poll            │<─────┤  connections, │
│  └─────────────┬─────────────┘      │   data, etc.  │
│  ┌─────────────┴─────────────┐      └───────────────┘
│  │           check           │
│  └─────────────┬─────────────┘
│  ┌─────────────┴─────────────┐
└──┤      close callbacks      │
   └───────────────────────────┘
```

### `setImmediate()` vs `setTimeout()`

- `setImmediate()` はいつも現在の`poll`段階で処理される
- `setTimeout()` は設定の時間の後にプロセスに実行される

コールバックすぐ実行してほしい時はどちらでも使えますが、違う場合で使うと全く異なる結果が出るかもしれません。

何もないメインプロセスの中に二つのタイマーを設置すると：

```javascript
setTimeout(() => {
  console.log('timeout');
}, 0);
setImmediate(() => {
  console.log('immediate');
});
```

結果は時間によって違います。今回は先に`timeout`が出力されましたが、次回は`immediate`かもしれません。完全にシステムのプロセス処理に任せてるので、予測はできません。

けど、I/O処理（非同期処理）の中で同時に二つのタイマーを設置すると、必ず`setImmediate()`の方が先に処理されます。

理由：I/O処理は`poll`段階でするので、処理が終わったら`poll`段階も終わり、`setImmediate()`はすぐ実行されるが、`setTimeout()`は次回の`timers`段階で実行されます。

```javascript
const fs = require('node:fs');
fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout');
  }, 0);
  setImmediate(() => {
    console.log('immediate');
  });
});
```

**結論**

もしIO処理など非同期処理のあるコードの中、その後すぐ実行したいコードがあれば、`setImmediate()`がおすすめです、それは必ず次の`timers`段階に入る前に処理されるためです。

### `process.nextTick()`

`process.nextTick()`はNodejsのイベントループのどの段階でも表れていないが、非同期処理となっています。じゃそれはどのタイミングで実行しますか？

答えは、特定の段階ではなく、任意一つの段階が終わってからすぐ実行します。だから、次の段階に行ける条件の一つは、`process.nextTick()`のコールバックが順調に終わってます。

そのため、うまく書いてない時、Nodejsのプロセスは`process.nextTick()`にブロックされる恐れがあります。

**いつ使いますか**

+ ユーザにエラーの処理やいらない資源のクリーンを行ってほしい時か、イベントループが続く前にもう一度リクエストを試すか。
+ メインスタックのタスクが終わってから、イベントループが続く前に何かしたい時。



> 参考：[The Node.js Event Loop](https://nodejs.org/en/learn/asynchronous-work/event-loop-timers-and-nexttick)
