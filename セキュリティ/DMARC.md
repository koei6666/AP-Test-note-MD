---
分類: プロトコル
---
Domain-based Message Authentication, Reporting and Conformance
## 概要
DMARCは[[SPF]]と[[DKIM]]を元のメール検証プロトコール。DMARCとSPF、DKIMの違いは、DMARCはSPFやDKIMと連携し、検証後の処置を支持する役割を担う。
DMARCの役割1
ポリシーの適用:サーバに、検証に失敗したメールに対するの処置（なし（監視のみ）、隔離、拒否）を指示。

DMARCの役割2
From:の送信者が実際の送信者と一致するかどうかをDKIMとSPFを通して検証。
判断モード：Strict(ドメインの完全一致のみ許可)
　　　　　　Relaxed(サブドメインも許可する)
　　　　　　
DMARCの役割3
ドメインサーバの送信したメールの履歴、検証の成功、失敗記録、実施した措置の記録

## DMARC DNS Record
```DNS
_dmarc.example.com IN TXT "v=DMARC1; p=reject; rua=mailto:reports@example.com"
```
- `p=reject` is the policy
- `rua=` specifies where to send aggregate reports