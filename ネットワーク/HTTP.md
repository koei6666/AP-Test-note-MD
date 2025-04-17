
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

##### Registry Server Header
```HTTP
HTTP/1.1 200 OK
Docker-Distribution-API-Version: registry/2.0
X-Content-Type-Options: nosniff
Content-Length: 87
Content-Type: application/json; charset=utf-8
Docker-Content-Digest: sha256:a3ed95caeb02ffe68cdd9fd84406680ae93d633cb16422d00e8a7c22955b46d4
Date: Wed, 6 Nov 2024 10:00:00 GMT
```

#### User-Agent
HTTPヘッダーに格納する、ユーザの使用するOS、ブラウザなどの情報を記録するパラメータ
#### GETとPOSTメソッド
GETはURLからデータを取得するメソッドだが、取得するデータを要求する引数を遅れるために、データを送信することもできる。
GETでデータを送信する場合は、データはクエリストリングに埋め込まれる。
```HTTP
通常のHTTPリクエストフォーマット
METHOD /path HTTP/version
headers...
(blank line)
body
```
例
```HTTP
POST /ping.cgi HTTP/1.0
Content-Length: 21

addr=127.0.0.1
```

>[!クエリストリング]
>クエリストリングはURLの一部で、通常は「?」の後に続き、キーと値のペア(key=value)で構成されます。例えば:
>`https://example.com/page?key1=value1&key2=value2`
>ただし、キーなしの値の引き渡しは、サーバー側が単一の値しか受け付けしない場合では可能
>`https://example.com/page?value1`


POSTで情報を送信する場合、情報はメッセージのボディに埋め込まれる。
**そのため、アクセスログなどに、POSTメソッドで送信した内容は残らない。セキュリティ性向上する一方、POSTメソッド方法を使う攻撃などでは、アクセスログからその攻撃の内容を確認できなくなるという問題点もある。**



>[!NOTE] RedirectとPOST Method  
>原則として、ブラウザがRedirectするときではPOSTを使わない、ブラウザが3xx Redirect responseを受信した後、307 Temporary Redirectと308 Permanent Redirect以外の場合は新しいGET Requestを発行し(新しいGET RequestはPOSTから変換され、データボディが廃却される)、Redirect指示を出す。
>307と308の場合でも、ユーザーにPOST Dataの転送要否を確認してから転送する。
>原因は、POSTデータの転送が、特に取引データなどの重複提出を引き起こす可能性があるから。
>この関係で、[[SAML]]認証がサイト間のRedirectを関わる場合では、通常GETとPOSTでSAML Requestを送信できたどころ、GETメソッドに制限され、query stringに記載されることになる。

^0e0ec9



URLの規則を定めたRFC 3986では、URLに使用可能な文字である「英数字」と記号「#;/?:@&=+$,'()」以外を用いるときには「%xx」という%の後に16進2桁の形式に変換してから使用するように定めています。**パーセントエンコーディング**は、一般にURLエンコードと呼ばれ、URLにおいて使用できない文字をURLに記述するために「%」＋16進2桁を組合わせた文字列に変換することをいいます。(16進表示が4桁の場合は設問の例のようになります)

GETメソッドは\"http://aaa.com/bbb.html?ccc=ddd \"というようにURLに続いてパラメタを記述することで、ブラウザからサーバに値を渡す方式です。漢字はURLとして使用できないので、パーセントエンコーディングした文字列で記述されます。

##### POSTパケットの偽造困難性
POSTパケットでは、情報をボディに埋め込まれているので、情報のパラメーターなどは外部から推測にくく、例えばGETとPOST両方受け付けるURLがあるとして、GETの場合は閲覧のみ、POSTの場合は状態変化させることができ、そして他の公開されているサイトと共有したCookieを使用する場合、
攻撃者は公開されているサイトのcookieを含んだGETリクエストを公開サイト向けに取得し、それを改造することで、上記の攻撃宛先URLに対してアクセスすることはできるが、POSTリクエストでは、そのメッセージボディの情報パラメーターを攻撃者が推測することが難しいので偽造することも困難である。

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
- [[OAuth]]
- JWT
- API Key
- Hawk
- AWS Signature
#### CONNECTメソッド
CONNECTメソッドはプロキシサーバでよく使われる、プロキシサーバは通信の中身を解釈し、再構成することで通信の中継を行なっているが、HTTPSなど暗号化した通信の場合、プロキシサーバでは暗号化された通信を復号できないため、CONNECTメソッドでトンネリングを行い、パケットをいじらずに単に中継することを依頼する。

[[Preflight Request]]

[[Content Disposition]]

