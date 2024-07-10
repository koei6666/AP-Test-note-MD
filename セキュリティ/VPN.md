Virtual Private Network
通信業者の広域IP網などに使うVPNをIP-VPNという。

### VPNの2モード
- トランスポートモード：通信を行う端末が直接データの暗号化と復号を行う。暗号化されるのはペイロード（ヘッダを除くデータの部分）のみで、送り先IPが盗聴される可能性がある。
- トンネルモード：VPNゲートウェイを拠点に、間の通信を暗号化する。送信側のゲートウェイが、送り先のIPアドレスをIPsecで暗号化（カプセル化）してから、受信側ゲートウェイのIPアドレスヘッダーをつけて送信する（トンネリング手法）。受信側が受信してから暗号化された宛先IPを復号し、送付先に転送する。

#### IPsec認証ヘッダ
IPsecで通信する場合、通常のIPでは利用しないパラメータを交換するので、この情報がIPヘッダに入り切らない場合は、別のヘッダを用意する。
認証だけ行う場合は、認証ヘッダ（AH:Authentication Header),認証と暗号化を行う場合は暗号ペイロード（ESP:Encapsulating Security Payload)を使う。

IPsec通信を始める前では、IKE(Internet Key Exchange)というフェーズで暗号化方式と鍵交換を行う。IPsecがIKEが終了してから、決まった暗号化方式と鍵でデータを暗号化し送信する。

#### その他のVPNプロトコル
- L2TP:Layer 2 Tunneling Protocol
- [[PPTP|PPTP]]:Point to Point Tunneling Protocol

[[SSL-VPN]]

---
- AH Authentication Header:IPsecプロトコルを構成する技術の一つで、IPパケットの送信元の認証やパケットが改ざんされていないか確認します。暗号化は行いません。