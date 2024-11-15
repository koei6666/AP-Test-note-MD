---
分類: 攻撃手段
---
## 概要
攻撃者がクエリストリング(リクエストの`?`の後）に悪意のあるスクリプトを埋め込み、Payloadをそのままクライアントに戻すようなサーバーでは、悪意のあるコードをユーザーに”反射(Reflect)"し、ユーザーの端末で実行される。
リンクの構築から、ユーザーがリンクをクリックするまでは[[DOM Based XSS]]と同じ流れで、２者の違いは、悪意のあるスクリプトがクライアントで実行されるか(DOM Based)?サーバーに送信してから反射されるか(Reflected)?

## 仕組み
```python
# Vulnerable server-side code example
@app.route('/search')
def vulnerable_search():
    query = request.args.get('q')
    # Directly inserting user input into HTML
    return f'<h1>Search results for: {query}</h1>'

# Secure version
@app.route('/search')
def secure_search():
    query = request.args.get('q')
    # Escape special characters
    safe_query = html.escape(query)
    return f'<h1>Search results for: {safe_query}</h1>'
```

上記のサーバに対して、下記のリンクでアクセスした場合、ページ上にそのままリンク内クリエストリングのqの値がhtml上に表示されるので、攻撃者のコードが実行される。
同じ処理と攻撃のトライでも、Secure versionではクエリストリングをエスケープした上表示するので、画面上にはqの値を表示するが、スクリプトは実行されない。
```url
https://sample.com/search?q=<script>alert(1)</script>
```