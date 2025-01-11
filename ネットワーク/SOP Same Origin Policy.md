
Webブラウザに設定される仕組みで、Cookieをはじめとする以下の要素には、同一オリジン（Same Origin)のスクリプトからしかアクセスできないように制限している。
>[!Note]　アクセス制限の要素
> - Cookie, LocalStorage, IndexedDB
> - DOM要素
> - Ajax(XMLHttpRequest, Fetch)リクエスト

同一オリジンかどうかは以下の3要素から判断する
1. プロトコール
2. ドメイン名
3. ポート番号

例えば、
`https://example.com`の`Cookie`には、

`https://example.com/page1`からはアクセスできるが、プロトコールの異なる`http://example.com`からはアクセス禁止となり、
ドメインの異なる`https://other.com`からは勿論、サブドメイン`https://sub.example.com`からや、ポート番号が異なる`https://example.com:8080`からのアクセスも禁止されている

### CSPとの違い
SOPはブラウザに設定され、ドメインを検証してリソースのアクセスを制御しているが、CSPは各種のリソースに対して細かくアクセスの許可を設定することができる
SOPはブラウザに設定されるのに対して、CSPはサーバ側でHTMLヘッダーに設定される
CSPを設定することで、SOPを一部緩和したり、またはより精密に制御することができる
[[Content Security Policy]]

##  CORS
WEBサイトのデザイン上、異なるサイトの資源にアクセスさせる必要がある場合がある、その場合は、特定のドメインからのアクセスを許可するよう、 CORSをヘッダーに設定することができる
[[CORS Cross Origin Resource Sharing]]