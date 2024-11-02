---
分類: プロトコル
---

HTTP ヘッダーを下記のとおりに設定することで、ヘッダー設定したページにあるリソースが、他のドメインからアクセスできるかどうかを制限する。


## CORS Headers
```http
# Allow specific origin
Access-Control-Allow-Origin: https://trusted-site.com
```
`https://trusted-site.com`からのアクセスしか許可しない。

```http
# Or allow all origins (not recommended for production)
Access-Control-Allow-Origin: *
```
全てのオリジン（ドメイン）からのアクセルを許可する

```http
# Allow specific HTTP methods
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
```
`GET,POST,PUT,DELETE,OPTIONS`のメソッドからのアクセルしか許可しない

```http
# Allow specific headers
Access-Control-Allow-Headers: Content-Type, Authorization, X-Requested-With
```
許可するアクセスパケットのヘッダーを指定する

```http
Access-Control-Allow-Headers: Content-Type, Authorization, X-Requested-With

1. Content-Type: 
   - Specifies the media type of the request body
   - Examples:
     application/json
     application/x-www-form-urlencoded
     multipart/form-data

2. Authorization:
   - Used for sending authentication credentials
   - Examples:
     Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6...
     Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ=

3. X-Requested-With:
   - Custom header often used to identify AJAX requests
   - Common value:
     X-Requested-With: XMLHttpRequest
```


```http
# Allow credentials (cookies, authorization headers)
Access-Control-Allow-Credentials: true
```
`cookies`や`authorization headers`を含ませたアクセスを許可するかどうか
`Access-Control-Allow-Credentials: true`の場合では、`Access-Control-Allow-Origin`をWildcard(\*)の設定をサポートしない

```http
# Cache preflight request for 1 hour (in seconds)
Access-Control-Max-Age: 3600
```
ブラウザから送信される[[preflight request]]の保存期限

```http
# Expose specific headers to the client
Access-Control-Expose-Headers: Content-Length, X-Request-Id
```
`Access-Control-Expose-Headers`は、ブラウザに、違うドメインからのリクエストで、レスポンスヘッダーの内どのヘッダーはクライアント側にあるjavascriptにアクセスできるを指定する

デフォルト（設定しない場合）は以下のヘッダーにJavascriptがアクセスできる
- Cache-Control
- Content-Language
- Content-Type
- Expires
- Last-Modified
- Pragma
```javascript
// Making a cross-origin request
fetch('https://api.example.com/data')
  .then(response => {
    // By default (without Access-Control-Expose-Headers):
    console.log(response.headers.get('Content-Type'));  // ✓ Works
    console.log(response.headers.get('X-Request-Id'));  // ✗ Returns null
    console.log(response.headers.get('Content-Length')); // ✗ Returns null
});

// With Access-Control-Expose-Headers set on server:
Access-Control-Expose-Headers: Content-Length, X-Request-Id

// Now JavaScript can access these headers:
fetch('https://api.example.com/data')
  .then(response => {
    console.log(response.headers.get('Content-Length')); // ✓ "1234"
    console.log(response.headers.get('X-Request-Id'));   // ✓ "abc-123"
});
```