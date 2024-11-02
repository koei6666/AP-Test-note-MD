---
分類: 認証方法
---

# 1. 認可要求 (Authorization Request)
```
GET /authorize HTTP/1.1
Host: authorization-server.com
Content-Type: application/x-www-form-urlencoded

response_type=code
&client_id=YOUR_CLIENT_ID
&redirect_uri=https://your-app.com/callback
&scope=read write
&state=xyz123
```

認可要求：

- HTTPメソッド: GET
- エンドポイント: /authorize
- パラメータ:
    - response_type: [[セキュリティ/OAuth　コードフロー|コードフロー]]を示す"code"
    - client_id: クライアントアプリケーションの識別子
    - redirect_uri: 認可コードを受け取るコールバックURL
    - scope: 要求する権限の範囲
    - state: CSRF対策のための任意の文字列

# 認可要求に対するレスポンス
```
HTTP/1.1 302 Found
Location: https://your-app.com/callback?code=AUTH_CODE_HERE&state=xyz123
```
# 2. アクセストークン要求 (Access Token Request)
```
POST /token HTTP/1.1
Host: authorization-server.com
Content-Type: application/x-www-form-urlencoded
Authorization: Basic BASE64_ENCODED_CLIENT_ID_AND_SECRET

grant_type=authorization_code
&code=AUTHORIZATION_CODE
&redirect_uri=https://your-app.com/callback
```

- HTTPメソッド: POST
- エンドポイント: /token
- ヘッダー:
    - Content-Type: application/x-www-form-urlencoded
    - Authorization: クライアントIDとシークレットをBase64エンコードした基本認証
- パラメータ:
    - grant_type: "authorization_code"（認可コードフローを示す）
    - code: 認可ステップで取得した認可コード
    - redirect_uri: 認可要求時と同じコールバックURL
# アクセストークン要求に対するレスポンス
```
HTTP/1.1 200 OK
Content-Type: application/json

{
  "access_token": "ACCESS_TOKEN_HERE",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "REFRESH_TOKEN_HERE",
  "scope": "read write"
}
```