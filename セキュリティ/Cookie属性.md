---
分類: プロトコル
---

### httpOnly属性:

httpOnly属性は、クライアントサイドのスクリプト（主にJavaScript）からcookieへのアクセスを制限するセキュリティ機能です。

主な特徴:

- JavaScriptなどのクライアントサイドスクリプトからcookieの読み取りや変更ができなくなります。
- クロスサイトスクリプティング（XSS）攻撃からの保護に役立ちます。
- HTTPリクエスト時にのみサーバーにcookieが送信されます。

使用例:

`Set-Cookie: session_id=12345; HttpOnly`

### secure属性:

secure属性は、cookieの送信をHTTPS接続時のみに制限するセキュリティ機能です。

主な特徴:

- cookieはHTTPS（暗号化された）接続でのみ送信されます。
- 中間者攻撃やパケットスニッフィングからの保護に役立ちます。
- HTTP接続では、cookieは送信されません。

使用例:


`Set-Cookie: user_token=abcdef; Secure`

両方の属性を組み合わせて使用することで、さらにセキュリティを強化できます:

`Set-Cookie: auth_token=xyz789; HttpOnly; Secure`

これらの属性を使用することで、セッション管理や認証に関連する重要な情報を含むcookieのセキュリティを大幅に向上させることができます。ただし、これらの属性を使用する際は、アプリケーションの要件や互換性を考慮する必要があります。

### SameSite属性:

SameSite属性は、クロスサイトリクエストフォージェリ（CSRF）攻撃を防ぐために導入された比較的新しい属性です。OS

値:

- Strict: 同一サイトからのリクエストでのみcookieを送信
- Lax: 同一サイトからのリクエストと、[[セキュリティ/トップレベルナビゲーション|トップレベルナビゲーション]]のGETリクエストでcookieを送信
- None: クロスサイトリクエストを含むすべてのコンテキストでcookieを送信（Secure属性との併用が必要）

例:

`Set-Cookie: session=abc123; SameSite=Strict`

### Domain属性:

Domain属性は、cookieを送信できるドメインを指定します。

特徴:

- 指定されたドメインとそのサブドメインにcookieが送信されます。
- 指定しない場合、cookieはそれが設定されたドメインにのみ送信されます（サブドメインを除く）。

例:

`Set-Cookie: user=john; Domain=example.com`

### Path属性:

Path属性は、cookieを送信するURLのパスを指定します。

特徴:

- 指定されたパスとそのサブパスに対してのみcookieが送信されます。
- デフォルトは "/"で、すべてのパスにcookieが送信されます。

例:

`Set-Cookie: preferences=dark; Path=/account`

### Expires / Max-Age属性:

これらの属性は、cookieの有効期限を設定します。

Expires: 具体的な日時を指定 Max-Age: 現在時刻からの秒数を指定

例:

`Set-Cookie: session=xyz789; Expires=Wed, 21 Oct 2023 07:28:00 GMT Set-Cookie: auth=token123; Max-Age=3600`

### Name / Value:

これは属性ではありませんが、cookieの基本的な構成要素です。

- Name: cookieの名前
- Value: cookieの値

例:

`Set-Cookie: username=alice`

これらの属性を適切に組み合わせることで、セキュリティ、プライバシー、パフォーマンスのバランスを取りながら、効果的にcookieを管理できます。