---
分類: プロトコル
---
DomainKeys Identified Mail
## 概要
PKIを利用し、受信したメールは実際にメールに記載された送信者から送られたかどうかを検証し、メールが送信される間に改竄されるかどうかを確認するプロトコル

## 仕組み
1. 送信側
   送信メールサーバが、メールのヘッダーとボディを元に、メールサーバの暗号鍵で電子署名を作成し、メールヘッダーに添付する

2. 受信側
   受信メールサーバが送信DNSサーバのDNSレコードから公開鍵を取得し、送信者の電子署名を検証する。

## DKIM in DNS
```DNS
selector._domainkey.example.com IN TXT "v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4..."
```

