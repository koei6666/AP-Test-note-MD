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

## SAML Request
### GET Request
`https://idp.example.com/SAML2/SSO?SAMLRequest=fZJvc6owEMY%2FDTOfISAL1hHQ...`
![[HTTP#^0e0ec9]]

## SAML Assertion
SAML AssertionはIdPとSPの間に交換される、ユーザ情報や承認情報が記載されている書類である。

SAML Assertionは以下の部分からなる:
署名、認証条件（認証期間、認証対象URL）、サブジェクト（認証対象、検証ルールの提示、元のリクエストへのリンク、Replay attack対策)
```xml
<?xml version="1.0" encoding="UTF-8"?> <saml2:Assertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion"ID="_93af655219464fb403b34436cfb0c5cb1d9a5502" IssueInstant="2024-11-16T10:00:00Z" Version="2.0"><saml2:Issuer>https://idp.example.com</saml2:Issuer> 
<!-- Digital Signature --> 
<ds:Signaturexmlns:ds="http://www.w3.org/2000/09/xmldsig#"> <ds:SignedInfo> <ds:CanonicalizationMethodAlgorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/> <ds:SignatureMethodAlgorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/> <ds:ReferenceURI="#_93af655219464fb403b34436cfb0c5cb1d9a5502"> <ds:Transforms> <ds:TransformAlgorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/> <ds:TransformAlgorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/> </ds:Transforms> <ds:DigestMethodAlgorithm="http://www.w3.org/2001/04/xmlenc#sha256"/> <ds:DigestValue>digest_value_here</ds:DigestValue></ds:Reference> </ds:SignedInfo> <ds:SignatureValue>signature_value_here</ds:SignatureValue> <ds:KeyInfo><ds:X509Data> <ds:X509Certificate>certificate_data_here</ds:X509Certificate> </ds:X509Data> </ds:KeyInfo></ds:Signature>

<!-- Conditions --> 
<saml2:Conditions NotBefore="2024-11-16T10:00:00Z" NotOnOrAfter="2024-11-16T10:10:00Z"> <saml2:AudienceRestriction> <saml2:Audience>https://sp.example.com</saml2:Audience></saml2:AudienceRestriction> </saml2:Conditions> 

<!-- Subject --> 
<saml2:Subject> <saml2:NameIDFormat="urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress"> user@example.com </saml2:NameID><saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer"> <saml2:SubjectConfirmationDataNotOnOrAfter="2024-11-16T10:10:00Z" Recipient="https://sp.example.com/acs" InResponseTo="_abc123"/></saml2:SubjectConfirmation> </saml2:Subject>
```

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
2. Attribute Statement
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