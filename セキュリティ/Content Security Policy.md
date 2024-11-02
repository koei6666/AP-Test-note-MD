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