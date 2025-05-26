---
分類: プロトコル
---

**SPF**(Sender Policy Framework)は、メールを送信しようとしてきたメールサーバの==IPアドレス情報を検証することで==、正規のサーバからのメール送信であるかどうか確認することができる技術です。受信メールサーバ側がメールの送信元==ドメイン==を管理するDNSサーバに問い合わせ、返された<u>IPアドレスが送信元メールサーバのIPアドレスと一致するか</u>どうかでなりすましを検知します。  
  
SPFでは以下の手順で送信元IPアドレスの検証を行います。

1. 送信側は、送信側ドメインのDNSサーバのSPFレコード(またはTXTレコード)に正当なメールサーバのIPアドレスやホスト名を登録し、公開しておく。
2. 送信側から受信側へ、SMTPメールが送信される。
3. 受信側メールサーバは、受信側ドメインのDNSサーバを通じて、MAIL FROMコマンドに記載された送信者メールアドレスのドメインを管理するDNSサーバに問い合わせ、SPF情報を取得する。
4. SPF情報との照合でSMTP接続してきたメールサーバのIPアドレスの確認に成功すれば、正当なドメインから送信されたと判断する。

# DKIMとの違い
SPFは、DNSベースのセキュリティプロトコル、導入する前では送信サーバドメインのDNSサーバに、SPFレコード(またはTXTレコード)を設定する必要がある。
他の検証系プロトコルと違って、SPFは電子署名や証明書を使用しない。
電子署名を利用した検証方法は[[DKIM]]

## SPF Record Structure and Failure Handling Mechanisms

### SPF Protocol Definition

Sender Policy Framework (SPF) is a DNS-based email authentication mechanism that allows domain owners to specify authorized mail transfer agents (MTAs) permitted to send email on behalf of their domain. SPF records define IP addresses and hostnames authorized to send email, along with explicit instructions for handling authentication failures.

### SPF Syntax and Failure Qualifiers

#### Basic SPF Record Structure

```
v=spf1 [mechanisms] [qualifiers] [modifiers] all
```

#### SPF Qualifiers (Failure Handling Mechanisms)

**1. Hard Fail (-all)**

```
v=spf1 ip4:192.168.1.0/24 ip4:10.0.0.0/8 include:_spf.google.com -all
```

**Technical Behavior**:

- Returns SMTP result code: "550 SPF check failed"
- Instructs receiving MTAs to reject the message
- Generates explicit authentication failure in headers
- Creates definitive fail result in authentication chain

**Security Implications**:

- Maximum protection against unauthorized senders
- Potential for legitimate email rejection
- Requires comprehensive authorized sender inventory
- No ambiguity in enforcement

**2. Soft Fail (~all)**

```
v=spf1 mx a ip4:203.0.113.0/24 include:sendgrid.net ~all
```

**Technical Behavior**:

- Returns SMTP result code: "250 OK" with SPF softfail header
- Message accepted but marked as suspicious
- Receiving MTA decides disposition based on local policy
- Typically results in spam folder delivery or increased spam score

**Security Implications**:

- Moderate protection with safety margin
- Allows gradual deployment and testing
- Reduces false positive impact
- Provides flexibility for complex environments

**3. Neutral (?all)**

```
v=spf1 include:_spf.salesforce.com ip6:2001:db8::/32 ?all
```

**Technical Behavior**:

- Neither passes nor fails authentication
- Equivalent to no SPF record for policy purposes
- No specific action recommended to receivers
- Provides mechanism documentation without enforcement

**Security Implications**:

- No protection against spoofing
- Used for informational purposes only
- Maintains compatibility during transitions
- Essentially negates SPF benefits

**4. Pass (+all) - NOT RECOMMENDED**

```
v=spf1 +all
```

**Technical Behavior**:

- Explicitly authorizes all sources
- Defeats purpose of SPF authentication
- Should never be used in production

### Complex SPF Record Examples with Failure Scenarios

#### Example 1: Enterprise Configuration with Strict Enforcement

```
v=spf1 ip4:198.51.100.0/24 ip4:203.0.113.0/28 include:_spf.office365.com include:_netblocks.amazonses.com exists:%{i}._spf.domain.com -all
```

**Failure Handling Analysis**:

- Unauthorized IPs receive hard fail
- No ambiguity in enforcement
- Clear audit trail in message headers
- SMTP-level rejection prevents delivery

**Implementation Considerations**:

- Requires complete IP inventory
- Regular updates for infrastructure changes
- Monitoring for false positives
- Coordination with cloud service providers

