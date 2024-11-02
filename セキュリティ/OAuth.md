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
![[OAuthのパケット#アクセストークン要求に対するレスポンス]]

[[セキュリティ/OAuthのパケット|OAuthのパケット]]
[[セキュリティ/PKCE|PKCE]]



