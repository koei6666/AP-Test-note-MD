---
分類: 認証方法
---

Security Assertion Markup Language
IdP(Identity Provider)とSP(Service Provider)の間に、異なるドメイン間の承認と認可データを交換するためのXMLベースのopen基準。
標準化団体 OASISによって策定される

## 仕組み
- ユーザがSPの提供したサービスにアクセス
- SPがユーザをIdPにリダイレクトする
- ユーザがIdPにログインし、承認を受ける
- 承認成功した後、IdPがSPにSAML Assertionを送信する
- SPがSAML Assertionを持ってユーザーのアクセスを許可する

## SAML Assertion
SAML AssertionはIdPとSPの間に交換される、ユーザ情報や承認情報が記載されている書類である。
SAML Assertionは以下の3つの部分からなる:
1. Authentication Statement
   ユーザが承認されたタイムスタンプや承認方法などが記載される
   ```xml
   <saml:AuthnStatement 
    AuthnInstant="2024-10-29T09:30:00Z">
    <saml:AuthnContext>
        <saml:AuthnContextClassRef>
            PasswordProtectedTransport
        </saml:AuthnContextClassRef>
    </saml:AuthnContext>
</saml:AuthnStatement>
```
2. Attribute Statemne
   ユーザの情報や、ユーザタイプ（ロール）などの情報を記載
   ```xml
   <saml:AttributeStatement>
    <saml:Attribute Name="email">
        <saml:AttributeValue>john@example.com</saml:AttributeValue>
    </saml:Attribute>
    <saml:Attribute Name="role">
        <saml:AttributeValue>admin</saml:AttributeValue>
    </saml:Attribute>
</saml:AttributeStatement>
```
3. Authorisation Decision Statement
   ユーザに許可した資源と、アクセス許可かどうかの判定を記載
   ```xml
   <saml:AuthzDecisionStatement 
    Resource="https://app.example.com/reports"
    Decision="Permit">
    <saml:Action>read</saml:Action>
</saml:AuthzDecisionStatement>
```

## JWTとの違い
同じくToken/File方式の認証方法で、ほかに[[JWT]]もある。
JWTはSAMLと比べて、長さが短く、Web APIなどの認証によく使われるが、SAMLは企業等のアクセスコントロールによく使用される。