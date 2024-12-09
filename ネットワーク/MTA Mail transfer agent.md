## What is a Mail Transfer Agent?

A mail transfer agent is a piece of software that transfers emails from one server to another. Imagine a network of couriers, each transferring a parcel from one to the next until it reaches its final destination. MTAs work in a very similar way. 

Mail transfer agents are kind of like the middlemen, transferring messages from one location to another. They work alongside Mail User Agents (MUAs), Mail Submission Agents (MSAs), and Mail Delivery Agents (MDAs) to cover every step of the process and ensure messages are delivered. 

**Mail user agents** enable you to create, send, receive, read, and manage emails—in other words, they are the email clients we use, such as Gmail, Outlook, and Yahoo. 

**Mail submission agents** receive the message from the MUA and forward it to the MTA. 

**Mail delivery agents** are software applications that deal with the delivery of email messages to their final recipients—usually a mailbox on a mail server. As well as delivering emails, they also handle sorting emails into various folders or applying filters, such as spam filters, email forwarding, and notifications.
## How MTAs work
**1. An email is sent and received by the MTA**

The sender composes and sends their email using an MUA like Gmail which is then received by the MSA. The MSA transfers the message to the MTA on Gmail’s server via SMTP (Simple Mail Transfer Protocol). 

**2. The MTA routes and, if necessary, relays the email**

The MTA checks the recipient’s email address and uses the domain to carry out a DNS (Domain Name System) lookup to fetch the MX (Mail Exchange) records for the recipient’s domain. This allows the MTA to see which mail server is accepting emails for the domain. 

If the receiving mail server is local, i.e. for internal company emails, the same MTA will be responsible for delivering the email. In this case, it will transfer the message directly to the MDA. 

If not, the MTA will identify the next MTA to transmit the message to until it reaches the MTA responsible for delivering the message for the domain. Sometimes, this may involve transferring or relaying the email through multiple MTAs until it reaches the one responsible for delivering messages to the recipient’s domain. 

**3. The email is transferred to the recipient server’s MTA**

The sending or relaying MTA establishes an SMTP connection with the recipient server’s MTA and transfers the email. If the transfer fails due to an error, such as network issues, the sending MTA will queue the email and retry sending until the email times out. This will result in the email bouncing. 

> [!NOTE] Received field
>When MTA received an email, it will add a extension header field called ‘received’ to record which MTA received this message, an example is like below
>```
Received: from sending-server.example.com (sending-server.example.com \[192.0.2.1])
by receiving-server.example.com (Postfix) with ESMTP id 1234ABCD
for \<recipient@example.com>;
Thu, 21 Dec 2023 13:45:23 +0000 (UTC)```

 


**4. The email is delivered**

The recipient server’s MTA receives the email and forwards it to the MDA. The MDA then delivers the email to the recipient’s mailbox, or MUA. MUAs use POP3 (Post Office Protocol 3) or IMAP (Internet Message Access Protocol) to retrieve emails from the mail server.  

POP3 downloads the email from the server to the client’s device and then usually deletes the email from the server. (It can be configured to keep a copy.)

IMAP offers more advanced features. Instead of simply downloading emails from the server, it synchronizes them between the server and client. This allows for messages to be accessed from multiple devices. 

Once the message has been synced or downloaded using one of these protocols, the recipient can open and interact with the email in their MUA (Gmail).