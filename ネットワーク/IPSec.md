
**IPsec**は、IP(Internet Protocol)を拡張してセキュリティを高めるプロトコルで、改ざんの検知，通信データの暗号化，送信元の認証などの機能をOSI基本参照モデルのネットワーク層レベル(TCP/IPモデルではインターネット層)で提供します。
- IKE(Internet Key Exchange)は、通信相手の認証と鍵情報の交換を安全に行うプロトコルです。
IPsecを用いれば上層のアプリケーションに依存せずに暗号化通信を行えるため、VPNの構築に利用されます。  
IPsecはプロトコル群の総称であり、認証、暗号化、鍵交換などの複数のプロトコルを含みます。そのうち、
- 認証を担うプロトコルが**AH**(Authentication Header)、
- 認証と暗号化を担うプロトコルが**ESP**(Encapsulated Security Payload)です。