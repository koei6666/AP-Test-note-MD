JSON Web Token
ユーザー認証（よく使われるシーンはAPI承認のために、サーバとクライアントの間に交換される）に必要な情報をJSONのフォーマットとして記載し、暗号化した上HTTPのリクエストに乗せることで、ステートレスの接続のユーザー認証に使われる。

## 構造

## Encoded
JWTは送信される時エンコード[[#安全性の懸念|(暗号化はされていない）]]されまして、'.'で以下の3つの構成を区分している。
エンコードされている状態では以下のような形になる:
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
```JSON
{
  "loggedInAs": "admin",
  "iat": 1422779638
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
   
   



