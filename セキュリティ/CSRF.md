---
分類: 攻撃手段
---

**クロスサイトリクエストフォージェリ(CSRF)**

悪意のあるスクリプトが埋め込まれたWebページを訪問者に閲覧させて，別のWebサイトで，その訪問者が意図しない操作を行わせる攻撃手法。会員制サイトでログイン状態であるときに会員情報の変更や商品の注文画面への直接リンクを踏ませる(またはスクリプトでリダイレクトする)などの意図しない不正な処理要求を行わせる行為がこれに該当する。

## 仕組み
攻撃者は、サーバに対する悪意のあるリクエスト（例えば銀行に対する送金の依頼、アカウントに対するパスワード変更の依頼など）を埋め込んだリンクを作成し、目標に踏ませるように仕込む。
目標は対象サイトにログインした状態（ログイン済みのCookieを持っている状態）でリンクを踏むと、ブラウザが自動的にサイトにCookieを送信し、ユーザ認証を完了させ、不正リクエストを実行させる。

## 対策
### CSRF Token
Cookieでセッション管理の他に、サーバ側が生成し、ユーザ側が保管するCSRK Tokenを使用して、リクエストは偽造かどうかを識別することができる。
#### 仕組み
サーバとクライアント間に新しいセッションが確立された際に、サーバが新しいCSRF Tokenをクライアントに送信する。
CSRF Tokenは推測困難の乱数で作成され、通常はHTML中のHidden fieldに埋め込むか、HTTPヘッダーに埋め込むことでクライアントに送付。
クライアントがサーバに対して新しい（State changing: PUT, POST, DELETE, etc,)リクエストを送信する際に、CSRF Tokenを埋め込んで送信。そしてサーバ側がそのTokenを認証し、リクエストの正当性を検証する。
CSRF Tokenの有効性だけを検証する場合では、CSRF Token自体が盗聴された場合、それが利用されて成り済ますことができてしまうので、CSRFとユーザーセッションオブジェクトとの紐付けも必要で、検証時はCSRF Tokenの有効性と、セッションオブジェクトのリンクしたものとの一致性も検証する必要がある。

### その他の対策
- Same-Site Cookies:
    - Set cookies with the SameSite attribute (Strict or Lax).
    - Prevents browsers from sending cookies in cross-site requests.
- Custom Request Headers:
    - Require custom headers (e.g., X-Requested-With) for state-changing requests.
    - Browsers restrict setting custom headers in cross-origin requests.
- Double Submit Cookie:
    - Send a random token in both a cookie and as a request parameter.
    - Server verifies both values match.
- Origin Header Checking:
    - Validate the Origin or Referer header on incoming requests.
    - Ensure requests come from your own domain.
- User Interaction Based Protection:
    - Require user interaction (e.g., re-authentication) for sensitive actions.
- URL Rewriting:
    - Include session identifiers in URLs instead of cookies (has usability drawbacks).
- CAPTCHA:
    - Implement for sensitive actions, though it impacts user experience.
- One-Time Tokens:
    - Generate single-use tokens for each form or action.
- Strict [[Content Security Policy]] (CSP):
    - Implement CSP headers to control which sources can load content.
- Multi-Step Transactions:
    - Break sensitive operations into multiple steps with checks.
- Session Expiration:
    - Implement short session timeouts and secure logout mechanisms.
- Encrypted Token Pattern:
    - Use encrypted tokens that include user information and expiration.