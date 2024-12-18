**curl** is a tool for transferring data from or to a server using URLs

## User-agent
![[HTTP#User-Agent]]

Typically without any additional setting, the user-agent header from a curl request will be
```
curl/{version}
```

However, with `-A` flag of curl, user and set their own custom user-agent header
```
curl -A "NotCurl/1.0" https://httpbin.org/user-agent
```

for this request above, the user-agent will be
```
NotCurl/1.0
```

since user can set whatever user-agent they want when using curl, it's also a security risk since attacker can fake their traffic with curl by set up their user agent looks like legitimate browser etc.
```
curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/91.0.4472.124"
```
by doing so, it can make some abnormal traffic hard to be noticed.