---
分類: 認証方法
---

## 概要
OAuthは、アプリケーションなどが、他のアプリケーションなどの保有しているデータやサービスにアクセスしたい時に、限定的にそのリソースへのアクセスを許可する認証フレームワークである。
認証は、アクセス先のアプリケーションで完了した後、そのアクセス権(Token)をアクセス元に送付するフローになるので、認証情報をアクセス元のアプリに渡すことなく完了することができる。

## 仕組み
例えば、ある写真印刷サービスが、Google Cloud上の写真を印刷できるとする。
信用性の低い写真印刷サービスにGoogleの認証情報を渡して印刷を代行してもらうのはリスクが大きく、その一方、例えばGoogleアカウントに登録し、アカウント全般のアクセス権限を委任するのも必要がなく、リスクが大きいので、OAuthを利用し、Google Cloud、または、特定のコンテンツだけ、限定した期間中にそのサービスからのアクセスを承認することができる。
下サンプルコードのように`Scope`に`read`と`write`の権限を、`expires_in`の時間内にアクセス可と示している、下記のような簡単な権限付与のほかに、個別なリソースに対する権限付与は：
```http
// Read-only access to Google Drive files
scope=https://www.googleapis.com/auth/drive.readonly

// Read and write access to calendar
scope=https://www.googleapis.com/auth/calendar
```

## Roles in OAuth Process
1. Resource Owner(User):
   OAuthは根本的には、ユーザーが第三者承認機関を介して、自分のリソースを別のサービスにアクセス権を付与する流れになるので、ユーザーはリソースのオーナーになる。
2. Client(the application):
   ユーザーのリソースにアクセスしようとするクライアント
3. Authorization Server
   ユーザー（リソースオーナー）を認証し、リソースサーバーにある、リソースオーナーの資源へのアクセス権（アクセストークンを発行する役割）
   例えば:`accounts.google.com`
4. Resource Server
   リソースを保管するサーバ、Authorization Serverにより発行されたアクセストークンを検証し、受付する。

![[OAuthのパケット#アクセストークン要求に対するレスポンス]]

[[セキュリティ/OAuthのパケット|OAuthのパケット]]
[[セキュリティ/PKCE|PKCE]]

![[OAuth　コードフロー]]



