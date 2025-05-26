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

## DMARC Policy Framework: Technical Specification and Implementation

### DMARC Protocol Definition

Domain-based Message Authentication, Reporting, and Conformance (DMARC) is an email authentication protocol that builds upon SPF (Sender Policy Framework) and DKIM (DomainKeys Identified Mail) to provide domain-level protection against email spoofing. DMARC enables domain owners to publish policies via DNS TXT records, instructing receiving mail servers on authentication handling and providing visibility through aggregate and forensic reporting mechanisms.

### DMARC Policy Options

#### 1. Policy: None (p=none)

**Technical Specification**: Monitoring mode with no enforcement action

**Operational Characteristics**:

- Authentication failures do not impact message delivery
- Full visibility through aggregate (RUA) and forensic (RUF) reports
- Allows baseline establishment without disrupting legitimate email flows
- Enables identification of authorized sending sources

**Security Implications**:

- No protection against domain spoofing
- Provides intelligence gathering capability
- Essential for initial deployment phases
- Risk-free implementation for complex email ecosystems

**Use Cases**:

- Initial DMARC deployment
- Complex multi-vendor email environments
- Organizations requiring extensive third-party sender mapping
- Pre-production testing phases

#### 2. Policy: Quarantine (p=quarantine)

**Technical Specification**: Suspicious message isolation

**Operational Characteristics**:

- Failed authentication results in spam/junk folder delivery
- Maintains message accessibility for end-user review
- Reduces inbox visibility of potentially fraudulent messages
- Partial protection while maintaining recovery options

**Security Implications**:

- Moderate protection against impersonation
- Reduces click-through rates on fraudulent messages
- Maintains audit trail for investigation
- Balances security with operational flexibility

**Use Cases**:

- Transitional phase before full enforcement
- Organizations with moderate risk tolerance
- Environments with occasional legitimate authentication failures
- Testing stricter policies with safety net

#### 3. Policy: Reject (p=reject)

**Technical Specification**: Full enforcement with message rejection

**Operational Characteristics**:

- Authentication failures result in SMTP-level rejection
- Messages never reach recipient infrastructure
- Provides definitive protection against spoofing
- Generates NDR (Non-Delivery Receipt) to purported sender

**Security Implications**:

- Maximum protection against domain impersonation
- Eliminates spoofed messages from mail flow
- Potential for false positives impacting legitimate mail
- Requires comprehensive authorized sender inventory

**Use Cases**:

- Mature DMARC implementations
- High-security environments
- Organizations with complete sender control
- Domains not used for email transmission

### DMARC Record Syntax and Components

#### Core Policy Tags

**Version Tag (v=DMARC1)**

- Required tag indicating DMARC version
- Must be the first tag in the record
- Currently only "DMARC1" is valid

**Policy Tag (p=)**

- Defines action for authentication failures
- Required tag with three valid values: none, quarantine, reject

**Subdomain Policy (sp=)**

- Optional tag for subdomain-specific policies
- Inherits from parent domain if not specified
- Same values as primary policy tag

#### Reporting Configuration

**Aggregate Reporting URI (rua=)**

- Specifies destination for aggregate reports
- Supports multiple URIs (comma-separated)
- Format: `mailto:address@domain.com`
- Enables cross-domain reporting with verification

**Forensic Reporting URI (ruf=)**

- Destination for forensic (failure) reports
- Privacy implications due to message content exposure
- Format identical to RUA
- Many receivers don't send forensic reports due to privacy concerns

#### Alignment and Sampling Controls

**DKIM Alignment (adkim=)**

- Controls DKIM identifier alignment strictness
- Values: 'r' (relaxed) or 's' (strict)
- Relaxed: Organizational domain match sufficient
- Strict: Exact domain match required

**SPF Alignment (aspf=)**

- Controls SPF identifier alignment strictness
- Values: 'r' (relaxed) or 's' (strict)
- Alignment behavior identical to DKIM

**Percentage Tag (pct=)**

- Gradual policy rollout mechanism
- Integer value 0-100
- Applies to quarantine and reject policies
- Enables risk-managed deployment

**Failure Reporting Options (fo=)**

- Controls forensic report generation triggers
- Values:
    - '0': Generate report if all mechanisms fail (default)
    - '1': Generate report if any mechanism fails
    - 'd': Generate report on DKIM failure
    - 's': Generate report on SPF failure

### Advanced DMARC Deployment Strategies

#### Progressive Deployment Model

**Phase 1: Discovery (p=none, pct=100)**

```
v=DMARC1; p=none; rua=mailto:dmarc-reports@domain.com; ruf=mailto:forensics@domain.com; fo=1
```

- Complete visibility without enforcement
- 4-8 week minimum observation period
- Identify all legitimate sending sources

**Phase 2: Gradual Quarantine**

```
v=DMARC1; p=quarantine; pct=10; rua=mailto:dmarc-reports@domain.com
```

- Incremental percentage increases (10% → 25% → 50% → 100%)
- Monitor false positive rates
- Adjust authorized sender configurations

**Phase 3: Enforcement Transition**

```
v=DMARC1; p=reject; pct=10; rua=mailto:dmarc-reports@domain.com
```

- Gradual migration to reject policy
- Careful monitoring of business impact
- Progressive percentage increases

**Phase 4: Full Protection**

```
v=DMARC1; p=reject; rua=mailto:dmarc-reports@domain.com; aspf=s; adkim=s
```

- Complete enforcement
- Strict alignment for maximum security
- Ongoing monitoring and maintenance

### Security Considerations and Best Practices

#### Report Processing Security

- Implement automated report parsing
- Validate report authenticity
- Monitor for report manipulation attempts
- Secure storage of forensic reports (PII considerations)

#### Subdomain Strategy

- Evaluate subdomain email usage patterns
- Consider stricter subdomain policies for unused domains
- Implement explicit sp= tags for clarity
- Regular subdomain audit procedures

#### Third-Party Sender Management

- Maintain authorized sender inventory
- Document SPF include authorizations
- Track DKIM signing domains
- Regular third-party security assessments

#### Monitoring and Alerting

- Automated anomaly detection in reports
- Threshold-based alerting for authentication failures
- Trend analysis for policy violations
- Integration with SIEM platforms

### Common Implementation Pitfalls

1. **Insufficient Preparation Time**: Rushing to enforcement without complete sender identification
2. **Ignoring Subdomain Implications**: Failing to consider subdomain email patterns
3. **Report Analysis Neglect**: Not implementing automated report processing
4. **Missing Alignment Configuration**: Defaulting to relaxed alignment unnecessarily
5. **Incomplete Stakeholder Communication**: Not informing third-party senders of policy changes
6. **Forensic Report Privacy**: Not considering privacy implications of RUF implementation

### Operational Metrics

- **Authentication Pass Rates**: SPF and DKIM success percentages
- **Policy Action Distribution**: Messages subjected to none/quarantine/reject
- **Alignment Success Rates**: Strict vs. relaxed alignment impacts
- **Geographic Threat Patterns**: Source IP analysis from reports
- **Vendor Compliance Rates**: Third-party sender authentication success

This comprehensive DMARC policy framework provides the technical foundation for implementing robust email authentication while balancing security requirements with operational needs.