---
分類: 用語
---
## 概要
A nonce (Number used ONCE) is a unique random value used to prevent replay attacks. 

## 使用例
### 認証リクエスト内の使用例
```
https://authorization-server.com/auth
  ?response_type=id_token
  &client_id=your_client_id
  &redirect_uri=https://your-app.com/callback
  &scope=openid
  &nonce=n-0S6_WzA2Mj   <- ここでnonceを送信
```

### IDトークン内使用例
```json
{
  "iss": "https://authorization-server.com",
  "sub": "user123",
  "aud": "your_client_id",
  "exp": 1516239022,
  "iat": 1516239022,
  "nonce": "n-0S6_WzA2Mj",  <- 認証リクエスト時のnonceが含まれる
  // その他のクレーム
}
```
