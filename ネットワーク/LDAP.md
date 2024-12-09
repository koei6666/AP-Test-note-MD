
Lightweight Directory Access Protocol

## 概要
LDAPはIPを経由して、ディレクトリインフォメーションサービス(Directory Infomation Service)へのアクセス機能を提供するプロトコル。
LDAPは中央集権的な、ユーザー名（ID)とパスワードダイジェストを補完する場所を提供し、ユーザ認証のサービスを提供する。
そのほかに、木構造で、組織情報を管理し、例えば組織メンバー情報などを応答する機能を提供する。

TCP:389

## 主要な機能
- User authentication
- Storing contact information for employees in an organization
- Storing network device configurations
- Single sign-on (SSO) systems

## 特徴
- 軽量
- プラットフォームに依存しない
- 拡張可能

### 製品の代表例
Microsoft Active Directory, OpenLDAP, Apache Directory Server

## A Sample response of LDAP request
```LDAP
# LDAP Query:
(&(objectClass=person)(uid=jsmith))

# LDAP Response:
dn: uid=jsmith,ou=People,dc=example,dc=com
objectClass: top
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
cn: John Smith
sn: Smith
givenName: John
uid: jsmith
mail: jsmith@example.com
telephoneNumber: +1 555 123 4567
title: Senior Software Engineer
department: Engineering
manager: uid=mjohnson,ou=People,dc=example,dc=com
roomNumber: 4B-222
employeeNumber: 1234567
```

## 安全性の懸念
LDAPは、管理している情報を、その情報を要求者に送信するプロトコルのため、インジェクション被害や、リフレクション攻撃に利用されることがある。

例えば、LDAPサーバに悪意のあるリンクを埋め込み、要求者に返信することで、そのリンクに踏ませるようなレスポンスの例は以下になる:
```LDAP
# Malicious LDAP Response
dn: uid=user,ou=People,dc=example,dc=com
objectClass: top
objectClass: person
objectClass: organizationalPerson
uid: user
cn: Regular User
sn: User
title: Employee
description: http://malicious-site.com/exploit
jpegPhoto: http://internal-server/sensitive-data
```

#### 対策
インジェクションを受けないために、LDAPへの入力をエスケープする上、LDAPからのレスポンスもエスケープし、その中に含まれるリンクを自動的にアクセスしないように設置することが重要。
また、設定できる範囲にあるLDAPサーバに関しては、要求によって返信するパラメーターをここに設定し、必要以上にデータを返信しないようにする。