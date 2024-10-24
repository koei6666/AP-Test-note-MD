
Hyper Text Transfer Protocol
WebサーバーとWebクライアントの間に、HTMLなどのデータをやり取りするためのプロトコルである。
WebクライアントがWebサーバーにリクエストを発行することで通信が始まる。
HTTPには以下のメソッドがよく使われる：
- GET:指定のURLにデータを取得する
- POST:指定のURLにデータを送信する
- PUT:指定のURLへデータを保存する
- DELETE:指定URLのデータを削除する
- CONNECT:プロキシにトンネリングを要求する

#### HTTP Header
```HTTP
Request Headers
GET / HTTP/1.1
Host: www.ap-siken.com
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1 
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebkit/537.36 (KHTML
Accept: text/html,application/xhtm1+xm1, application/xml;q=0.9,image/webp, Accept-Encoding: gzip, deflate, sach
Accept-Language: ja, en-US;q=0.8, en;q=0.6
Cookie: _utma=82185798.227027948.1358520982.1458747825.1458856749.1162;
```

#### User-Agent
HTTPヘッダーに格納する、ユーザの使用するOS、ブラウザなどの情報を記録するパラメータ
#### GETとPOSTメソッド
GETはURLからデータを取得するメソッドだが、取得するデータを要求する引数を遅れるために、データを送信することもできる。
GETでデータを送信する場合は、データはクエリストリングに埋め込まれる。
>[!クエリストリング]
>クエリストリングはURLの一部で、通常は「?」の後に続き、キーと値のペアで構成されます。例えば:
>`https://example.com/page?key1=value1&key2=value2`


POSTで情報を送信する場合、情報はメッセージのボディに埋め込まれる。
**そのため、アクセスログなどに、POSTメソッドで送信した内容は残らない。セキュリティ性向上する一方、POSTメソッド方法を使う攻撃などでは、アクセスログからその攻撃の内容を確認できなくなるという問題点もある。**

URLの規則を定めたRFC 3986では、URLに使用可能な文字である「英数字」と記号「#;/?:@&=+$,'()」以外を用いるときには「%xx」という%の後に16進2桁の形式に変換してから使用するように定めています。**パーセントエンコーディング**は、一般にURLエンコードと呼ばれ、URLにおいて使用できない文字をURLに記述するために「%」＋16進2桁を組合わせた文字列に変換することをいいます。(16進表示が4桁の場合は設問の例のようになります)

GETメソッドは"\http://aaa.com/bbb.html?ccc=ddd"というようにURLに続いてパラメタを記述することで、ブラウザからサーバに値を渡す方式です。漢字はURLとして使用できないので、パーセントエンコーディングした文字列で記述されます。

#### Authorization Header
クライアントを認証するためのヘッダー、クライアントからサーバへのリクエストが承認必要な場合に、その承認情報を送信するために使用される。

- GET METHODの場合
```HTTP
GET /api/user/profile HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Accept: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
```

- POST METHODの場合
```HTTP
POST /api/user/profile HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
Content-Type: application/json
Accept: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36

{
  "name": "John Doe",
  "email": "john.doe@example.com"
}
```

##### Authorization Schema
Authorization FiledのBearerは、トークンベースの承認スキームを指定するキーワードで、下記のような構成になる。
`Authorization: Bearer <Token>`

Bearerの他に、下記の通りいくつか他のスキーマもある。
- Basic
- Digest
- OAuth
- JWT
- API Key
- Hawk
- AWS Signature
#### CONNECTメソッド
CONNECTメソッドはプロキシサーバでよく使われる、プロキシサーバは通信の中身を解釈し、再構成することで通信の中継を行なっているが、HTTPSなど暗号化した通信の場合、プロキシサーバでは暗号化された通信を復号できないため、CONNECTメソッドでトンネリングを行い、パケットをいじらずに単に中継することを依頼する。

[[Preflight Request]]