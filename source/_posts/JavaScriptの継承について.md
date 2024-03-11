---
title: JavaScriptの継承について
abbrlink: 70e88e60
date: 2024-03-06 14:42:36
category: JavaScript
description: 継承において、 JavaScript が持っている構成要素はただひとつ、オブジェクトだけです。
---

## １、基本概念

### １.１、JavaScriptの継承

継承において、 JavaScript が持っている構成要素はただひとつ、オブジェクトだけです。

それぞれのオブジェクトは、その**プロトタイプ**と呼ばれる別のオブジェクトへのリンクを持つプライベートプロパティを持っています。

そのプロトタイプオブジェクト自身もプロトタイプを持ち、 `null` をプロトタイプとするオブジェクトに到達するまで続きます。

定義上、 `null` はプロトタイプを持たず、この**プロトタイプチェーン**の最後のリンクとして機能します。

### １.２、JavaScriptのプロトタイプチェーン

 JavaScript のオブジェクトは、プロトタイプオブジェクトへのリンクを持っています。

あるオブジェクトのプロパティにアクセスしようとすると、オブジェクトだけでなく、オブジェクトのプロトタイプ、プロトタイプのプロトタイプへと、一致する名前のプロパティが得られるか、**プロトタイプチェーンの終端に到達するまで、プロパティの探索が行われます**。

## ２、継承方法

### ２.１、プロトタイプチェーン継承

子オブジェクトのプロトタイプを**親オブジェクトのインスタンス**に指向する：`child.prototype = new father()`。

```javascript
//親コンストラクタ
function father(name) {
	this.name = name;
	this.colors = ['red'];
}
//親にメソッドをあげる
father.prototype.sayName = function() {
	console.log(this.name);
}
///子コンストラクタ
function son(name,age) {
	this.age = age;
}
// 子オブジェクトのプロトタイプを親オブジェクトのインスタンスに指向する
son.prototype = new father();

// 継承の前に指定しますと、継承の時親のプロパティに覆われ、なくなります
son.prototype.sayAge = function() {
	console.log(this.age);
}

var tmp1 = new son('try',18);
console.log(tmp1.colors); //'red'
tmp1.sayAge();

var tmp2 = new son();
tmp2.colors.unshift('yellow'); 
console.log(tmp1.colors); //'yellow','red'
```

問題：

1. 親と子の間、プロパティはシェアできますが、**各子オブジェクトに自分のプロパティを持たせることができない：（tmp2でcolorsを変えたが、tmp1のcolorsも影響されている）
2. 他の子オブジェクトに影響を与えずに一つの子オブジェクトに引数を渡すことができない：全ての継承が`son.prototype = new father();`でしてるので、これから`son`で新しいオブジェクトを作る時は全部影響されます。



### ２.２、コンストラクタとcallを使う

プロトタイプ継承が上記二つの問題があるため、コンストラクタを使って解決できます。

**子オブジェクトのコンストラクタの中で親オブジェクトのコンストラクタメソッドを実行する**、これで子オブジェクトにも親オブジェクトの全てプロパティが貰えます。

```javascript
function father(name) {
	this.name = name;
	this.colors = ['red'];
	this.test = function() {
		console.log('test success');
	}
}
father.prototype.sayName = function() {
	console.log(this.name);
}

function son(name,age) {
	//　これだけ追加しました
	father.apply(this);
	this.age = age;
}

var tmp1 = new son('try',18);
console.log(tmp1.colors); //'red'
tmp1.sayName(); //error!
tmp1.test(); //OK

var tmp2 = new son();
tmp2.colors.unshift('yellow'); 
console.log(tmp1.colors); // 'red'
```

問題：

この方法で継承したオブジェクトは、**親オブジェクトのコンストラクタの中で定義されたプロパティしか継承できません**、上記コードの`father.prototype.sayName`で定義されたメソッドは子オブジェクトの中にはありません。

コードの再利用はできません。



### ２.３、コンビ継承

２.１と２.２はそれぞれ自分の欠点がありますが、二つの方法を合わせて使ったら問題が解消されます。

+ プロトタイプ指定でプロトタイプのプロパティ（father.prototype=...で定義された物）を継承；
+ コンストラクタで各子オブジェクト自分特有のプロパティを継承。

