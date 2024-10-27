JavaではGarbage Collection機能を備えており、通常メモリ上のオブジェクトであれば、使用後明記的に解放しなくても、Garbage Collectionにより自動的に解放される。

ただ、以下のようなリソースは自動的に解放されないので、明記的に解放する必要がある:
- システムリソースを使用するなどのオブジェクト（データベースへの接続や、ファイルのI/Oストリームを使用するオブジェクトは、明記的に`.close()`を宣言しないと、接続が継続的に維持され、リソースがロックされる可能性がある。
```java
Statement stmt = null;
ResultSet resultObj = null;
BufferedInputStream in = null;
try {
    stmt = conn.createStatement();
    resultObj = stmt.executeQuery(sql);
    // ... 中略 ...
    in = new BufferedInputStream(new FileInputStream(fileObj));
    // ... 処理 ...
} catch (Exception e) {
    // エラー処理
} finally {
    try {
        if (in != null) in.close();
        if (resultObj != null) resultObj.close();
        if (stmt != null) stmt.close();
    } catch (Exception e) {
        // クローズ時のエラー処理
    }
}
```