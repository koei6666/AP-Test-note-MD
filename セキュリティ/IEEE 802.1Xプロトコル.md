
## **RADIUS認証システム**
アクセスサーバは認証情報を蓄積している上、アクセスを外部に開いているので、万が一クラッキング時の被害が大きいため、RADIUS(Remote Authentication Dial-In User Service)サーバを設立し、アクセスサーバ（RADIUSクライアント）と認証サーバ（RADIUSサーバ）を分離することで脆弱性を緩和する。
ユーザー名とパスワード、および認証業務を分離することで、認証情報などを一元に保管することができるのは利点

## IEEE 802.1X
802.1XはRADIUSを認証の仕組みとして採用し、認証プロトコルをPPPの拡張したEAP(Extended Authentication Protocol)を使用する。
[[EAP]]はクライアント証明書方式のEAP-TLSや、MD5を使ったチャレンジレスポンス方式のEAP-MD5というバリエーションがある。

### Authenticator オーセンティケータ
SwitchやAPなどの機器、ユーザのアクセス要求を受けて、ユーザ認証プロセスを発動する。

### Authentication Server 認証サーバ
RADIUSサーバなどの、認証情報を管理するデータベースと、Authenticatorからのユーザ認証依頼を判断し、許可と拒否の判断をAuthenticatorに返信する

### Supplicant サプリカント/クライアント
ユーザデバイス

### 認証の流れ
![[Pasted image 20241030225530.png]]

1. ユーザがAPなどのAuthenticatorに対して、アクセスをする
2. Authenticatorがユーザに対して、（使用するメカニズムに合わせて）証明書などを要求
3. ユーザがAuthenticatorに対して証明書を提出、Authenticatorが受領後、Authentication Serverに転送し、認証を依頼する
4. \*必要に応じて、Auth Serverから追加の証明提出を要請し、Authenticatorがそれらをユーザに中継する
5. Auth Serverがアクセスの許可と拒否を判断した後、判断をAuthenticatorに返信し、Authenticatorがユーザに認証成功または失敗の連絡をする。