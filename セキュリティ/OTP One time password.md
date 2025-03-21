---
分類: 認証方法
---
## 概要
ユーザを認証するための、ランダムな、使い捨てパスワード。
OTPは通常では数字4~8桁の長さがあり、決まった時間中にだけ有効と設定されている（通常では5分から10分）

TOTPとHOTPには、サーバとクライアント間共有する暗号を基礎にパスワードを生成しているので、この2種類はシェアードシークレット(Shared Secret)が重要である。

### OTPの種類
#### TOTP(Time based OTP)
生成時の時間と、秘密鍵でパスワードを生成。
クライアントとサーバの時間でパスワードを生成するため、それぞれの時間が不一致の場合ではDriftという問題が発生する（時間不一致により認証不可）

#### HOTP(HMAC based OTP)
サーバとクライアント間で同期されているカウンターと秘密鍵でパスワードを生成。
一部の認証設定では、カウンターが一定の範囲内のものを認証する設定になっているので、オフラインでも認証できることが特徴。

#### SMS Based/ Email Based
サーバが生成したランダムな4~8桁の番号を、SMSあるいはメールでクライアントに送信する。
認証するため、サーバが生成した後、この番号をサーバに保存し、リンクしているユーザ名や、Timeout時間をサーバに記録する。