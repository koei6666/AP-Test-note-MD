The /etc/shadow file is a critical system file in Linux that stores encrypted password information and password-related settings for user accounts. Here's what it contains and does:

1. Encrypted passwords: It stores password hashes (not the actual passwords) for user accounts
2. Password aging information including:
   - Last password change date
   - Minimum days between password changes
   - Maximum password age
   - Password expiration warning period
   - Days after password expires until account is disabled
   - Account expiration date

**The file is only readable by the root user for security purposes**. This is an improvement over the older system where password hashes were stored in /etc/passwd, which needed to be readable by all users.

A typical entry in /etc/shadow looks like this:
```
username:$6$salt$hashedpassword:18000:0:99999:7:0:0:
```

The \$6$ indicates SHA-512 hashing algorithm (newer systems), and each field is separated by colons.