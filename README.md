### ステージ6

モジュールを実装するトレーニング



JavaScript は言語機能としてモジュールの  
仕組みをもっていません。

言語機能としてのモジュールシステムを利用するには  
[ECMAScript 6](https://developer.mozilla.org/ja/docs/Web/JavaScript/ECMAScript_6_support_in_Mozilla) を待たなければなりません。



とはいっても、みんなモジュールを使いたかったので、  
さまざまなモジュールシステムとそれに付随する  
エコシステムが開発されてきました。



- bower
- component
- jam
- volo
- npm with browserify
- spm
- jspm
- duo

(source: [wilmoore/frontend-packagers](https://github.com/wilmoore/frontend-packagers))



あ…めっちゃ多い…&#x1f635;



今回は、利用方法がシンプルな「[bower](http://bower.io)」を使います。



#### bower

bower は、JavaScript、HTML、CSSなどを  
共有して使えるようにするフロントエンドの  
エコシステムです。

他の人が作ったモジュールを利用することや、  
自分が作ったモジュールを公開することも  
できます。



ただ、bower はモジュール読み込みの  
仕組みを提供していません。

この部分は RequireJS など、別の  
モジュールシステムに頼ることになります。  

どのモジュールシステムに対応するかという選択は、
bower によって読み込まれるパッケージ側に  
裁量（責務）があります。



この方針を[公式ドキュメント](http://bower.io/#getting-started)は端的に  
言い表しています。

> How you use packages is up to you.
>
> （どのようにしてパッケージを使うのかはあなた次第です）



#### 実習

まず、bower を実行することを体験してみます。  
bower の設定ファイル bower.json を対話的に  
作成します。

	cd public/stage6/sample
	bower init



あとは説明に従って選択していくと、bower の  
パッケージ設定ファイル `bower.json` が作成されます。



##### 1. name

このパッケージの名前を指定します。

パッケージとして公開する場合には、既に同じ  
パッケージ名が存在していないか確かめる必要が  
あります。

この研修では、公開/非公開を問わないので、  
お好きな名前をつけてください。



##### 2. version

このパッケージのバージョンを指定します。

バージョンの形式は [Semantic Versioning](http://semver.org/lang/ja/) に  
準拠しています。

この形式は、一般的に `X.Y.Z` と記述されます。

- `X` は major version（後方互換性がなくなる変更）
- `Y` は minor version（前方互換性がなくなる変更）
- `Z` は patch version（バグ修正など）

今回は開発版なので、0.0.0 にしておきましょう  
（major versionの 0 は開発版であることを示します）。



##### 3. description

パッケージの簡単な概要を記述します。

有名どころの説明をみてみます。

- bootstrap: The most popular HTML, CSS, and JavaScript framework for developing responsive, mobile first projects on the web.
- angular-latest: HTML enhanced for web apps
- less.js: Leaner CSS



##### 4. main file

このパッケージが外部のパッケージに公開したい  
ファイルを指定します。文字列と配列が指定できます。  
今回は空で問題ありません。



##### 5. what types of module does this package expose?

このパッケージが外部にエンドポイントを公開する  
方法を明示します。

- amd: [Asynchronouse Module Definition](https://github.com/amdjs/amdjs-api/wiki/AMD) ([参考資料](http://www.matzmtok.com/blog/?p=845))
- es6: [EcmaScript 6](http://wiki.ecmascript.org/lib/exe/fetch.php?id=harmony%3Aspecification_drafts&cache=cache&media=harmony:ecma-262_edition_6_03-17-15-releasecandidate3.pdf) ([参考資料](https://www.xenophy.com/javascript/8447#run-time-renaissance))
- globals: グローバル変数経由でエンドポイント公開
- node: [Node.js](https://nodejs.org/api/modules.html)
- yui: [YUI](http://yuilibrary.com/yui/docs/yui/create.html) (メンテ停止したのでもうやめましょう)

今回は何も選択しないで問題ありません。



##### 6. keywords

このパッケージを検索でヒットさせるための  
キーワードを指定します。



##### 7. authors

このパッケージの作者を指定します。



##### 8. license

好きなライセンスを選ぶとよいです。

デフォルトは [MIT ライセンス](http://sourceforge.jp/projects/opensource/wiki/licenses%2FMIT_license)です。



##### 9. homepage

このパッケージの情報が見られる URL を記述します。



##### 10. set currenttly installed components as dependencies?

既に `bower_components` に含まれている  
コンポーネントをパッケージ設定に  
含まれるようにするかどうかを指定します。

n で構いません。



##### 11. add commonly ignored files to ignore list?

`.gitignore` などのファイルから、  
パッケージに含めないファイルの指定を  
読み込むかどうか指定します。

y で読み込ませます。



##### 12. would you like to mark this package as private which prevents it from being accidentaly published to the registry?

bower のレジストリへ登録できないようにするか  
どうか指定します。

y でレジストリへの公開ができないように設定します。



##### 13. Looks good?

この設定で問題なければ y を入力します。



##### bower install

いよいよ、パッケージを追加していきます。

パッケージは [Search Bower packages](http://bower.io/search/) で  
検索することができます。



では、試しに [Buttons](https://github.com/alexwolfe/Buttons) パッケージを  
追加してみましょう。

下のコマンドによって、Buttons パッケージが、
`bower_components` 以下に配置されます。

	bower install --save Buttons



`--save` はパッケージ設定に依存ファイルを  
追記する効果があります（`bower.json` の  
内容が変化しているので、見てみてください）。

ここで設定に追記されたパッケージは、  
次回から `bower install` でまとめて  
取得することができるようになります。



今回は、簡単のために script タグで直接  
`bower_components` 以下の JavaScript/CSS を  
読み込みます。



今回の実習はテスト駆動形式ではありません。

満足のいく Web アプリケーションが書けたら、
`qualityOfYourAppliation` に `true` を  
代入してください。

[http://localhost:8000/stage6/](http://localhost:8000/stage6/)



#### クリアできたら

Lint をかけてみましょう。

    gulp lint-stage6

警告があれば、修正してみてください。



### ステージ7

よくあるイディオムを読むトレーニング



このステージでは、よくある JavaScript の  
不思議な書き方を学びます。

なお、今回はヒントがありません！  
ぜひ自分で調べて、結果を確かめてみてください！



なお、興味のある方は、ステージ「闇」に  
挑戦してみてくださいね！

`.skip` を削除すれば挑戦できるようにに  
なります。



#### 実習

下のテストが green になるように、  
`public/stage7/tests.js` を  
修正してください。

[http://localhost:8000/stage7/](http://localhost:8000/stage7/)



#### クリアできたら

Lint をかけてみましょう。

    gulp lint-stage7

警告があれば、修正してみてください。



付録
----



### 解答・解説について

解答は [2015-example-solution](https://github.com/mixi-inc/JavaScriptTraining/compare/2015...2015-example-solution) で見られます！



### Promise について



#### Promise による平行非同期処理

Promise による平行非同期処理を通常のやりかたと、
Promise らしいやり方とでやってみました。

コードを比較してみてください。



```javascript
// 2つの Web API からレスポンスが欲しい！

var done = { foo: false, bar: false };
var responses = { foo: null, bar: false };
fetch('/api/foo').then(function(responseFoo) {
  if (!done.bar) {
    done.foo = true;
    responses.foo = responseFoo;
    return;
  }
  doSomething(responseFoo, responses.bar);
});
fetch('/api/bar').then(function(responseBar) {
  if (!done.foo) {
    done.bar = true;
    responses.bar = responseFoo;
    return;
  }
  doSomething(responses.foo, responseBar);
});
```

レスポンス取得の待ち合わせ処理があり、  
状態を複数もつ厄介なコードにしあがっていますね。



```javascript
// 2つの Web API からレスポンスが欲しい！

Promise.all([
  fetch('/api/foo'),
  fetch('/api/bar')
])
.then(function(responses) {
  var responseFoo = responses[0];
  var responseBar = responses[1];
  doSomething(responseFoo, responseBar);
});
```

`Promise.all` を使うと、待ち合わせ処理が  
なくスッキリ！



#### Promise による直列非同期処理

直列非同期処理についても、通常のやり方と、  
Promise らしいやり方でやってみました。

コードを比較してみてください。



```javascript
// Web API の結果を利用して別の API を実行したい！

fetch('/api/foo').then(function(responseFoo) {
  doSomething(responseFoo);
  fetch('/api/bar').then(function(responseBar) {
    doSomething(responseBar);
    fetch('/api/buz').then(function(responseBuz) {
      doSomething(responseBuz);
    });
  });
});
```

コードがネストしているので、後ろの方の  
関数のスコープが深くなってしまっています。
変数を追跡するのに手間がかかりそうです。



ネストの外に出すだけならば、終了コールバックを  
呼び出す[継続渡しスタイル](http://ja.wikipedia.org/wiki/%E7%B6%99%E7%B6%9A%E6%B8%A1%E3%81%97%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB) で  
書くことができます。

```javascript
// Web API の結果を利用して別の API を実行したい！

fetch('/api/foo').then(callbackFoo);

function callbackFoo(responseFoo) {
  doSomething(responseFoo);
  fetchBar(callbackBar);
}

function fetchBar(callback) {
  fetch('/api/bar').then(callback);
}

function callbackBar(responseBar) {
  doSomething(responseBar);
  fetchBuz(callbackBuz);
}

function fetchBuz(callback) {
  fetch('/api/buz').then(callback);
}

function callbackBuz(responseBuz) {
  doSomething(responseBuz);
}
```

流れが追いづらい！



クロージャー + 継続渡しスタイルを使うと…

```javascript
// Web API の結果を利用して別の API を実行したい！

fetch('/api/foo').then(fetchBar(fetchBuz(doSomething)));

function fetchBar(callback) {
  return function(responseFoo) {
    doSomething(responseFoo);
    fetchBar(callback);
  };
}

function fetchBuz(callback) {
  return function(responseBar) {
    doSomething(responseBar);
    fetchBuz(callback);
  };
}
```



これはこれで美しい…&#x1f60c;

（JS に慣れるまではちょっと読みづらいと思います）



Promise らしいやり方をとると `.then` で  
次々に処理を連結できます。

```javascript
// Web API の結果を利用して別の API を実行したい！

fetch('/api/foo')
  .then(doSomething)
  .then(function() { return fetch('/api/bar'); })
  .then(doSomething)
  .then(function() { return fetch('/api/buz'); })
  .then(doSomething);
```
