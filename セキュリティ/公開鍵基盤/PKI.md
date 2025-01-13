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

#### 発行ポリシー
CAがあるドメインに対する証明書発行時には、まずはDNS上そのドメインのCAA Recordを確認し、その証明書発行ポリシーを確認しなければいけない：
発行しようとするドメインにはCAA Recordが存在しなければ、そのドメインに対してすべてのCAが証明書を発行可能なので発行できる。
CAA Recordsが存在する場合は、そのリスト内になるCAのみが該当ドメインに対して証明書を発行することが許可される。
申請されたCAがリストにない場合は、CAA違反になりますので、CAはその発行依頼を拒否しなければいけない。
CAA違反は、上記の認可外CAの発行違反の他に: ^065014
1. Wildcard発行違反：
   CAAレコードではこのような記載がある場合：
   ```
   example.com. IN CAA 0 issue "digicert.com" 
   example.com. IN CAA 0 issuewild "none"
   ```
　　通常の証明書は, `digicert.com`というCAから発行は許可されるが、`*.example.com`のようなwildcard証明書の指定発行元はNoneに指定されているので、このドメインのwildcard証明書を発行するCAがあったら違反になる。

2. Subdomain発行違反
   CAA Recordでは、トップドメインとサブドメインが異なるCAを指定する場合があるので、トップドメインの指定するCAがサブドメインの証明書を発行したり、またはその逆の場合はCAA違反になる

3. Critical flag違反
   CAAレコードでは、`0:Non-Critical`と`128:Critial`の2種類のフラグで、CAA値に対するパラメーターへの制限を設定している。
   `example.com.    IN    CAA    0    issue    "digicert.com;validation=strict"`
   例えば上記recordには、値の後ろに`validation=strict`というパラメーターが続いている、これはdigicert.comの独自のパラメーターで、他のCAでは解読できない可能性がある、`0`と設定された場合は、このパラメーターを解読できなくても発行できるが、128と設定した場合は、このパラメータを解読できなければ発行が許可されなく、発行した場合はCAA違反になる。
#### CA役割の細分化
一般的に、証明書に署名発行するのはCAだが、CA内部の役割を細分化した時には以下の細かい分業がある
##### IA Issuing Authority
確認、承認後の証明書にデジタル署名を付与し、発行する業務を担う部署

##### RA Registration Authority
承認依頼の処理、承認依頼者の身分確認、承認後IAに発行依頼

##### PA Policy Authority 
CPを規定、PKIの運行を監視、Trust relationshipの策定

### CP&CPS
CP(Certificate Policy):証明書の目的や利用用途を定めた規定(PAによる規定)
CPS(Certificate Practice Statement):CA認証業務に関する規定(PCAによる規定、実施)
これらの規定を対外的に公開しており、認証の利用者に対して、信頼性や安全性などを評価できるようにしている。

### デジタル証明書の失効情報
デジタル証明書が有効期間内、以下の理由で執行になることがある：
- 秘密鍵が紛失し、第三者に悪用されることが予測される場合
- 証明書に記載した事項に変更がある場合
- 規定違反行為が判明された場合
  など

失効になったデジタル証明書の情報(==シリアル番号と失効日時==)をCRL(Certificate Revocation List)に登録する。

デジタル証明書の有効性チェックは以下2つのモデルがある：
- CRLモデル(リアルタイムではない）：CAがCRLを定期的に公開し、利用者がCRLを参照し、デジタル証明書の有効性を確認する ^2601b6
- OCSPモデル（リアルタイム）：Online Certificate Status Protocol,利用者（OCSPクライアント)デジタル証明書のシリアルなどを、失効情報を保持しているOCSPサーバに問い合わせして、その応答でリアルタイムにデジタル証明書の有効性を確認する。 ^e6620a

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