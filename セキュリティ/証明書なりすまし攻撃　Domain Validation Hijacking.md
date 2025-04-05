# Domain Validation Hijacking: Attack Flow

## Attack Overview

Domain Validation Hijacking is an attack that exploits the Certificate Authority (CA) validation process to fraudulently obtain legitimate TLS certificates for domains the attacker doesn't own.

## Attack Sequence

### 1. Initial Server Compromise

- Attacker gains unauthorized access to the target web server
- Attacker obtains sufficient privileges to create files in the web root directory
- No special configuration changes are needed as long as file write access is achieved

### 2. Certificate Request Process

- Attacker generates their own private/public key pair
- Attacker submits a Certificate Signing Request (CSR) to a legitimate, trusted CA
- The CSR contains the attacker's public key and the target domain name
- CA initiates domain control validation process using HTTP-01 challenge

### 3. Domain Validation Manipulation

- Attacker creates `/.well-known/pki-validation/` directory on the compromised server
- Attacker places the CA-provided challenge file in this directory
- CA's validation check succeeds, confirming apparent domain control
- This exploits the fundamental trust assumption in domain validation

### 4. Certificate Acquisition

- CA issues a valid, browser-trusted certificate binding the domain to the attacker's public key
- Attacker maintains exclusive possession of the corresponding private key
- Certificate appears legitimate to all technical verification systems

### 5. Traffic Interception Deployment

- Attacker implements traffic redirection through DNS hijacking, network manipulation, or social engineering
- Victim traffic is routed to attacker-controlled infrastructure
- Attacker's server presents the fraudulently-obtained but technically valid certificate
- Browsers accept the certificate without security warnings
- Traffic is decrypted using the attacker's private key, allowing credential theft and data manipulation

## Security Implications

This attack is particularly dangerous because it bypasses the core security mechanism of HTTPS without triggering any client-side security warnings, potentially leading to credential theft, data exposure, and service impersonation.

# Related countermeasures
[[Certificate Transparency]]:
Helps validate the legitimacy of a certificate by validate the issue record from a append-only issue log, mitigate the risk of trust a fraud certificate 

[[DNS Records#^ec373f|CAA specification in DNS records]]:
By specify the CAA in DNS records, a domain can limit the CA which can issue the certificate for this domain