```javascript

function father(name) {
	this.name = name;
	this.colors = ['red'];
	this.test = function() {
		console.log('test success');
	}
}
father.prototype.sayName = function() {
	console.log(this.name);
}

function son(name,age) {
	father.apply(this); //一回目
	this.age = age;
}
son.prototype = new father(); //二回目
var tmp1 = new son('try',18);
console.log(tmp1.colors); //'red'
tmp1.sayName(); //error!
tmp1.test(); //OK

var tmp2 = new son();
tmp2.colors.unshift('yellow'); 
console.log(tmp1.colors); // 'red'
```

問題：

全ての利用に関する問題は解消されたが、この方法で**親オブジェクトのコンストラクタを2回実行している**ため、効率少し低く感じます。

### ２.４、親オブジェクトのプロポタイプをコーピして渡す

欲しい物は親オブジェクトのプロポタイプのコーピだけで、親オブジェクトのコンストラクタを二回実行する必要がなく、新たにオブジェクトを作り、プロトタイプを指定し、インスタンス化して子オブジェクトに渡すこともできます。

```javascript

function object(fa) {
	//浅いコピーを実施している
	function F() {}; //fatherのプロトタイプをゲットために作成した新しいオブジェクト
	F.prototype = fa; //fatherのプロトタイプをゲット
	return new F();
}
function inheritPrototype(son, father) {
	var prototype = object(father.prototype); //プロトタイプをゲット
	prototype.constructor = son; //プロトタイプを自分のコンストラクタに指定
	son.prototype = prototype; //プロトタイプ継承
}

function father(name) {
	this.name = name;
	this.colors = ['red'];
}
father.prototype.sayName = function() {
	console.log(this.name);
}
function son(name,age) {
	father.call(this, name);
	this.age = age;
}
inheritPrototype(son,father);

var tmp1 = new son('try',19);
tmp1.colors.unshift('yellow');
var tmp2 = new son('tt',10);
console.log(tmp2.colors); //'red'
tmp2.sayName(); //tt
```

この方法は、直接`Object.create()`メソッドで使えます。

```javascript
const a = { a: 1 };
// a ---> Object.prototype ---> null

const b = Object.create(a);
// b ---> a ---> Object.prototype ---> null
console.log(b.a); // 1 （継承）

const c = Object.create(b);
// c ---> b ---> a ---> Object.prototype ---> null

const d = Object.create(null);
// d ---> null （d はプロトタイプとして直接 null を持つオブジェクト）
console.log(d.hasOwnProperty);
// undefined。 Object.prototype を継承していないため
```



## ３、サマリー

JavaScript は Java や C++ から来た開発者にとっては少し戸惑うかもしれません。なぜなら、JavaScript はすべて動的で、すべて実行時であり、静的な型はまったくないからです。すべてがオブジェクト（インスタンス）かメソッド（コンストラクター）であり、メソッド自体も `Function` コンストラクターのインスタンスです。構文の構成要素である「クラス」でさえ、実行時にはコンストラクターメソッドに過ぎないのです。

JavaScript のすべてのコンストラクターメソッドは `prototype` という特別なプロパティを保有しており、このプロパティは `new` 演算子と組み合わせて動作します。プロトタイプオブジェクトへの参照は、新しいインスタンスの内部プロパティ `[[Prototype]]` にコピーされます。例えば、 `const a1 = new A()` とすると、 JavaScript は（オブジェクトをメモリー上で作成した後、 `this` を定義したメソッド `A()` を実行する前に） `a1.[[Prototype]] = A.prototype` を設定します。その後、インスタンスのプロパティにアクセスするとき、 JavaScript はまずそのオブジェクトに直接存在するかどうかを調べ、存在しない場合は `[[Prototype]]` を探します。すなわち、 `a1.doSomething`, `Object.getPrototypeOf(a1).doSomething`, `Object.getPrototypeOf(Object.getPrototypeOf(a1)).doSomething` など、見つかるまで、あるいは `Object.getPrototypeOf` が `null` を返すまで再帰的に調べられます。これは、 `prototype` で定義されたすべてのプロパティが、事実上すべてのインスタンスで共有され、後で`prototype`の一部を変更しても、その変更が既存のすべてのインスタンスに現れることを意味します。



> 参考 [継承とプロトタイプチェーン](https://developer.mozilla.org/ja/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
