---
分類: 攻撃手段
---

**クロスサイトリクエストフォージェリ(CSRF)**

悪意のあるスクリプトが埋め込まれたWebページを訪問者に閲覧させて，別のWebサイトで，その訪問者が意図しない操作を行わせる攻撃手法。会員制サイトでログイン状態であるときに会員情報の変更や商品の注文画面への直接リンクを踏ませる(またはスクリプトでリダイレクトする)などの意図しない不正な処理要求を行わせる行為がこれに該当する。

# 仕組み
攻撃者は、サーバに対する悪意のあるリクエスト（例えば銀行に対する送金の依頼、アカウントに対するパスワード変更の依頼など）を埋め込んだリンクを作成し、目標に踏ませるように仕込む。
目標は対象サイトにログインした状態（ログイン済みのCookieを持っている状態）でリンクを踏むと、ブラウザが自動的にサイトにCookieを送信し、ユーザ認証を完了させ、不正リクエストを実行させる。

# 対策
### CSRF Token
Cookieでセッション管理の他に、サーバ側が生成し、ユーザ側が保管するCSRK Tokenを使用して、リクエストは偽造かどうかを識別することができる。
#### 仕組み
サーバとクライアント間に新しいセッションが確立された際に、サーバが新しいCSRF Tokenをクライアントに送信する。
CSRF Tokenは推測困難の乱数で作成され、通常はHTML中のHidden fieldに埋め込むか、HTTPヘッダーに埋め込むことでクライアントに送付。
クライアントがサーバに対して新しい（State changing: PUT, POST, DELETE, etc,)リクエストを送信する際に、CSRF Tokenを埋め込んで送信。そしてサーバ側がそのTokenを認証し、リクエストの正当性を検証する。
CSRF Tokenの有効性だけを検証する場合では、CSRF Token自体が盗聴された場合、それが利用されて成り済ますことができてしまうので、CSRFとユーザーセッションオブジェクトとの紐付けも必要で、検証時はCSRF Tokenの有効性と、セッションオブジェクトのリンクしたものとの一致性も検証する必要がある。

# The typically way to implement CSRF token

The most common and standardized methods for embedding CSRF tokens in web applications are:

## 1. Hidden Form Fields (Most Common)

The hidden form field approach is the most widely adopted implementation pattern across frameworks:

```html
<form method="POST" action="/submit-data">
    <input type="hidden" name="csrf_token" value="a8c1b3d45e6f7g8h9i0j">
    <!-- Form fields -->
    <button type="submit">Submit</button>
</form>
```

This method is used by Django (`{% csrf_token %}`), Laravel (`@csrf`), Rails (`<%= csrf_meta_tags %>`), and most other major frameworks. The server validates that the submitted token matches the one stored in the user's session.

## 2. Meta Tag + JavaScript Headers

For JavaScript-heavy applications and SPAs, embedding the token in a meta tag and retrieving it for AJAX requests is standard:

```html
<head>
    <meta name="csrf-token" content="a8c1b3d45e6f7g8h9i0j">
</head>

<script>
    // Configure token for all AJAX requests
    const csrfToken = document.querySelector('meta[name="csrf-token"]').getAttribute('content');
    
    // Example with fetch API
    fetch('/api/endpoint', {
        method: 'POST',
        headers: {
            'X-CSRF-Token': csrfToken,
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    });
</script>
```

This pattern is commonly used in frameworks like Ruby on Rails (via `csrf_meta_tags`), Laravel, and many JavaScript frameworks.

## 3. Double-Submit Cookie Pattern

Some applications implement CSRF protection using the double-submit cookie pattern:

```
// Set in HTTP response header
Set-Cookie: CSRF-TOKEN=a8c1b3d45e6f7g8h9i0j; SameSite=Lax; Secure
```

```html
<form method="POST" action="/submit-data">
    <input type="hidden" name="csrf_token" value="a8c1b3d45e6f7g8h9i0j">
    <!-- Form fields -->
    <button type="submit">Submit</button>
</form>
```

The server then verifies that the token in the cookie matches the token in the form field or request header.

## 4. Framework-Specific Implementation Examples

### Django Implementation

```html
<form method="POST" action="/submit">
    {% csrf_token %}
    <!-- Form fields -->
</form>
```

Django's `{% csrf_token %}` template tag renders the following HTML:

```html
<input type="hidden" name="csrfmiddlewaretoken" value="a8c1b3d45e6f7g8h9i0j">
```

### Spring Framework (Java)

```html
<form method="POST" action="/submit">
    <input type="hidden" name="${_csrf.parameterName}" value="${_csrf.token}">
    <!-- Form fields -->
</form>
```

## Security Considerations for Implementation

1. **Token Generation**: Tokens should be cryptographically secure random values
2. **Validation Timing**: Validate tokens early in the request processing pipeline
3. **Transmission**: Always transmit tokens over HTTPS
4. **Regeneration**: Consider regenerating tokens after authentication events
5. **Expiration**: Implement appropriate expiration policies

The hidden form field method combined with server-side session storage remains the most widely adopted, effective, and straightforward implementation for CSRF protection across the industry.

## その他の対策
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