CGIはWebサーバに外部のプログラムを実施するための基準。

動的にHTMLを戻すcgiの例
```HTTP
# Request
POST /cgi-bin/test.cgi HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 27

name=John&message=Hello+World

# Response
HTTP/1.0 200 OK
Content-Type: text/html

<html>
<body>
<h1>Hello John!</h1>
<p>Your message: Hello World</p>
</body>
</html>

```

パラメーターを入力し内容を要求する例
```Http
# Request
GET /cgi-bin/info.cgi?user=admin&type=system HTTP/1.0
Host: example.com

# Response
HTTP/1.0 200 OK
Content-Type: text/html

<html>
<body>
<h2>System Info for: admin</h2>
<pre>
Time: 12:00:00
Server: Apache
</pre>
</body>
</html>
```

HTTPリクエストでは`?`でURLの完了を示し、`?`以降のをパラメーターと解釈される
```HTTP
1. Basic:
/page.cgi?name=value

2. Multiple parameters:
/search.cgi?keyword=python&category=books

3. No parameters (no ? needed):
/page.cgi

4. Empty parameter:
/page.cgi?name=

5. Complex path with parameters:
/store/products/view.cgi?id=123&color=blue
```