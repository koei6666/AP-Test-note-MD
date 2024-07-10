### 主なSQL命令
#### データベースオブジェクトの操作と変更
```SQL
CREATE データベースオブジェクトの定義
　CREATE TABLE Table_name

DROP データベースオブジェクトの削除
　DROP TABLE Table_name

ALTER データベースオブジェクトの定義変更
```

#### データ操作
```SQL
SELECT データの検索
　SELECT Criteria FROM Table_name [WHERE Condition] [GROUP BY Attribute] [HAVING Group_Condition]

INSERT データの挿入
  INSERT INTO Table_name (Values1, Values2 ...)

UPDATE データの更新
  UPDATE Table_name SET Coloumn_name = Value [WHERE Condition]

DELETE データの削除
```

#### データ制御
```SQL
GRANT 表などへのアクセス権付与
REVOKE アクセス権取り消し
COMMIT トランザクション更新処理の確定
ROLLBACK トランザクション更新処理の取り消し
```

#### カーソル定義、操作
SQL文埋め込み方式でデータベースアクセスする場合はカーソルの操作と定義が必要となる。
```SQL
DECLARE CURSOR カーソルの割り当て
OPEN カーソルのオーペン
FETCH カーソルが指し示す行データの取り出し
CLOSE カーソルのクローズ
```

[[基本的なSQL問い合わせ]]
[[表の結合]]