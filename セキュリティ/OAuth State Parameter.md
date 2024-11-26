---
分類: 認証方法
---
## 概要
Stateパラメーターは、OAuthの[[CSRF]]攻撃を防止するために使用するトークン。
StateはクライアントがOAuthプロセスを開始時に、セッションに保存し、そのセッションでユーザをリダイレクトする。
```javascript
// Client generates state
const state = generateRandomString(); // e.g., "xyz123"
// Stores it securely (usually in session)
session.state = state;

// Redirects user to auth server with this state
const authUrl = `https://auth-server.com/oauth/authorize
    ?client_id=${clientId}
    &redirect_uri=${redirectUri}
    &state=${state}      // state is included here
    &response_type=code`;
```
認証サーバがユーザを認証後、同じstateを含ませて、ユーザーをクライアントにリダイレクトする。(ユーザに送信)

クライアントに戻ったユーザーのstateコードを検証し（クライアントに送信)、偽造など（CSRF）でなければ、セッションをクリアにし、認証コードを使用してアクセスコードの要求に移る
```javascript
// At your callback endpoint
function handleCallback(req, res) {
    const { code, state } = req.query;
    
    // VALIDATION HAPPENS HERE
    if (state !== session.state) {
        // Invalid state - potential CSRF attack
        return res.status(403).send('Invalid state');
    }
    
    // Clear the stored state
    delete session.state;
    
    // Proceed with code exchange
    exchangeCodeForToken(code);
}
```
