```http
# Example Security Headers (related to SOP)

# Strict-Transport-Security: Force HTTPS
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload

# Content-Security-Policy: Control resource loading
Content-Security-Policy: default-src 'self'; \
                       script-src 'self' https://trusted-scripts.com; \
                       img-src 'self' https://trusted-images.com; \
                       style-src 'self' https://trusted-styles.com;

# Cookie settings
Set-Cookie: sessionId=abc123; \
           Secure; \           # Only send over HTTPS
           HttpOnly; \         # Prevent JavaScript access
           SameSite=Strict; \ # Only send in same-site requests
           Domain=example.com; \
           Path=/
```
[[Content Security Policy]]