Linuxのファイルはユーザーグループごとに、アクセルの権限を規定している。

その権限は以下の2種類の表示の仕方で表示される。

## Symbolic
ファイルやディレクトリなどに対して、`ls -l`を使って詳細を表示するとき、最初に表示される10桁の印はそのファイルのタイプおよび権限を表示する
`drwxr-xr-x`
1桁目:ファイルタイプ
![[File type]]

2~10桁目:権限情報
左から三桁ずつでそれぞれ`Owner`,`Group`,`Others`を示し、それぞれ三桁で`r:読み取り`,`w:書き込み`、`x:実行`を表示する。
その権限を付与しない場合は`-`と示す。
例えば下記のtxtファイルに対して、`ls -l`を実行した結果は：
```bash
$ ls -l file.txt
-rwxrwxr-- 1 user group 1234 Oct 19 10:00 file.txt
```
`Owner`と`Group`グループは読み取り、書き込み、実行を付与し、`Others`グループには読み取り権限しか付与していないことを示している。

### Numeric
一般的には、rwxタイプでファイルの権限を表現しているが、スクリプトなどで設定する時や、一部の画面では三桁の数字で表現することがある
```bash
$ stat -c "%a %n" myfile.txt
774 myfile.txt
```
コマンドの中の`"%a"`は権限をバイナリで表現するstatのパラメーターである、
数字タイプの権限表示は同じく、3グループ（`Owner`,`Group`,`Others`)のアクセス権限をそれぞれ表現する。
- 読み取り権限:4
- 書き込み権限:2
 - 実行権限:1
 一つのグループに付与された権限は上記3つの数字の和で表現する、
 例えば上記ファイルの`rwxrwxr--`を数字に表現する場合は:
 `Owner`:4+2+1 = 7
 `Group`:4+2+1=7
 `Others`:4+0+0=4
 権限は774と表示される。


![[IMG_4093.jpeg]]