---
title: flutterで歌詞展示画面を作る
category: flutter
description: Dartでスクレイパーして歌詞展示画面を作ります。
abbrlink: a1d3d31
date: 2024-03-21 17:39:10
tags:
---

## １、画面＆機能

画面は歌一覧リストと歌詞詳細二つがあり、一覧画面から歌詞を見たい歌をクリックし、歌詞詳細画面に遷移します。

歌詞詳細画面にふりがな付きの歌詞が表示されます。

## ２、詳細

### ホームページ

`DefaultTabController`ウィジェットを使って三つのタブを作成します。

- `length`：タブの数；
- `child`：子ウィジェットで、ここは`Scaffold`ウィジェットを入れます。

```dart
Widget build(BuildContext context) {
  return DefaultTabController(length: 3, child: Scaffold(
    // ...
    ),
  );
}
```

`Scaffold`ウィジェットはflutterの主画面でよく使われているウィジェットで、ページに必要な要素がそこに揃っています：

- `appBar`：ナビゲーションバー、`AppBar`ウィジェットを入れて、タイトルや右の共有ボタンを作れます。例：

```dart
appBar: AppBar(
  title: const Text('My home1'),
  centerTitle: true,
  actions: <Widget>[
    IconButton(onPressed: () {},
    icon: const Icon(Icons.search)),
  ],
),
```

- `drawer`：ドロワーメニュー：`Drawer`ウィジェットを入れて、`child`プロパティで入れたい要素容器ウィジェットを入れてメニュー作ります。例：

```dart
drawer: Drawer(
    child: ListView(
      children: const <Widget>[
        UserAccountsDrawerHeader(
          decoration: BoxDecoration(
            color: Colors.blue,
          ),
          accountName: Text('Puiken'),
          accountEmail: Text('xxx@gmail.com'),
          currentAccountPicture: CircleAvatar(backgroundImage: NetworkImage('https://cdn.jsdelivr.net/gh/try0929/cdn/img/blog_avatar.JPG'))
        ),
        ListTile(title: Text('To do list'), trailing: Icon(Icons.schedule)),
        ListTile(title: Text('Alarm'), trailing: Icon(Icons.alarm)),
        ListTile(title: Text('Settings'), trailing: Icon(Icons.settings)),
        Divider(),
        ListTile(title: Text('Log out'), trailing: Icon(Icons.logout)),
      ],
    ),
  ),
```

- `bottomNavigationBar`：ボトムナビゲーション：`TabBar`ウィジェットを入れて、`tabs`プロパティに各タブのテキストやアイコンなどを入れます。例：

```dart
bottomNavigationBar: const TabBar(tabs: <Widget>[
    Tab(text: 'Home', icon: Icon(Icons.home)),
    Tab(text: 'Songs', icon: Icon(Icons.list_alt)),
    Tab(text: 'Personal', icon: Icon(Icons.person)),
  ]),
```

- `floatingActionButton`： 右下隅には浮動アクションボタン
- `body`：画面下のタブナビゲーションを使いたいので、ここに`TabBarView`を入れて、`children`プロパティに各タブで表示したいページクラスのインスタンスを入れます。

```dart
body: const TabBarView(children: <Widget>[
  ListPage(),
  SongsList(),
  ListPage(),
])
```

### 歌一覧画面

