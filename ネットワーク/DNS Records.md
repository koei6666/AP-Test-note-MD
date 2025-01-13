## Recordsの構成
DNS Recordsは下記の構成でDNSサーバに記述される
`NAME    TTL(Optional)    CLASS    TYPE    DATA`

- `NAME`
  該当Recordが所属するドメイン名を記述する、例えば`example.com`の名前を解決するrecordであれば、
  `example.com. 3600 IN A 192.0.2.1`

- `TTL`
  recordが存続する時間を指定する、デフォルトでは設定されているのであれば省略可能

- `CLASS`
  recordのclassを示す、種類は`IN`(インターネット)、`CH`(Chaos Network)、`HS`(Hesiod)などがありますが、現代のネットワーク環境では`IN`以外のクラスが出現することが少なく、一部のDNSツールではこの項目の省略も許可している。

- `TYPE`
  DNS Recordのタイプ[[DNS Records#Records Type]]

- `DATA`
  DNS Recordのデータ、データのタイプの違いによって構成は異なるが、例としてAタイプのデータはIPv4のIPアドレス
### Records Type
- A (Address)
  ドメインネームに紐づくIPv4アドレス
  `exmaple.com IN A 192.0.2.1`

- AAAA(IPv6 Address)
  ドメインネームに紐づくIPv6アドレス
  `example.com. IN AAAA 2001:db8::1`

- CNAME(Canonical Name)
  ドメインネームの別名を紐づく
  `www.example.com. IN CNAME example.com.`

- TXT(Text)
  任意のText情報を記録できるレコード
  [[SPF]]とでメイン認証の目的によく使用される
  `example.com. IN TXT "v=spf1 include:_spf.example.com ~all"`

- NS(Name Server)
  ドメインの権威DNSサーバを指定する
  `example.com. IN NS ns1.example.com.`
  このRecordはゾーンの境界線で、前のゾーンは前のRecordまでで終了となり、（他のDNSサーバが管理する）新しいゾーンはこのRecordから始まるを示している。

- SOA(Start Of Authority)
  Primary name server,管理者Email, シリアル、更新などを管理する複数のタイマー情報を記録している。
  `example.com. IN SOA ns1.example.com. admin.example.com. ( 2023121401 ; serial 3600 ; refresh 1800 ; retry 604800 ; expire 86400 ; minimum TTL )`
  SOAはドメインの詳細情報を記録するRecordで、[[DNS Records#ゾーン|ゾーン]]の開始を示している。
  内容の構成は以下：
  First line: `example.com. IN SOA ns1.example.com. admin.example.com.`
	- `example.com.` is the domain name
	- `IN` is the class (Internet)
	- `SOA` is the record type
	- `ns1.example.com.` is the primary DNS server for this zone
	- `admin.example.com.` is the email address of the administrator (with @ replaced by .)
	
	　The numbers in parentheses are timing parameters that control how DNS servers interact with your domain:
	
	1. Serial Number (2024011501):
	    - This is like a version number for your DNS zone
	    - Format is usually YYYYMMDDNN (year, month, day, revision number)
	    - Must be increased every time you update your zone file
	    - Secondary DNS servers check this to know if they need to update their copy
	2. Refresh (7200 seconds):
	    - How often secondary DNS servers should check for zone updates
	    - In this example, they'll check every 2 hours
	3. Retry (3600 seconds):
	    - If a refresh attempt fails, wait this long before trying again
	    - Here, it will retry every hour after a failed attempt
	4. Expire (1209600 seconds):
	    - If secondary servers can't reach the primary server for this long, they should stop answering queries
	    - This example shows 2 weeks, preventing outdated data from being served indefinitely
	5. Minimum TTL (86400 seconds):
	    - The minimum time that negative responses (like "domain doesn't exist") should be cached
	    - Here set to 24 hours

- PTR(Pointer)
  通常のDNS recordでは、ドメインからIPアドレスの関連をしているに対して、PTRはIPアドレスからドメインネームを関連をつけている。
  このプロセスは、reverse DNS lookup　あるいは　rDNS　と呼ばれている。
  `15.2.0.192.in-addr.arpa.    IN    PTR    myserver.example.com.` 
  DNSは通常のドメインネームを解読時では、小さい単位(左)から大きい単位（右）の順に解読するので、IPアドレスは左が大きく、右のセグメントに行くにつれて小さくなるので、PTR recordのIPアドレスは末尾から先頭に逆転された状態で記載される。
  PTRは通常このようなシーンで利用される：
  　1. メールアドレスの検証:受信したメールの送信IPアドレスは、DNSサーバに登録されたPTR Recordに存在するかどうかを検証する、存在しない場合は、信用されていないメールサーバからの送信と認識され、スパムと判断されることがある。
  　2. ログとTroubleshooting: ログに表示されるIPアドレスのドメインネームを表示
  　3. Access control: Access control systemでは、PTRと通常のDNS LookupでIPアドレスが紐付けするドメインネームを検証した上、ドメインネームからIPアドレスを検索し照合する"Forward-Confirmed reverse DNS FCrDNS"を実施する。

- SRV(service)
  (VoIPや、XMPPなどの）サービスの所在を示す
  `_sip._tcp.example.com. IN SRV 10 60 5060 sip.example.com.`

- MX(Mail Exchange)
  メール受信サーバーを指定する、優先度のパラメーターで、複数のメールサーバーを順位づけすることができる。(優先順位が小さいほど優先)
```
example.com.    IN    MX    10    primary-mail.example.com.
example.com.    IN    MX    20    backup1-mail.example.com.
example.com.    IN    MX    30    backup2-mail.example.com.

```

- CAA(Certificate Authority Authorization)
  ドメインの証明書を発行できるCAを指定するレコード、認可されていないCAが不正にがいとうドメインの証明書を発行し、成り済ましすることを防止する。
  `example.com.    IN    CAA    0    issue    "letsencrypt.org"`
  要素：
  ルールポリシーフラグ: 0/128
  許可タイプタグ: `issue`, `issuewild`, `iodef`
  - `issue`:通常の証明書の発行を許可する
  - `issuewild`: wildcard証明書の発行を許可する(\*.example.com)
  - `iodef`: 発行ポリシー違反発生時の報告先メールアドレス（例：`example.com.    IN    CAA    0    iodef    "mailto:security@example.com"`)[[PKI#発行ポリシー]]
  
  値: CAのドメインを示す値（ドメインネーム)
 ^ec373f
- DKIM(Domain Keys Identified Mail)
  ![[DKIM]]
- DMARC(Domain-based Message Authentication)
  ![[DMARC]]
- DS(Delegation Signer)
  DNSSECの一部、DNSKEYのハッシュを格納
  `example.com. IN DS 12345 13 2 123456789abcdef...`

## ゾーン