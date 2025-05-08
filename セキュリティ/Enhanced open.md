I'll search for more specific information about this Wi-Fi security protocol that encrypts traffic on open APs without requiring authentication.​​​​​​​​​​​​​​​​



## Enhanced Open Protocol / Opportunistic Wireless Encryption (OWE)

Thank you for clarifying. You're referring to what's formally known as Wi-Fi CERTIFIED Enhanced Open™, which is based on Opportunistic Wireless Encryption (OWE) technology. This is indeed a security protocol specifically designed for open Wi-Fi networks that provides encryption without requiring users to authenticate.

### Technical Definition

Enhanced Open, also called Opportunistic Wireless Encryption (OWE), is a Wi-Fi standard that ensures encrypted communication between public hotspots and end devices without requiring authentication. It was introduced by the Wi-Fi Alliance as part of Wi-Fi Protected Access 3 (WPA3). [![Wikipedia](claude-citation:/icon.png?validation=FAD8CAEE-1D29-4452-B1D8-DB2D9959CF12&citation=eyJlbmRJbmRleCI6NzAwLCJtZXRhZGF0YSI6eyJmYXZpY29uVXJsIjoiaHR0cHM6XC9cL3d3dy5nb29nbGUuY29tXC9zMlwvZmF2aWNvbnM/c3o9NjQmZG9tYWluPXdpa2lwZWRpYS5vcmciLCJzaXRlRG9tYWluIjoid2lraXBlZGlhLm9yZyIsInNpdGVOYW1lIjoiV2lraXBlZGlhIiwidHlwZSI6IndlYnBhZ2VfbWV0YWRhdGEifSwic3RhcnRJbmRleCI6NDE4LCJ0aXRsZSI6Ik9wcG9ydHVuaXN0aWMgV2lyZWxlc3MgRW5jcnlwdGlvbiAtIFdpa2lwZWRpYSIsInVybCI6Imh0dHBzOlwvXC9lbi53aWtpcGVkaWEub3JnXC93aWtpXC9PcHBvcnR1bmlzdGljX1dpcmVsZXNzX0VuY3J5cHRpb24iLCJ1dWlkIjoiMjJiMzU2ZmEtOGEyZi00MjBjLWFjZTQtMTA1ZDRjMmNiNDA2In0%3D)](https://en.wikipedia.org/wiki/Opportunistic_Wireless_Encryption)

OWE is defined in RFC 8110 as an extension to IEEE 802.11 that "uses a cryptographic handshake to encrypt the devices connecting open network access points." It employs similar cryptography to that used in Simultaneous Authentication of Equals (SAE). [![Wi-fi](claude-citation:/icon.png?validation=FAD8CAEE-1D29-4452-B1D8-DB2D9959CF12&citation=eyJlbmRJbmRleCI6OTUyLCJtZXRhZGF0YSI6eyJmYXZpY29uVXJsIjoiaHR0cHM6XC9cL3d3dy5nb29nbGUuY29tXC9zMlwvZmF2aWNvbnM/c3o9NjQmZG9tYWluPXdpLWZpLm9yZyIsInNpdGVEb21haW4iOiJ3aS1maS5vcmciLCJzaXRlTmFtZSI6IldpLWZpIiwidHlwZSI6IndlYnBhZ2VfbWV0YWRhdGEifSwic3RhcnRJbmRleCI6NzAyLCJ0aXRsZSI6IldpLUZpIENFUlRJRklFRCBFbmhhbmNlZCBPcGVu4oSiOiBUcmFuc3BhcmVudCBXaS1GacKuIHByb3RlY3Rpb25zIHdpdGhvdXQgY29tcGxleGl0eSB8IFdpLUZpIEFsbGlhbmNlIiwidXJsIjoiaHR0cHM6XC9cL3d3dy53aS1maS5vcmdcL2JlYWNvblwvZGFuLWhhcmtpbnNcL3dpLWZpLWNlcnRpZmllZC1lbmhhbmNlZC1vcGVuLXRyYW5zcGFyZW50LXdpLWZpLXByb3RlY3Rpb25zLXdpdGhvdXQtY29tcGxleGl0eSIsInV1aWQiOiJkZDc0ZmVhOS0wYTIyLTQzNjQtYjY3OC01N2ZlMDYyMjdkOTcifQ%3D%3D)](https://www.wi-fi.org/beacon/dan-harkins/wi-fi-certified-enhanced-open-transparent-wi-fi-protections-without-complexity)

### How It Works

The protocol works by performing an unauthenticated Diffie-Hellman key exchange during the association process. When a client connects to an OWE-enabled access point, they exchange public keys that allow them to generate a unique Pairwise Master Key (PMK), which is then used in a 4-way handshake to create traffic encryption keys. 

This encryption mechanism is conceptually similar to how HTTPS connections work between browsers and web servers, with the key difference being that Wi-Fi access points do not use certificates for authentication. This means that while OWE prevents passive eavesdropping, it doesn't protect against man-in-the-middle attacks with rogue access points. 

### Implementation and Compatibility

For backward compatibility, OWE offers a transition mode that allows both OWE-capable devices and legacy clients to connect to the same network. This is implemented by creating two SSIDs: one visible SSID for open authentication and one hidden SSID for OWE authentication, with information linking them together. 

In practical terms, this means that devices supporting OWE can establish encrypted connections to public Wi-Fi hotspots automatically, while legacy devices can still connect using traditional open authentication. The key security advantage is that OWE connections use a unique session key negotiated on-the-fly rather than a shared network password. 

### Security Benefits and Limitations

The primary security benefit of OWE is that it provides encryption for open networks without adding complexity for users. It remains a "select and connect" experience, but all traffic is encrypted, preventing passive eavesdropping attacks that are common on public Wi-Fi networks.

It's important to note that OWE "only protects against passive attacks" and doesn't provide authentication of the access point. This means it's still vulnerable to active attacks where an attacker sets up a malicious access point.

Enhanced Open represents a significant improvement in the security posture of public Wi-Fi networks, offering encryption without sacrificing convenience - addressing a longstanding vulnerability in open wireless networks.