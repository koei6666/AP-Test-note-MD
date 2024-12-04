サーバから提出された証明書に対して、クライアントは証明書の有効性をチェックする、そのチェックはの内容は：
1. Domain Name Validation
   サーバのドメインネームが証明書のドメインネームとマッチするかどうかを確認する。具体的には証明書の[[X.509 Certificate Structure|Subject Alternative Name(subjectAltName) Extension]]からdNSNameを確認し、その一致性を検証する。
   Subject Alternative Nameが存在しない場合は、legacyのcommonNameフィールドを確認し検証する。

>[!Note] SAN Extension Example
> 
```
# OpenSSL text format (certificate dump)
X509v3 Subject Alternative Name: 
    DNS:example.com
    DNS:www.example.com
    DNS:blog.example.com
    DNS:*.example.com
    email:admin@example.com
    IP:192.168.1.1

# ASN.1 notation
subjectAltName EXTENSION ::= {
    SYNTAX SubjectAltName
    IDENTIFIED BY id-ce-subjectAltName
}

SubjectAltName ::= SEQUENCE SIZE (1..MAX) OF GeneralName

GeneralName ::= CHOICE {
    dNSName                [2] IA5String,
    iPAddress              [7] OCTET STRING,
    rfc822Name             [1] IA5String,
    directoryName          [4] Name,
    uniformResourceIdentifier [6] IA5String,
    registeredID           [8] OBJECT IDENTIFIER
}

# Example in OpenSSL configuration file (for certificate creation)
[alt_names]
DNS.1 = example.com
DNS.2 = www.example.com
DNS.3 = *.example.com
IP.1 = 192.168.1.1
email.1 = admin@example.com

# DER encoding (hexadecimal representation)
30 4C # SEQUENCE
  A0 4A # [0] {
    30 48 # SEQUENCE
      82 0E # [2] DNS name
        65 78 61 6D 70 6C 65 2E 63 6F 6D # "example.com"
      82 12 # [2] DNS name
        77 77 77 2E 65 78 61 6D 70 6C 65 2E 63 6F 6D # "www.example.com"
```

2. Certificate Chain Validation
   Domain Name検証完了後、証明書が信頼されたCAから発行されたものかどうかを検証する、続いて、証明書の期限と有効性(Revoked or not)を検証する