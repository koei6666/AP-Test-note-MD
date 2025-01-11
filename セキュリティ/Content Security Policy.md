---
分類: プロトコル
---

```CSP
Content-Security-Policy: 
  default-src 'self';
  script-src 'self' https://trusted-cdn.com;
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  frame-src 'none';
```

```http
# Example Security Headers (related to SOP)

# Strict-Transport-Security: Force HTTPS
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

# Content-Security-Policy: Control resource loading
Content-Security-Policy: default-src 'self'; \
                       script-src 'self' https://trusted-scripts.com; \
                       img-src 'self' https://trusted-images.com; \
                       style-src 'self' https://trusted-styles.com;

# Cookie settings
Set-Cookie: sessionId=abc123; \
           Secure; \           # Only send over HTTPS
           HttpOnly; \         # Prevent JavaScript access
           SameSite=Strict; \ # Only send in same-site requests
           Domain=example.com; \
           Path=/
```
## 目的
XSS攻撃のリスクを軽減、承認されていないのリソースの読み込みを禁止し、Webページ上の実行できるリソースと読み込みできるリソースを指定する。

## 設定方法
サーバにより、HTTPヘッダーで設定する、また、HTMLの`<meta>`tagで設定する。

## 機能
- Scripts, Styles, Imagesなどの資源を表示できるかどうかを制限
- inline scriptとstyleの禁止
- eval()とそれに類似する関数のコントロール
- https接続の強制

## Directives
- `default-src`:他のdirectiveが未設定の場合デフォルトの許可するコンテンツオリジン
  `default-src 'self' https://trusted-cdn.com;`:デフォルトでは現在のオリジンと`https://trusted-cdn.com`からのコンテンツしか許可しない
  
- `script-src`:実行できるScriptのオリジンを指定する
- `style-src`:実行できるStyleのオリジンを指定する
- `img-src`:表示できる画像のオリジンを指定する
- `connect-src`: 
  `Fetch`,`WebSocket`,`EventSource`,`XMLHttpRequest`などAPIが接続できるURLを制限する
  `connect-src 'self' https://api.example.com;`:現在オリジンのURLまたは、`https://api.example.com`への接続を許可する。

## 実行できるスクリプトについて
script-src 'self'の説明では、”URLと同じオリジンのスクリプトが実行可能”とあるが、それはHTMLに直接記載されているinline scriptではなく、URLと同じオリジンの.jsファイルなどの実行を許可するという意味。

例えば、下記のhtml、中に記載されているスクリプトは確かにHTMLにあり、URLと同じオリジンだが、CSPではデフォルトでinline scriptの実行を禁止している（インジェクションを防止するため）ので、CSP有効なページでは実行されない。
```html
<script>
	alert('xss')
</script>
```
かわりに、スクリプトを実行したい場合は、HTMLに記載されたスクリプトをファイルに保存し、HTMLから呼び出すのがBest practice
```HTML
<script src="/js/main.js"></script>
```