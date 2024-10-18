### 1. Cloud based DDoS prevention service
Cloudサービスプロバイダーのデータセンターの処理キャパシティや、分散の特性を利用して、大量なトラフィックをまず分析センター(Scrubbing center)に分散し、それぞれを分析することで、DDoS攻撃に対策する。
Cloudサービスプロバイダーのデータを活用し、攻撃のパターンやURL reputationを活用し、DDoSのトラフィックを判別することができる。

>[!NOTE]- そのほかのCloud based DDoS prevention measures
>
>Rate Limiting:
>- Implements throttling mechanisms to control the rate of incoming requests.
>- This helps prevent application layer attacks that might otherwise appear as legitimate traffic.
>  
>Challenge-Response Mechanisms:
   > - For suspicious traffic, the system may employ challenge-response tests (like CAPTCHAs).
   > - This helps distinguish between human users and automated bots.
   >
>SSL/TLS Inspection:
   > - Can decrypt and inspect encrypted traffic to detect attacks hidden within HTTPS requests.
   > - This is crucial for protecting against application layer attacks.
   >   
>Real-time Mitigation:
   > - As attacks are detected, mitigation rules are automatically applied.
   > - These rules can be updated in real-time as attack patterns change.
   >   
> Traffic Forwarding:
   > - After scrubbing, clean traffic is forwarded to the origin servers.
   > - This can be done through secure tunnels to maintain end-to-end security.
   >   
>Scalable Protection:
   > - Cloud providers can quickly scale up resources to handle larger attacks.
   > - This elastic capacity is a key advantage over on-premises solutions.
   >   
>Continuous Monitoring and Reporting:
   > - Provides real-time visibility into traffic patterns and attack metrics.
   > - Generates detailed reports for post-attack analysis and ongoing security improvements.
   >   
> Multi-layer Protection:
   > - Offers protection at multiple layers of the OSI model, from network layer (Layer 3) up to application layer (Layer 7) attacks.
   

### 2.Content Delivery Networks (CDNs)
CDNの分散ネットワークでトラフィックを分散することで、DDoSを希釈する。

### そのほかの外部DDoS対策サービス
- Anycast network diffusion:
    - Distribute incoming traffic across multiple servers in different locations.
    - Makes it harder for attackers to target a single point of failure.
- Traffic analysis and filtering:
    - Use intelligent systems to analyze traffic patterns and identify anomalies.
    - Implement rate limiting and IP reputation filtering.
- Web application firewalls (WAFs):
    - Deploy cloud-based WAFs to filter out malicious HTTP/HTTPS traffic.
    - Can be particularly effective against application layer attacks.
- API management and protection:
    - Implement robust authentication and rate limiting for APIs.
    - Use API gateways to manage and protect your services.
- Regular security audits and penetration testing:
    - Identify vulnerabilities before they can be exploited.
    - Continuously improve your security posture.
- Incident response planning:
    - Develop and regularly update a DDoS response plan.
    - Conduct drills to ensure your team can react quickly and effectively.
- ISP-level filtering:
    - Work with your Internet Service Provider to implement upstream filtering.
    - Some ISPs offer DDoS mitigation as a service.
- DNS redundancy:
    - Use multiple DNS providers to ensure continued accessibility even if one provider is attacked.