# Network Level Authentication (NLA)

Network Level Authentication is a security feature implemented in Remote Desktop Protocol (RDP) that requires user authentication before establishing a full remote desktop connection.

## Technical Definition

NLA leverages the Credential Security Support Provider (CredSSP) protocol to perform strong user authentication through the Security Support Provider Interface (SSPI), validating user credentials before a remote session is fully established.

## Security Benefits

- **Pre-Connection Authentication**: Requires authentication before creating a complete RDP session, reducing exposure to unauthenticated connections
- **Resource Protection**: Mitigates server-side resource exhaustion attacks by authenticating users before allocating full session resources
- **Man-in-the-Middle Mitigation**: When properly configured with TLS, provides protection against session hijacking and credential theft
- **Reduced Attack Surface**: Limits the attack surface by requiring authentication before exposing the full RDP stack

## Implementation Context

NLA is enabled by default in modern Windows systems (Windows Vista and later) and can be configured through Group Policy or system settings. The authentication process occurs within the Transport Layer Security (TLS) tunnel, providing credential protection during transmission.

## Technical Standards

NLA implementation follows protocols defined in:

- MS-RDPBCGR: Remote Desktop Protocol Basic Connectivity and Graphics Remoting
- MS-CSSP: Credential Security Support Provider Protocol
- RFC 4556: Public Key Cryptography for Initial Authentication in Kerberos (PKINIT)

## Security Considerations

- Disabling NLA creates significant security vulnerabilities, including exposure to attacks like BlueKeep (CVE-2019-0708)
- NLA should be complemented with additional security controls such as Network Access Control, TLS configuration, and proper credential management
- Legacy clients incompatible with NLA require careful security assessment before allowing connections

