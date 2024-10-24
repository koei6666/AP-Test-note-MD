公開鍵は、第三者機関により、本人と一致し正当なものであることを証明する必要がある。
そのために利用されるのは、PKIである。PKIは公開鍵暗号を利用した証明書の作成、管理、格納、廃棄に必要な方式である。

### 認証局(CA Certificate Authority)
PKIではCAのデジタル署名をつけた証明書を発行し、ある公開鍵の真正性を証明する。
これを証明する証明書はデジタル証明書（公開鍵証明書）と呼ばれる。
申請の流れ：

-　申請者がキーペアを作成し、うちの公開鍵と身分証明などをCAに提出し、証明を依頼する
- 　CAが申請書類に基づき、公開鍵の所有者の本人性を審査し、問題なければデジタル証明書を発行する

認証されたデジタル証明書と公開鍵には、CAのデジタル署名が付与される
受信者には、送信者の秘密鍵で暗号化した文書および、CAに認証された公開鍵を送信する。

公的証明書が不要で、社内ネットワークなどの場合、社内のサーバー上にプライベートCAを構築することもできる。

認証局に申請する**証明書署名要求**(CSR:Certificate Signing Request)とは、申請した情報(表1の識別名情報)と公開鍵をもとに認証局に対して公開鍵証明書の発行を依頼するメッセージです。要求が成功した場合、申請した情報に認証局のディジタル署名が付された公開鍵証明書が送り返され、証明書として有効状態になります。

### CP&CPS
CP(Certificate Policy):証明書の目的や利用用途を定めた規定
CPS(Certificate Practice Statement):CA認証業務に関する規定
これらの規定を対外的に公開しており、認証の利用者に対して、信頼性や安全性などを評価できるようにしている。

### デジタル証明書の失効情報
デジタル証明書が有効期間内、以下の理由で執行になることがある：
- 秘密鍵が紛失し、第三者に悪用されることが予測される場合
- 証明書に記載した事項に変更がある場合
- 規定違反行為が判明された場合
  など

失効になったデジタル証明書の情報(==シリアル番号と失効日時==)をCRL(Certificate Revocation List)に登録する。

デジタル証明書の有効性チェックは以下2つのモデルがある：
- CRLモデル：CAがCRLを定期的に公開し、利用者がCRLを参照し、デジタル証明書の有効性を確認する
- OCSPモデル：Online Certificate Status Protocol,利用者（OCSPクライアント)デジタル証明書のシリアルなどを、失効情報を保持しているOCSPサーバに問い合わせして、その応答でリアルタイムにデジタル証明書の有効性を確認する。

### ディジタル証明書

ディジタル証明書の内容はディジタルデータなので様々な媒体に格納することができます。  
個人認証用のディジタル証明書は電子証明書と呼ばれ、公的認証サービスなどを利用すると電子証明書が記録されたICカードの発行をうけることができます。

ディジタル証明書では、証明書が有効なサーバ(コモンネーム)を記述するのにIPアドレスだけでなくFQDNも使用できます。FQDNで指定した場合にはIPアドレスが変わっても再発行の必要はありません。

#### **fully qualified domain name** (**FQDN**) 
sometimes also referred to as an _absolute domain name_, is a domain name that specifies its exact location in the tree hierarchy of the Domain Name System (DNS). It specifies all domain levels, including the top-level domain and the root zone. A fully qualified domain name is distinguished by its lack of ambiguity in terms of DNS zone location in the hierarchy of DNS labels: it can be interpreted only in one way.
For example, a website which domain is "a.com", and its FQDN should be "\www.a.com", in which, "www" is the host name, and "a.com" is the domain name,FQDN is a combination of both.

#### **コモンネーム**(CN:Common Name)
サーバ証明書に含まれる登録情報で、証明書が有効なFQDN、またはそのIPアドレスが格納される項目です。クライアント側ではアクセスしたURLのドメイン名と証明書のコモンネームを比較することで証明書の正当性を検証します。また本文中にも「コモンネーム(SSL接続するサイトのFQDN)」と説明が記載されています。  
SSL/TLS通信の対象URLが "\https://www.a.co.jp/member" の場合、このURLのFQDN(ホスト部＋ドメイン部)にあたる "\www.a.co.jp" がコモンネームになります。

[[Certificate Transparency]]