#### Example 2: Gradual Deployment with Soft Fail

```
v=spf1 mx a:mail.domain.com include:_spf.mailchimp.com ip4:192.0.2.0/24 ~all
```

**Failure Handling Analysis**:

- Failed authentication marked but not rejected
- Receiving servers apply local policies
- Typically increases spam probability score
- Messages remain recoverable from spam folders

**Header Example for Soft Fail**:

```
Received-SPF: softfail (domain.com: domain of sender@domain.com does not designate 192.0.2.99 as permitted sender) 
    client-ip=192.0.2.99; envelope-from=sender@domain.com;
X-SPF-Result: softfail
```

#### Example 3: Complex Multi-Provider Configuration

```
v=spf1 ip4:198.51.100.0/22 ip6:2001:db8:1234::/48 include:spf.protection.outlook.com include:_spf.salesforce.com include:_spf.zendesk.com include:sendgrid.net a:smtp.domain.com mx -all
```

**DNS Lookup Limit Considerations**:

- Maximum 10 DNS lookups (includes nested lookups)
- Mechanisms counting toward limit: include, a, mx, ptr, exists
- Exceeds limit = PermError (authentication failure)

### SPF Failure Processing Flow

#### 1. DNS Query Phase

```python
# Conceptual representation of SPF lookup
def check_spf_record(domain, sender_ip):
    spf_record = dns_query(f"{domain}", "TXT")
    if not spf_record.startswith("v=spf1"):
        return "None"  # No SPF record
```

#### 2. Mechanism Evaluation

```python
# Evaluation order (left to right)
def evaluate_mechanisms(spf_record, sender_ip):
    for mechanism in spf_record.split():
        if matches(mechanism, sender_ip):
            return get_qualifier(mechanism)
    return get_all_qualifier(spf_record)  # -all, ~all, ?all
```

#### 3. Result Determination

- **Pass**: Explicit authorization found
- **Fail**: -all qualifier triggered
- **SoftFail**: ~all qualifier triggered
- **Neutral**: ?all qualifier triggered
- **TempError**: DNS timeout or temporary failure
- **PermError**: Syntax error or DNS lookup limit exceeded

### Advanced Failure Handling Strategies

#### Redirect Modifier for Centralized Management

```
v=spf1 redirect=_spf.centraldomain.com
```

- Delegates entire SPF policy to another domain
- Failure handling inherited from target domain
- Simplifies multi-domain management

#### Explanation Modifier for Custom Messages

```
v=spf1 ip4:192.0.2.0/24 -all exp=explain.domain.com
```

- Provides custom explanation for failures
- TXT record at explain.domain.com contains message
- Limited support across receiving MTAs

#### Macro Expansion for Dynamic Policies

```
v=spf1 exists:%{i}._spf.%{d} -all
```

- Dynamic lookups based on sender characteristics
- Enables IP-specific or domain-specific policies
- Complex but powerful for large deployments

### Security Best Practices for Failure Handling

1. **Progressive Hardening**
    
    - Start with ~all during initial deployment
    - Monitor authentication reports via DMARC
    - Transition to -all after verification period
    - Document all authorized sources
2. **Failure Monitoring**
    
    - Implement DMARC for comprehensive reporting
    - Track soft fail rates and patterns
    - Investigate geographic anomalies
    - Correlate with security incidents
3. **Third-Party Integration**
    
    - Audit included SPF records regularly
    - Monitor for unauthorized modifications
    - Implement change control procedures
    - Consider dedicated subdomains for third parties
4. **Testing Methodology**
    
    - Use SPF testing tools before deployment
    - Verify from multiple geographic locations
    - Test all sending scenarios
    - Document expected vs. actual results

### Common Implementation Errors

1. **Syntax Errors Leading to PermError**

```
# Invalid - multiple SPF records
"v=spf1 ip4:192.0.2.0/24 ~all"
"v=spf1 include:_spf.google.com ~all"

# Correct - single combined record
"v=spf1 ip4:192.0.2.0/24 include:_spf.google.com ~all"
```

2. **DNS Lookup Limit Violations**

```
# Problematic - too many includes
v=spf1 include:provider1.com include:provider2.com include:provider3.com ... include:provider10.com -all

# Solution - use IP ranges or consolidate
v=spf1 ip4:192.0.2.0/24 ip4:198.51.100.0/24 include:consolidated-spf.domain.com -all
```

This comprehensive analysis demonstrates how SPF records handle authentication failures through various qualifiers and mechanisms, providing domain owners with granular control over email authentication enforcement while balancing security requirements with operational needs.
