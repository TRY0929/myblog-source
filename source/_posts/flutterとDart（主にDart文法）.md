---
title: flutterとDart（主にDart文法）
abbrlink: f8ee9575
date: 2024-03-21 15:31:01
tags:
category: dart
description: flutterの簡単紹介とDart文法のノート記録です〜
---

最近flutterの勉強を始めたので、これからノートで自分で開発する時であった問題や習ったポイントなどを記録していこうと思ってます。

## １、flutterは何できますか

flutterはDart言語で開発したアプリを作成できるフレームワークです。一回の開発でIOS、Androidなどに変換し、さまざまなプラットフォームで実行できるアプリを開発したい人にはおすすめです。

> [flutter](https://flutter.dev/)



## ２、Dart言語

新しい言語勉強するのはなかなか大変ですが、Dartの文法をよく見たら、とてもTypescriptに似ていて、オブジェクト指向のところはJavaによく似ていることがわかりますので、実はそこまで難しくないです。

### Dart VS JavaScript

JavaScriptの'**弱い型付け（動的型付け）**'は常に批判されており、そのために TypeScript（JavaScript言語のスーパーセットであり、JavaScriptとの構文互換性がありますが、'型'が追加されています）が市場に存在しています。

しかし、すべてには2つの面があります。JavaScriptの強力な動的な特性もまた、二重の刃です。

このような動的性質がJavaScriptにはあまりにも悪いと考える声を頻繁に聞くことができ、あまりにも柔軟すぎてコードが予測しにくく、予期しない変更を制限することができません。

結局、いくつかの人々は自分自身または他人が書いたコードに常に不安を感じており、コードを制御可能にし、エラーを減らすのに役立つ静的型チェックシステムがあることを期待しています。

このため、Flutterでは、**Dartはほぼスクリプト言語の動的な特性を放棄しました**。

リフレクションをサポートしない、また関数の動的な作成もサポートしません。また、Dartは2.0から型チェック（Strong Mode）を強制的に有効にしました。以前のチェックモードとオプションの型は徐々に使用されなくなります。

そのため、型の安全性の観点から言えば、DartとTypeScriptはほぼ同じです。したがって、動的性だけで見ると、Dartには明確な利点はありませんが、総合的に見ると、Dartはサーバーサイドスクリプト、アプリ開発、Web開発の両方に適しているため、優位性があります！



### Dart VS Java

客観的に言えば、構文レベルでは、**Dartは確かにJavaよりも表現力があります**。

VMレベルでは、**Dart VMはメモリ回収とスループットの両方に反復的に最適化されています**が、具体的な性能比較に関するテストデータは見当たりませんでした。

Dart言語が普及すれば、VMの性能を心配する必要はないと思います。なぜなら、GoogleはGo、JavaScript（v8）、Dalvik（Android上のJava VM）などで多くの技術を蓄積しているからです。

注目すべきは、Flutterの中でDartはすでにGC（ガベージコレクション）を10ms未満にすることができるということです。したがって、DartとJavaを比較すると、勝敗の要因は性能面ではなくなります。

そして、構文レベルでは、DartはJavaよりも表現力があり、最も重要なのは、Dartが関数型プログラミングをサポートする点がJavaよりも優れていることです（現在はラムダ式にとどまっています）。



## ３、Dart文法

今まではJavaScriptを使ってきたので、主にDartとJSの違いを考えてDartの学習を進めていきたいと思います。

### ３.１、変数

#### `var`キーワード

Dartの`var`キーワードはJSと似ていて、全種類の変数を作れますが、Dartの`var`変数は一旦値が与えられたら、二度と型を変えることはできません。

下記のコードはJavaScriptでは通常に動けますが、Dartにはエラーが出ます：

```dart
var t = "hi world";
t = 1000;
```

#### `dynamic` と `Object`

Dartの中、全ては`Object`を基づいて作られています。（JavaScriptと同じく）

全ての型は`Object`に継承しているので、`Object`で作られた変数は、違う型の値でも与えられます。

それと似ている`dynamic`も、違う型の値でも与えられます。

```dart
dynamic t;
Object x;
t = "hi world";
x = 'Hello Object';
// 下記のコードは問題ないです
t = 1000;
x = 1000;
```

`dynamic` と `Object`に最大な違いは、 `Object`で作成された変数に、 `Object`のメソッドしか使えないが、`dynamic`変数は全て可能なメソッド使えます。

例：

```dart
 dynamic a;
 Object b = "";
 main() {
   a = "";
   printLengths();
 }   

 printLengths() {
   // 問題ない
   print(a.length);
   // エラー： The getter 'length' is not defined for the class 'Object'
   print(b.length);
 }
```

注意点は、コンパイル時に`dynamic`変数はエラー出ないが、実行時存在しないメソッドを呼び出す時は出ます。

#### `final`和`const`

変数を一度も変更する予定がない場合は、varではなく、finalまたはconstを使用してください。

+ const変数がコンパイル時定数であることです（コンパイル時に定数値に直接置換されます）
+ final変数は最初に使用されるときに初期化されます。

finalまたはconstで修飾された変数の場合、変数の型は省略できます。

#### null-safety

Dartの中、全てはObjectなので、ある数字を定義してたら、値なしで使おうと思っても実際に実行する前に、エラーにはなりません。

実行したらエラーは出ます。

```dart
test() {
  int i; 
  print(i*8);
}
```

そういう危ないことを避けるため、定義するまたは使う時下記のようにした方がおすすめです。

```dart
int i = 8; // 通常定義はnullで使えない、初期化します。
int? j; // nullでもいい場合、?をつけて定義します、使う前にnull判定を行わないとエラーが出ます。

// nullはだめで、初期値がわからない場合、lateで定義しても良いですが、使う前は絶対初期化します。
late int k;
k=9;
```

#### 非同期処理（Future,async/await）

FutureはJavaScriptのPromiseと非常に似ており、非同期操作の最終的な完了（または失敗）およびその結果値を表します。

簡単に言えば、Futureは非同期操作を処理するために使用され、非同期処理が成功した場合は成功した操作が実行され、非同期処理が失敗した場合はエラーをキャプチャしたり後続の操作を停止したりします。

1つのFutureは1つの結果に対応し、成功するか失敗します。

すべてのFutureのAPIの戻り値は依然としてFutureオブジェクトであることに注意してください。そのため、Futureはチェーン式の呼び出しが非常に便利です。

1）`Future.then`

下記のコードは２秒後プリントします。

```dart
Future.delayed(Duration(seconds: 2),(){
   return "hi world!";
}).then((data){
   print(data);
});
```

2）`Future.catchError`

非同期処理にエラーが発生した場合、`then()`ではなく、`catchError()`函数が呼び出されます。

```dart
Future.delayed(Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");  
}).then((data){
   // 成功した場合
   print("success");
}).catchError((e){
   //　失敗した場合  
   print(e);
});
```

`catchError()`だけではなく、`then()`の中に`onError()`函数も定義できます。

```dart
Future.delayed(Duration(seconds: 2), () {
	//return "hi world!";
	throw AssertionError("Error");
}).then((data) {
	print("success");
}, onError: (e) {
	print(e);
});
```

3）`Future.whenComplete`

時々、非同期タスクが**成功または失敗に関係なく何かしらの処理を行う**必要がある場面に遭遇することがあります。

例えば、ネットワークリクエストの前にローディングダイアログを表示し、リクエストが終了した後にダイアログを閉じるなどです。

このような場面では、2つの方法があります。

1つ目は、thenまたはcatch内でそれぞれダイアログを閉じる方法であり、2つ目はFutureのwhenCompleteコールバックを使用する方法です。

```dart
Future.delayed(Duration(seconds: 2),(){
   //return "hi world!";
   throw AssertionError("Error");
}).then((data){
   // 成功した場合
   print(data);
}).catchError((e){
   // 失敗した場合
   print(e);
}).whenComplete((){
   // 処理が完了し、成功＆失敗した場合
});
```

4）`Future.wait`

時々、**複数の非同期タスクがすべて完了する**のを待ってから特定の操作を行う必要があります。

たとえば、2つの異なるネットワークインターフェイスからデータを取得する必要がある場合、それらのデータを特定の処理を行った後にUIに表示する必要があるかもしれません。

この場合、Future.waitを使用します。

これはFutureの配列を受け取り、配列内のすべてのFutureが成功した場合にthenの成功コールバックがトリガーされます。

1つのFutureでも失敗した場合はエラーコールバックがトリガーされます。

以下に、Future.delayedを模擬して2つのデータ取得の非同期タスクを模擬し、2つのタスクが両方成功した場合に結果を連結して出力する例を示します。

下記のコードは４秒後にプリントします。

```dart
Future.wait([
  // 2秒後 
  Future.delayed(Duration(seconds: 2), () {
    return "hello";
  }),
  // 4秒後
  Future.delayed(Duration(seconds: 4), () {
    return " world";
  })
]).then((results){
  print(results[0]+results[1]);
}).catchError((e){
  print(e);
});
```

Dartの`async`/`await`とJSのはほぼ一緒で、こちらでは紹介しないです。



## ４、まとめ

Dartの学習はそこまで難しくなく、今までにウェブ開発の経験があり、さらにスマホアプリをflutterで作りたい人は簡単に（そこまで簡単ではないけど）始められます！

言語の違いと、アプリ開発の注意点を考えながら勉強を進めましょう〜



> 参考：https://book.flutterchina.club/
