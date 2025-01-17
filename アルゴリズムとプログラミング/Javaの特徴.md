
^31b9c8

 Javaは[[プログラミング言語#^7ff59b|オブジェクト指向]]プログラミング言語である。クラスとオブジェクト、継承、カプセル化、ポリモーフィズムなどの主要なオブジェクト指向プログラミングの概念をサポートしています。しかし、Javaは複数のスーパクラスから継承する「多重継承」を直接サポートしていません。
 Javaには、CやC++のような直接的なポインタ型は存在しません。Javaでは、安全性とポータビリティを重視しており、ポインターを直接操作することによるエラーを避けるため、ポインターの概念は抽象化されています。したがって、Javaプログラマーはメモリ上のアドレスを直接参照または操作することはできません。
 [[Javaのリソース解放|Javaには自動ガーベジコレクションという機能があります]]。この機能は、プログラムが使用しなくなったメモリ（ガーベジ）を自動的に探し出し、再利用可能な状態に戻すためのものです。これにより、プログラマーはメモリ管理に関する煩雑な作業から解放され、[[プログラムメモリの割り当て#^0aff2a|メモリリーク]]や他のメモリ関連の問題を大幅に軽減できます。
 Javaの基本データ型は #プリミティブ型 として扱われます。これらはオブジェクトではなく、そのためクラスとして扱うことはできません。しかし、Javaはラッパークラスという機能を提供し、これを用いるとプリミティブ型をオブジェクトとして扱うことが可能になります。

### JavaのSQL
Javaでは、SQLへのコネクションを`connection`と、データベースへのlink stringを記載しており、
```java
Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/mydatabase", "user", "password");

```
実際データベースにアクセスし、SQLクエリを実行するのは、以下の3つの`Statement`になる、そのうち通常のSQLクエリ（文字列を単純に組み立てるタイプ）は`statement`になる
この`statement`の生成は、コネクションの`.createstatemt`を使用する
```java
stmt = connection.createStatemt()
ResultSet rs = stmt.executeQuery("SELECT * FROM" + table_name)
```

SQLインジェクション対策のため、SQL文条件をプレースホールダーする場合では、`PreparedStatemt`というタイプのオブジェクトを使用し、Javaからでは`.prepareStatement`メソッドが使用される
```java
preparedStatement stmt = connection.prepareStatement(SQL query)
```

以上2タイプの他に、DBサーバのstored proceduresを実行するために、`callableStatemt`というオブジェクトもある
```java
connection = DriverManager.getConnection(url, user, password); 
// Step 2: Prepare the CallableStatement 
String sql = "{CALL GetEmployeeDetails(?, ?, ?)}"; callableStatement = connection.prepareCall(sql); 
// Step 3: Set input 
parametercallableStatement.setInt(1, 101); // emp_id = 101 
// Step 4: Register output
parameterscallableStatement.registerOutParameter(2, Types.VARCHAR); // emp_name 
callableStatement.registerOutParameter(3, Types.DOUBLE);  // emp_salary 
// Step 5: Execute the stored procedure 
callableStatement.execute(); 
// Step 6: Retrieve output 
parameters String empName = callableStatement.getString(2); 
double empSalary =callableStatement.getDouble(3);
```

### ClassのStatic value
JavaのClassに定義される`Static`値は、Classのinstanceによって生成されなく、すべてのInstancesにアクセスされる共通値になる、例えば以下のコードの`orderNo`はすべてのinstancesに共有されるので、例えばAユーザが先にInstanceを生成したが、その後Bユーザが上書きした場合では、最終的に出力される値はBユーザが入力したデータになる
```java
public class sample_static_variable {
	private static String orderNo;
}
```

pythonに書き換えすると
```python
class sample_class_attribute()
	ORDER_NO = None
	def __init__(self):
		pass
```
