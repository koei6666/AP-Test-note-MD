### DNS
DNS（Domain Name System）は、インターネット上でドメイン名（例：`www.example.com`）とIPアドレス（例：`192.168.1.1`）を相互に変換するシステムです。基本的に、DNSはインターネット上の電話帳のようなものと考えることができます。

プロトコル：
UDP:53
一部のケースでは（DNSレスポンスが512バイトより大きい、ゾーン転送、安定性が重視される運用など）TCP:53を使用する ^0ea5e6

[[Why DNS uses UDP]]
#### **主な機能**

1. **名前解決（Name Resolution）**: ユーザーがウェブブラウザにURL（例：`www.google.com`）を入力すると、DNSはこのドメイン名を対応するIPアドレス（例：`172.217.22.14`）に変換します。
>**名前解決テストのコマンド nslookup**
>  `nslookup` is a network administration command-line tool available for many computer operating systems. It is used for querying the Domain Name System (DNS) to obtain domain name or IP address mapping or other specific DNS records.
> ` nslookup example.com`
```
Server:     [Name of your DNS server]
Address:    [IP address of your DNS server]
Non-authoritative answer:
Name:       example.com
Address:    93.184.216.34
```


1. **逆引き（Reverse Lookup）**: IPアドレスからドメイン名を取得することもできます。
2. **負荷分散（Load Balancing）**: 同一のドメイン名で複数のIPアドレスを管理し、名前の問い合わせが発生するたびに、応答するIPアドレスを順番に変えることで、トラフィックを分散することができます。
![[攻撃手段リスト#^43f4af|DNSのセキュリティ問題]]
![[攻撃手段リスト#^1db6aa|DNSポイズンニング対策DNSSEC]]
### **動作の手順**

1. **ユーザーのリクエスト**: ユーザーがウェブブラウザで`www.example.com`を開くように指示。
2. **ローカルキャッシュのチェック**: コンピュータは最初にローカルキャッシュをチェックして、以前に取得したIPアドレスが残っているかどうかを調べます。
3. **再帰的・反復的クエリ**: ローカルキャッシュに情報がない場合、DNSクエリがローカルDNSサーバーに送信されます。このサーバーは、必要ならば他のDNSサーバーに問い合わせて結果を得ます。
4. **応答**: 最終的にIPアドレスが見つかれば、それがユーザーのコンピュータに返され、ウェブブラウザはそのIPアドレスに接続します。

#### **DNSの種類**

- **TLD（Top-Level Domain）**: 最上位のドメイン。例：`.com`, `.org`, `.jp`
- **SLD（Second-Level Domain）**: TLDの下のレベル。例：`google` in `www.google.com`
- **サブドメイン（Subdomain）**: SLDまたは別のサブドメインの下に位置するドメイン。例：`www` in `www.google.com`

### **DNSのセキュリティ**

- **DNSSEC（Domain Name System Security Extensions）**: DNS情報の改ざんを防ぐためのセキュリティ拡張。 ^1dac9e


![[DNSサーバ]]


![[DNS Records]]

[[Dynamic DNS Update]]

![[スタブリゾルバ]]