SMTPコマンドはクライアントとサーバの間でrequest-response方式で情報をやり取りし、以下のコマンドが用いられる

## HELO/EHLO(extended SMTP)
クライアントがSMTPサーバに自分のドメインネームを提示するコマンド
```
Client: EHLO mail.example.com
Server: 250 Hello mail.example.com, pleased to meet you
```

## MAIL FROM
新規メールトランザクションを開始する、送信者のアドレスを提示
```
Client: MAIL FROM:<sender@example.com>
Server: 250 Sender ok
```

## RCPT TO
送信者を指定、複数の送信者を指定可能
```
Client: RCPT TO:<recipient@example.com>
Server: 250 Recipient ok
```

## DATA
メールの本文を送信
```
Client: DATA
Server: 354 Enter mail, end with "." on a line by itself
Client: From: sender@example.com
Subject: Hello World
...
.
Server: 250 Message accepted for delivery
```

## QUIT
セッションを終了
```
Client: QUIT
Server: 221 Closing connection
```

## その他のコマンド
AUTH: Allows authentication of the sending client 
STARTTLS: Initiates secure (encrypted) communication 
VRFY: Verifies if a mailbox exists 
EXPN: Expands a mailing list 
HELP: Requests help information from the server

## サーバレスポンス
- 2xx indicates success
- 3xx indicates additional information needed
- 4xx indicates temporary failure
- 5xx indicates permanent failure

## SMTP Transaction
```
CLIENT: EHLO client.example.com
SERVER: 250-mail.example.com Hello client.example.com
SERVER: 250-SIZE 14680064
SERVER: 250 HELP

CLIENT: MAIL FROM:<sender@example.com>
SERVER: 250 OK

CLIENT: RCPT TO:<recipient@example.com>
SERVER: 250 OK

CLIENT: DATA
SERVER: 354 Start mail input; end with <CRLF>.<CRLF>

CLIENT: From: "Sender Name" <sender@example.com>
CLIENT: To: "Recipient Name" <recipient@example.com>
CLIENT: Subject: Test Email
CLIENT: 
CLIENT: This is a test email.
CLIENT: .
SERVER: 250 OK: queued as 12345

CLIENT: QUIT
SERVER: 221 Bye
```