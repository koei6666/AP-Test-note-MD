URL(Uniform Resource Locator)
```
protocol://subdomain.domain.tld:port/path?query_parameters#fragment
```
## 構成:

### プロトコル/スキーム　Protocol/Scheme
`protocol:`
`//`より前の部分、URLのプロトコールを示す
例：
`http://`
`ldap://`

### ドメイン/ホスト
subdomain.<strong><u>domian</u></strong>.tld
サーバの所在

#### サブドメイン
<u>subdomain</u>.domain.tld
同じドメイン内の異なるサービス/環境/地理的な環境を分類する役割
例：
異なるサービス
```
mail.google.com
docs.google.com
```
異なる環境
```
dev.server.com
prod.server.com
```
異なる地理位置
```
us.domain.com
eu.domain.com
```

#### TLD Top Level Domain
subdomain.domain.<u>tld</u>
ドメインの目的または地理位置を示す
Common types:

1. Generic TLDs:
    - .com (commercial)
    - .org (organization)
    - .edu (education)
    - .gov (government)
    - .net (network)
2. Country Code TLDs:
    - .uk (United Kingdom)
    - .de (Germany)
    - .jp (Japan)
    - .ca (Canada)

### ポート　Port
Port指定が必要な場合には下記のように記載する
```
sample.sample.com:8080
```

### Path
ドメインの`/`後に続く内容アクセス先サーバ上の位置を示す

### Query parameter
アクセス先にパラメーターが必要な場合に使用するオプションのパラメーター
URLの`?`の後に続き、key=valueの形で記述される、複数のパラメーターがある場合では、`&`で連結する
`?name=john&age=25`

#### 他のスキームのQuery
HTTPのURLでは、明確に`?`でqueryを示しているが、その他のプロトコルでは表示フォーマットが異なることがある、たとえば、LDAPの場合では、明確な`?`を示してなく、ドメインの後に続く内容をqueryとして使うことがある
`ldap://a8.b8.c8.d8/JExp`
このような最小限のデータしか渡さないqueryの仕方は、ldap攻撃では多く見られ、特徴的なqueryと言えるであろう

通常のladp URLフォマート
`ldap://host:port/dn?attributes?scope?filter?extensions`
### Fragment/Anchor
アクセス先の内容のセクションを指定する場合に使用するオプションのパラメーター、
`#section1`

### URL Encoding
URLエンコードするときでは、安全ではない符号(`/`など)を表示するために、先頭に%をつけて、後にその符号のASCII値の形で表示する。
例えば、`/`を表示する場合では、`%2f`と表示する