![image-20240321174820049](https://cdn.jsdelivr.net/gh/try0929/cdn/img/image-20240321174820049.png)

ホームページと同じく、`Scaffold`ウィジェットを使用しています。

ページのクラスは`SongsList`で、ステートクラスは`_SongsListState`。

この画面に自分のデータがあるため、`StatefulWidget`を使用しますので、ステートクラス`_SongsListState`が必要です。

> flutterに、自分のデータを持ちたいウィジェットは`StatefulWidget`クラスに継承し、持ちたくない方は`StatelessWidget`クラスに継承します。前者自分のデータを自分に持つのではなく、新しく作成されたステートクラス（一対一）に持たせます。

全体の感じはリストなので、`ListView`ウィジェットを使って歌リストを表示しますが、外に`padding`や`border`などを入れたいので、`ListView`の外にもう一層`Container`ウィジェットを使います。

- `padding`：`EdgeInsets`クラスの函数で数値入れます、例：`EdgeInsets.only(left: 10, right: 10)`；
- `decoration`：`BoxDecoration`クラスインスタンスを入れて、文字の色(color)やボーダー(border)などを設定できるプロパティです、例：

```dart
decoration: const BoxDecoration(
	color: Colors.black,
  border: Border(
    top: BorderSide(color: Colors.black38)
  ),
)
```

- `child`：この画面はリストを表示したいので、`ListView`ウィジェットを使用します。また、リストは事前に準備して、画面上に配列から値を取り出すため、`ListView.builder`を使います。

#### `ListView.builder`

- `itemCount`：表示したいリストのアイテム数；
- `itemBuilder`：リストのアイテムと処理する函数、引数は`context`と`index`、戻り値は画面上に表示したいアイテム要素です。
  - ここでアイテムタップしたら画面遷移して欲しいので、戻り値は`GestureDetector`クラスのインスタンスにします（`GestureDetector`は要素にリスナーをバインドし、`tap`や`press`などのイベントが発生する際に処理を付与できるクラスです。）

```dart
ListView.builder(
  itemCount: list.length,
  itemBuilder: ((context, index) {
    var song = list[index];
    return GestureDetector(
      onTap: () {
        Navigator.push(context, MaterialPageRoute(builder: (BuildContext ctx) {
          return LyricPage(songId: song['songId'] as String, title: song['title'] as String, singer: song['singer'] as String);
        }));
      },
      child: Container(
        decoration: const BoxDecoration(
          border: Border(
            bottom: BorderSide(color: Colors.black26)
          )
        ),
        child: ListTile(title: Text(song['title'] as String)),
      ),
    );
  })
)
```

### 歌詞詳細ページ

![image-20240321174953393](https://cdn.jsdelivr.net/gh/try0929/cdn/img/image-20240321174953393.png)

一覧画面から詳細画面まで遷移する時は、`songId`、`title`、`singer`を持っていきます。

詳細画面で三つのデータを使って該当歌の詳細をとってきます（DBか、ネットか）。

今回の歌詞はふりがなをつけたいので、既に作成されているあるサイトでスクレイピングして情報をとってきて表示することになります。

流れ：

前画面から持ってきた`songId`、`title`、`singer`で、DBで検索します：

- 該当のデータがある場合、表示します；
- ない場合、ネットで検索して、情報を処理してDBに入れてから表示します。

下記のライブラリを追加します：

```
http: ^0.13.5
html: ^0.15.1
```

#### 画面

画面の`initState`でデータをとってきます。

主に`LyricRequest`クラスの`getLyric`で処理を行なってます。

```
@override
void initState() {
  super.initState();
  lyricReq.getLyric(title: widget.title, singer: widget.singer, songId: widget.songId)
    .then((var list) {
      setState(() {
        lyricList = list as List<dynamic>;
      });
    });
}
```

#### DB操作

**１、DBのコネクション**

今回のDB操作はそこまで複雑ではないので、flutterの`sqflite`ライブラリを使用します。

データベースの初期化（`DatabaseHelper`クラス）：

`getDatabasesPath`メソッドでDBのパスをもって、`openDatabase`メソッドでコネクションを作ります。

```dart
class DatabaseHelper {
  static final DatabaseHelper instance = DatabaseHelper._init();
  static Database? _database;

  DatabaseHelper._init();

  Future<Database> get database async {
    if (_database != null) return _database!;
    _database = await _initDB('database.db');
    return _database!;
  }

  Future<Database> _initDB(String filePath) async {
    final dbPath = await getDatabasesPath();
    final path = join(dbPath, filePath);
    return await openDatabase(
      path,
      version: 1,
      onCreate: (db, version) async {
        await db.execute('''
          CREATE TABLE lyric (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            title TEXT,
            singer TEXT,
            lyric_content TEXT,
            created_time INTEGER,
            modified_time INTEGER
          );
        ''');
      },
    );
  }
}
```

**２、テーブルのモデルファイル**

テーブルの定義しているファイル

```dart
import 'dart:convert';

class Lyric {
  final String? title;
  final String? singer;
  final String? lyricContent;
  final DateTime createdTime;
  final DateTime modifiedTime;

  const Lyric({
    required this.title,
    required this.singer,
    required this.lyricContent,
    required this.createdTime,
    required this.modifiedTime,
  });

  String? getLyricContent() {
    return lyricContent;
  }

  List<dynamic> parseLyricContent() {
    return json.decode(lyricContent!);
  } 

  Map<String, dynamic> toMap() {
    // flutterのデータをDB形式に変換する
  }

  factory Lyric.fromMap(Map<String, dynamic> map) {
    // DBのデータをflutterで使えるデータに変換する
  }
}
```

**３、テーブルの操作＆スクレイピング**

テーブルの操作は検索と追加があります。

スクレイピングはflutterの`html`ライブラリの`parse`メソッドを使って、リクエストでとってきたHTMLファイルを解析します。

```dart
class LyricRequest {
  LyricRequest();
  late Lyric lyricIns;

  Future<List<Lyric>> checkIfExist({String? title}) async {
    return await getLyricFromDB(title: title);
  }

  void addNewSongToDB({String? title, String? singer, String? lyricContent}) async {
    lyricIns = Lyric(title: title, singer: singer, lyricContent: lyricContent, createdTime: DateTime.now(), modifiedTime: DateTime.now());
    await insertLyricToDB(lyricIns.toMap());
  }

  Future<List?> getLyric({ String? title, String? singer, String? songId }) async{
    final existList = await checkIfExist(title: title);
    if (existList.isNotEmpty) {
      lyricIns = existList[0];
      return lyricIns.parseLyricContent();
    }
    var url = Uri.parse('https://xxx.com/lyric/$songId/');
    print('---- Not existing, call network --- $url');
    var response = await http.get(url);
    if (response.statusCode == 200) {
      print('success!');
      // HTMLファイルの解析
      var document = parse(response.body);
      var hiraganaList = document.querySelector(".hiragana");
      var parsedLyric = parseLyric(hiraganaList?.nodes);
      addNewSongToDB(title: title, singer: singer, lyricContent: json.encode(parsedLyric));
      return parsedLyric;
    } else {
      print('Failed!');
    }
    return null;
  }

  parseLyric(hiraganaList) {
    // HTMLストラクチャーの解析
  }
}
```
