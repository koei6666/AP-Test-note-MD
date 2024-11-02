---
分類: 攻撃手段
---

動的に構成されるDOMに攻撃者が悪意のあるコードを埋め込み、他のXSSタイプのサーバー側からコード実行とは違って、クライアント側のブラウザによって実行される。

以下のコードのように、変量`name`に悪意のあるコードを埋め込みできる脆弱性があるとする
```Javascript
// URL: https://example.com/page?name=John
let name = new URLSearchParams(window.location.search).get('name');
document.getElementById('greeting').innerHTML = 'Hello, ' + name + '!';
```

攻撃者は下記のようなURLを作成し、攻撃先に踏ませることで、コードをクライアント側で実行させることができる。
```
https://example.com/page?name=<script>alert('XSS')</script>
```

DOM Based XSSはクライアント側の脆弱性になるので、サーバー側の脅威検知システムなどでは検知出来ない。

## 影響
ユーザの代わりにページ上ををアクセスするので、重要データが含まれるページでは特に影響が大きい


## 対策
1. ユーザーインプットを適切にエスケープする
2. 必要ではない場合では、`innerHTML`APIではなく、`textContent`でテキストを出力をする。
3. [[Content Security Policy]]ヘッダーの導入
4. 静的にコード分析チェックツールの活用、動的テストツールで脆弱性を検出する
