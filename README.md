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
