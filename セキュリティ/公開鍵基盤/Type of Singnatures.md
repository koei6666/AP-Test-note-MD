# Raw Signature
Separated from the message body and certificate, raw signature are generated from the data body and encrypted. (sometimes also encoded when necessary)
in order to use raw signature from party-A, party-B will also need to obtain the certificate, and the message body. in order to exchange and use raw signature to validate the data, there will be 3 data be exchanged between the party-A and party-B

# CMS Signature(Cryptographic Message Syntax)
A CMS signature is a standardized way to send data + signature + certificate with the public key all in one file from Party-A to Party-B. As long as the certificate is valid and not revoked, Party-B can use the supplied public key to verify the signature.
[[TLS Certificate Validation]]

# SOAP Signature
A SOAP signature also contains data and the signature and optionally the certificate. All in one XML payload. There are special steps involved in calculating the hash of the data. This has to do with the fact that the SOAP XML sent from system to system might introduce extra elements or timestamps. Also, SOAP Signing offers the possibility to sign different parts of the message by different parties.

# Email Signature
Sending emails is not very difficult. You have to fill in some data and send it to a server that forwards it, and eventually it will end up at its destination. However, it is possible to send emails with a FROM field that is not your own email address. In order to guarantee to your receiver that you really sent this email, you can sign your email. A trusted third party will check your identity and issue an email signing certificate. You install the private key in your email application and configure it to sign emails that you send out. The certificate is issued on a specific email address and all others that receive this email will see an indication that the sender is verified, because their tools will verify the signature using the public certificate that was issued by the trusted third party.