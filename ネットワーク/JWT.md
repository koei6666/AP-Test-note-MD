JSON Web Token
ユーザー認証（よく使われるシーンはAPI承認のために、サーバとクライアントの間に交換される）に必要な情報をJSONのフォーマットとして記載し、暗号化した上HTTPのリクエストに乗せることで、ステートレスの接続のユーザー認証に使われる。

## 構造

## Encoded
JWTは送信される時エンコード[[#安全性の懸念|(暗号化はされていない）]]されまして、'.'で以下の3つの構成を区分している。
エンコードされている状態では以下のような形になる:

`header.payload.signature`

```JWT
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiIiLCJpYXQiOjE2NzYyMTc5NTAsImV4cCI6MTcwNzc1Mzk1MCwiYXVkIjoiYWthbWFpLWJsb2ciLCJzdWIiOiIiLCJjb21wYW55IjoiQWthbWFpIiwidXNlciI6IkFrYW1haS1yZWFkZXIiLCJhZG1pbiI6Im5vIn0.kMPz3Z7BSlBTJKijD8bcrpzTZejX7VCZ77w5oQwJO6I
```

### Header

```JSON
{
	"alg" : "HS256",
	"typ" : "JWT"
}

```
ヘッダーには、JSONファイルのタイプを指定する"typ"のパラメーターと、署名を生成するアルゴリズムを指定する”alg"パラメーターがある。
"alg"パラメーターをNoneに指定することもでき、悪意でalgをNone指定に改ざんし、署名の認証をスキップさせる攻撃を[[攻撃手段リスト#alg=None攻撃|alg=None攻撃]]という。

### Payload

Issued At Time claim (`iat`)
Expire claim(`exp`)
```JSON
{
  "loggedInAs": "admin",
  "iat": 1422779638,
  "exp": 1422779660
}
```

### Signature

```JSON
HMAC_SHA256(
  secret,
  base64urlEncoding(header) + '.' +
  base64urlEncoding(payload)
)
```

## 安全性の懸念
1. 有効性の管理
　　JWTはステートレスのため、JWT単体では自身を無効にすることはできない。JWTの有効性を管理するためには、サーバ側で何らかのステート管理が必要になる。
2. ペーロードの改ざん
   JWTは暗号化されていないため、攻撃者に入手された場合では、Payloadを変更することで、権限のエスカレーションをすることができる。
   例えば下記例：
   `{"loggedInAs": "user"}`-> `{"loggedInAs": "admin"}`
   **対策方法**:Payloadのカスタムフィールドに、ユーザー名や識別コードなどを追加し、ユーザを検証する仕組みをサーバ側に導入する
3. ![[攻撃手段リスト#alg=None攻撃]]
   **対策方法**:JWTを検証し、alg＝Noneのtokenを禁止する他に、サーバ側の署名検証アルゴリズムを固定にし、JWTのヘッダーのアルゴリズム指定に従わない。

4. 同じ秘密鍵の流用
   異なるアプリケーションの認証に対して、同じ秘密鍵を使う場合では、他のアプリケションに発行されるtokenを用いてこのアプリケーションのログインを成功し、他人のアカウントを乗っ取れるリスクがある。
   手法：ユーザIDを暗号化し、JWTを発行するアプリa,bがあるとして、a、bは同じ秘密鍵を使用している。
   通常ユーザcがアプリaで、id=1234でアカウントを作成した、攻撃者はアプリbで同じくid=1234のアカウントを作成し、JWT Tokenを入手する、そしてそのtokenをアプリaに送付する。
   アプリaは署名を検証したところ、id=1234をもとに発行した署名は間違いなかったので、攻撃者にアプリaへのアクセス権を付与

# `.parseClaimsJws()` vs `.parse()` in JWT Processing

These methods represent two distinct approaches to JWT token processing, primarily found in JWT libraries like JJWT (Java JWT). The key differences lie in their signature validation behavior and return types.

## `.parseClaimsJws()`

```java
Jws<Claims> claimsJws = Jwts.parserBuilder()
    .setSigningKey(key)
    .build()
    .parseClaimsJws(tokenString);
```

**Functionality:**

- **Signature Validation**: Enforces cryptographic signature verification
- **JWS-Specific**: Processes only signed tokens (JSON Web Signatures)
- **Automatic Verification**: Immediately validates the token's signature against the provided key
- **Throws Exceptions**: Will generate `SignatureException` for invalid signatures
- **Claims Extraction**: Returns a `Jws<Claims>` object with direct access to payload claims
- **Security Focus**: Ensures token integrity before allowing access to claims

## `.parse()`

```java
Jwt jwt = Jwts.parserBuilder()
    .build()
    .parse(tokenString);
```

**Functionality:**

- **Structure Parsing**: Decodes and parses JWT structure without validation
- **Format Agnostic**: Processes any JWT format (signed or unsigned)
- **No Immediate Verification**: Does not validate signatures by default
- **Generic Return Type**: Returns a general `Jwt` object requiring casting for claims access
- **Inspection-Oriented**: Suitable for examining token structure or processing unsigned tokens
- **Security Implication**: Bypasses integrity verification, creating potential security risks

## Security Considerations

The difference between these methods has significant security implications:

- **Authentication Bypass**: Using `.parse()` without subsequent signature validation may allow attackers to forge tokens
- **Header Manipulation**: Without signature validation, attackers could alter the `alg` header to downgrade security
- **None Algorithm Attack**: The `.parse()` method doesn't protect against the "none" algorithm vulnerability

## Best Practice Implementation

For secure JWT processing:

```java
// Secure approach with proper signature validation
try {
    // Explicitly require signature verification
    Jws<Claims> claimsJws = Jwts.parserBuilder()
        .setSigningKey(secretKey)
        .requireSignature() // Explicitly require signatures
        .build()
        .parseClaimsJws(token);
    
    // Access claims only after validation
    Claims claims = claimsJws.getBody();
    String subject = claims.getSubject();
    
} catch (JwtException e) {
    // Handle invalid tokens (expired, malformed, tampered)
    // Log attempt with appropriate context for security monitoring
    throw new AuthenticationException("Invalid token", e);
}
```

From a security perspective, `.parseClaimsJws()` is strongly preferred for authentication contexts as it implements the verification step essential for maintaining the JWT security model.


## Refresh Token Mechanism in JWT Architecture

A **refresh token** is a long-lived credential used to obtain new access tokens without requiring user re-authentication. In JWT-based authentication systems, refresh tokens serve as a security mechanism to balance token longevity with security risk mitigation.

## Technical Architecture

### Token Lifecycle Management

The refresh token operates within a dual-token architecture where:

- **Access Token**: Short-lived JWT (typically 15-30 minutes) containing user claims and permissions
- **Refresh Token**: Long-lived credential (hours to weeks) used exclusively for token renewal

### Authentication Flow Process

1. **Initial Authentication**: User credentials are validated, resulting in both access and refresh token issuance
2. **Resource Access**: Access token validates API requests until expiration
3. **Token Refresh**: Upon access token expiration, the refresh token requests new token pairs
4. **Token Rotation**: New refresh token replaces the previous one (recommended security practice)

## Security Implications and Threat Model

### Attack Vector Mitigation

**Token Theft Protection**: Short-lived access tokens minimize exposure window if compromised. Even if an access token is intercepted, its utility expires rapidly.

**Refresh Token Compromise**: More critical than access token theft since refresh tokens can generate new access tokens. Requires immediate revocation capabilities.

### Security Controls Implementation

**Secure Storage Requirements**:

- Refresh tokens must be stored securely (HttpOnly cookies, secure storage mechanisms)
- Never expose refresh tokens to client-side JavaScript or local storage
- Implement proper encryption for token persistence

**Rotation Strategy**:

- Implement refresh token rotation on each use
- Maintain token family tracking to detect replay attacks
- Implement automatic revocation of token families upon suspicious activity

## Vulnerability Considerations

### Replay Attack Prevention

Token rotation prevents refresh token reuse. When a refresh token is used, it becomes invalid, and a new one is issued. Detection of old refresh token usage indicates potential compromise.

### Cross-Site Request Forgery (CSRF) Protection

Refresh tokens stored in HttpOnly cookies require CSRF protection mechanisms. Implement additional validation such as:

- CSRF tokens for refresh operations
- Origin header validation
- SameSite cookie attributes

### Session Management Security

**Revocation Mechanisms**: Implement comprehensive token revocation including:

- Individual token revocation
- User session termination (all tokens)
- Global revocation capabilities for security incidents

**Audit Trail Requirements**: Log all refresh token operations for security monitoring and incident response.

## Best Practices and Standards Compliance

### Token Lifetime Configuration

- Access tokens: 15-30 minutes maximum
- Refresh tokens: Application-dependent (1-24 hours for high-security, up to 30 days for standard applications)
- Implement configurable expiration policies

### Cryptographic Requirements

- Use cryptographically secure random number generation for token creation
- Implement proper JWT signing with RS256 or ES256 algorithms
- Avoid symmetric signing (HS256) in distributed systems

### Monitoring and Alerting

Implement security monitoring for:

- Unusual refresh patterns
- Geographic anomalies in token usage
- High-frequency refresh requests indicating potential automated attacks
- Failed refresh attempts suggesting credential compromise

The refresh token mechanism provides essential security benefits by limiting access token exposure while maintaining user session continuity. Proper implementation requires careful consideration of storage security, rotation policies, and comprehensive revocation capabilities to maintain system security integrity.
   



