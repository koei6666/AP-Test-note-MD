---
分類: 認証方法
---

Server Name Indication (SNI) is a crucial extension of the Transport Layer Security (TLS) protocol that allows a client, such as a web browser, to specify the hostname it is trying to connect to during the initial handshake process. This capability is essential for enabling multiple secure websites to be served from a single IP address without requiring all sites to share the same SSL certificate.

## How SNI Works

1. **TLS Handshake**: During the [[TLS Handshake|TLS handshake]], the client sends a "Client Hello" message that includes the SNI field. This field indicates the hostname of the website the client wants to connect to.
2. **Server Response**: The server uses this information to select and present the appropriate SSL certificate for that hostname. This ensures that the client receives a valid certificate that matches the requested domain, preventing common name mismatch errors.
3. **Multiple Certificates**: Without SNI, if multiple domains are hosted on a single server, each would typically require its own IP address to avoid certificate mismatches. SNI allows these domains to share an IP address while still using separate SSL certificates.

## Importance of SNI

- **Cost Efficiency**: By allowing multiple SSL certificates on a single IP address, SNI reduces the need for additional IP addresses, which are limited under IPv4.
- **Enhanced Security**: SNI helps ensure that clients connect securely to the correct domain, thus enhancing overall security by reducing the risk of man-in-the-middle attacks due to certificate mismatches.
- **Widespread Support**: Most modern web browsers and servers support SNI, although some older systems may not. For example, browsers like Google Chrome and Firefox have supported SNI for many years, while legacy systems like Windows XP do not support it.

## Limitations

While SNI is widely supported, there are some limitations:
- **Visibility of Hostname**: The hostname specified in the SNI field is **not encrypted** in the initial handshake, which means it can be visible to eavesdroppers.
- **Compatibility Issues**: Some older browsers and operating systems do not support SNI, which can lead to users encountering default certificates instead of the intended ones.

In summary, Server Name Indication (SNI) is an essential feature of TLS that facilitates secure connections for multiple domains hosted on a single server by allowing clients to specify their desired hostname during the handshake process.

