```mermaid
graph TD
    A[X.509 Certificate] --> B[Version]
    A --> C[Serial Number]
    A --> D[Signature Algorithm]
    A --> E[Issuer]
    A --> F[Validity Period]
    A --> G[Subject]
    A --> H[Subject Public Key Info]
    A --> I[Extensions]
    
    I --> J[Subject Alternative Name]
    I --> K[Other Extensions]
    
    J --> L[DNS Names]
    J --> M[IP Addresses]
    J --> N[Email Addresses]
    J --> O[URIs]
    
    G --> P[Common Name]
    G --> Q[Organization]
    G --> R[Country]
    
    style J fill:#f9f,stroke:#333,stroke-width:2px
    style L fill:#bbf,stroke:#333
    style M fill:#bbf,stroke:#333
    style N fill:#bbf,stroke:#333
    style O fill:#bbf,stroke:#333
```

# Subject Alternative Name
The Subject Alternative Name (SAN) extension is a field within an X.509 certificate itself that contains identifiers (domain names, IP addresses, email addresses, URIs) for which the certificate is valid. When a client (like a web browser) connects to a server presenting a certificate, the client checks if the server's hostname matches any of the identifiers listed in the SAN extension of that certificate.