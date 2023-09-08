### WITH
WITHはその問い合わせの中だけ使用できる一時的な表を定義する文。
例えば在籍社員数が一番多い部門を検索する場合だと、以下の通りWITHを使用して検索することができる：
```SQL
WITH StaffCount (DepartmentCode,StaffNumber)
AS (SELECT DepartmentCode, COUNT(*) 
   FROM Department GROUP BY DepartmentCode)

SELECT DepartmentCode, StaffNumber FROM StaffCount
WHERE StaffNumber = (SELECT MAX(StaffNumber) FROM StaffCount)
```

### CASE
CASEは問い合わせの中、結果を条件に合わせて別の値に変換できる文、
例えば社員の年代を集計する場面では：
```SQL
SELECT E.Name, CASE
				WHEN E.Age < 30 THEN '20s'
				WHEN E.Age >= 30 AND E.Age < 40 THEN '30s'
				ELSE 'Over 30'
				END AS AgeGroup
FROM Employee E
```