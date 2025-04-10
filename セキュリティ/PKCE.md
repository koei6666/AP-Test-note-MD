---
分類: 暗号化
---
>[!NOTE]　PKCE におけるbase64urlエンコーディングの必要性
>PKCEでは仕様上(RFC 7636)、Code challengeを生成時、Code veriferをbase64urlでencodingすることを必須としている。
>その理由は、PKCEでコードチャレンジを送信時では、SHA-256などのハッシュアルゴリズムから得たハッシュ値のバイナリデータをHTTPのパラメータとして使用できなく、特に中の`+`や`/`などの符号はHTTPでは安全ではないので。
>Base64urlでバイナリデータをエンコードしてから送信する必要がある。

PKCEフロー（Proof Key for Code Exchange）について解説いたします。

PKCEは、OAuth 2.0の拡張機能で、主にパブリッククライアント（ブラウザベースのアプリケーションやモバイルアプリ）のセキュリティを向上させるために設計されました。

#Tricky PKCEフローの主な手順は以下の通りです：
```mermaid
flowchart TD
	A[Client] -.->|Authentication Request| B(Code Verifier=Random String) -.->|Hash and encode with base64url| C(Code Challenge) -->|Hash Algo & Code Challange| Authenticator -->|Code challenge stored at Authenticator, return code to client| A
```

1. クライアントが認可リクエストを作成する際に、ランダムな文字列（code verifier）を生成します。

2. code verifierをハッシュ化して code challenge を作成します。

3. クライアントは認可リクエストに code challenge と使用したハッシュ方法を含めて送信します。

4. 認可サーバーは code challenge を保存し、通常の認可コードを返します。

5. クライアントがアクセストークンを要求する際、元の code verifier を含めて送信します。
```mermaid
flowchart TD
	A[Client] -.->|Request for Access token| B[Original Code verifier] --> C[Authenticor] -.->|Hash the code verifier and verify it with stored code challenge| A
```

7. 認可サーバーは受け取った code verifier をハッシュ化し、保存していた code challenge と比較して検証します。

#Tricky PKCEの認証フローをまとめると：クライアントがチャレンジコードのハッシュ値とハッシュアルゴリズムを認可リクエストとともにAuthenticatorに送付し、Authenticatorがハッシュ値とアルゴリズムをサーバに保存し、クライアントに認可コードを返却。
#Tricky クライアントがアクセスコードを要求する際に、元のチャンレンジコードをAuthenticatorに送付し、Authenticatorが受領したチャンレンジコードを当初保存したアルゴリズムでハッシュし、保存したハッシュ値と比較する、一致すれば、クライアントにアクセスコードを返送する。

この方法により、以下のセキュリティ上の利点があります：

- 認可コードの傍受攻撃を防ぐ
- クライアントの偽装を困難にする
- クライアントシークレットを使用せずに、クライアントを認証できる

PKCEは特に、クライアントシークレットを安全に保管できないモバイルアプリやシングルページアプリケーション（SPA）で重要です。

