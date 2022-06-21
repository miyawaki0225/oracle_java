# JDBC

```SQL:department
CREATE DATABASE department;

CREATE table department(
  dept_code INT(11) NOT NULL PRIMARY KEY,
  dept_name VARCHAR(20) NOT NULL,
  dept_address VARCHAR(40) NOT NULL,
  pilot_number VARCHAR(20)
);

INSERT INTO department(dept_code,dept_name,dept_address,pilot_number) VALUES(1,'Sales','Tokyo','03-3333-xxxx');
INSERT INTO department(dept_code,dept_name,dept_address,pilot_number) VALUES(2,'Engineering','Yokohama','045-444-xxxx');
INSERT INTO department(dept_code,dept_name,dept_address,pilot_number) VALUES(3,'Development','Osaka',NULL);
INSERT INTO department(dept_code,dept_name,dept_address,pilot_number) VALUES(4,'Marketiong','Fukuoka','092-222-xxxx');
INSERT INTO department(dept_code,dept_name,dept_address,pilot_number) VALUES(5,'Education','Tokyo',NULL);

SELECT * FROM department;
```

```SQL:mytableA
CREATE table mytableA(
  field1 INT(10) NOT NULL,
  field2 INT(20) NOT NULL,
  field3 VARCHAR(20) NOT NULL,
);
```

```SQL:mytableB
CREATE table mytableB(
  field1 INT(10) NOT NULL,
  field2 INT(20) NOT NULL,
  field3 VARCHAR(20) NOT NULL,
);
```

```java
package main;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Main {
	public static void main(String[] args) {
		Connection con = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		String sql = "SELECT dept_code, dept_name FROM department";
		try {
			String url = "jdbc:mysql://localhost:3306/department?verifyServerCertificate=false&useSSL=false&serverTimezone=JST";
			con = DriverManager.getConnection(url, "root", "test");
			pstmt = con.prepareStatement(sql);
			rs = pstmt.executeQuery();
			while (rs.next()) {
				System.out.println("dept_code:" + rs.getInt(1));
				System.out.println("dept_name:" + rs.getString(2));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {
				if (rs != null)
					rs.close();
				if (pstmt != null)
					pstmt.close();
				if (con != null)
					con.close();

			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}
}

```

```java
//jdbc:<subprotocol>:<subname>
//"jdbc:mysql://localhost:3306/department?verifyServerCertificate=false&useSSL=false&serverTimezone=JST"
```
### ResultSetインターフェースには、getObject()メソッドも提供されている

### SQL型⇒日付／時刻型への変換

```java
//java.sql.Date ⇒ java.time.LocalDate
java.sql.Date sqlDate = rs.getDate(1);
java.time.LocalDate localDate = sqlDate.toLocalDate();
System.out.println("localDate : " + localDate);

//java.sql.Time ⇒ java.time.LocalTime
java.sql.Time sqlTime = rs.getTime(2);
java.time.LocalTime localTime = sqlTime.toLocalTime();
System.out.println("localTime : " + localTime);

//java.sql.Timestamp ⇒ java.time.LocalDatetime
java.sql.Timestamp timestamp = rs.getTimestamp(3);
java.time.LocalDateTime localDateTime = timestamp.toLocalDateTime();
System.out.println("localDateTime : " + localDateTime);
```


|インターフェース名|説明|
|---|---|
|Statement|標準的なSQL文を実行|
|PreparedStatement|プリコンパイルされたSQL文を実行|
|CallableStatement|ストアドプロシージャを実行|

> ストアドプロシージャとは、
> データベース管理システム（DBMS）の機能の一つで、データベースに対する連続した複数の処理を一つのプログラムにまとめ、
> データと共に保存できるようにしたもの。処理はDBMS側で行われ、外部からはクエリを発行するのと同じ手順で実行できる。


### 接続専用クラス作成

```java
//executeQuery()メソッドで問い合わせ（SELECT）
String sql = "SELECT dept_name FROM department WHERE pilot_number = ?";

//executeUpdate()メソッドで挿入処理（INSERT）
String sql = "INSERT INTO department VALUES(6,'Planning','Yokohama','045-333-xxxx')";

//executeUpdate()メソッドで更新処理（UPDATE）
String sql = "UPDATE department set dept_address='Tokyo',pilot_number='03-6666-xxxx' WHERE dept_code = ?";

//executeUpdate()メソッドで削除処理（DELETE)
String sql = "DELETE FROM department WHERE dept_code = ?";
```

|メソッド名|戻り値|説明|
|---|---|---|
|execute()|boolean|true:ResultSetオブジェクトの取得が可能  false:更新行数の取得が可能、もしくは結果無し|
|executeQuery()|ResultSet|結果の取得には、1回はnextを呼ぶ。該当レコードがない場合でも、ResultSetはnullにならない。|
|executeUpdate()|int|更新行数が返る。更新した行がなければ0が返る|

### CallableStatementインターフェース

> 構文
> `{call <procedure-name> [(<arg1>,<arg2>,...)]}`

[初心者からのMySQLストアドプロシージャ＆ファンクション入門]
(https://proengineer.internous.co.jp/content/columnfeature/7078)

### ストアドルーチンとは？
ストアドルーチン：サーバ上に名前を付けて格納しておける一塊のSQLのこと。 長いクエリを発行せず名前を呼ぶだけで実行できる。 
- ストアドプロシージャ：戻り値がない、再帰的利用可能
- ストアドファンクション（ユーザー定義関数）：戻り値あり、再帰的利用不可能

```SQL
CREATE PROCEDURE [プロシージャ名]([IN/OUT][引数][データ型])
BEGIN
[///一連の処理を記載]
END;
```
「in」：入力引数  
「out」：出力引数  

#### ストアドプロシージャ登録時の注意
> ストアドプロシージャの登録は「CREATE～END;」
> までの記述を全てそのまま入力するだけで行うことができますが、そこで問題となるのはデリミタ（終端文字）です。
> 通常コマンドライン上で使用するデリミタは「;」ですが、
> ストアドルーチンの場合は本文中の処理で「;」が登場してしまうため、
> そのまま入力しようとすると一番最初の「;」でエラーとなってしまいます。
> そこで「DELIMITER」コマンドを使用して終端文字を「//」に変更し、「CREATE～END;」の末尾に「//」を追加して実行します。
> 登録後は、忘れないようデリミタを「;」に戻すようにして下さい。
> ```sql
> DELIMITER ;
> ```

`DELIMITER`はコマンドライン専用

- 登録済みのストアドプロシージャを呼び出すには、「CALL」を使用します。
- プロシージャを削除したい場合は、「DROP PROCEDURE文」を使用します。

```sql
CREATE FUNCTION [ファンクション名]([引数] [引数のデータ型])
　　RETURNS [戻り値のデータ型] [DETERMINISTIC／NOT DETERMINISTIC]
BEGIN
　　[・・・一連の処理を記載・・・]
　　RETURN(戻り値);
END;
```
戻り値を持つファンクションには、引数の前の「IN／OUT」は不要です。その代わりに、「RETURNS」の後に戻り値のデータ型を指定します。  
さらに入力値が同じであれば常に戻り値が同じになる場合は末尾に「DETERMINISTIC」を、異なる可能性がある場合は「NOT DETERMINISTIC」を末尾に記載します。

```java
//「DECLARE文」は、変数の宣言です。
DELARE [変数名] [データ型];
//変数などに値を代入するには、「SET文」を使用します。
SET [変数名] = [代入値];
//これらはプロシージャでも同様に使用できます。
```

ストアドファンクションの登録
```sql
DELIMITER //
CREATE FUNCTION func_test01(input INT) RETURNS FLOAT(10,2) DETERMINISTIC
BEGIN
　　DECLARE zei_ritsu FLOAT(3,2);
　　SET zei_ritsu = 1.08;
　　RETURN input * zei_ritsu;
END;
//
```

#### 登録済ストアドルーチンの一覧を表示する
登録済みプロシージャの一覧を表示したい場合は、「information_schema.ROUTINES」というテーブルを参照することで表示することができます。

```sql
SELECT ROUTINE_NAME, ROUTINE_TYPE
　　FROM information_schema.ROUTINES
　　WHERE ROUTINE_TYPE = 'PROCEDURE';
```


- JDBC URLに含まれる必須の要素。>> jdbcプロトコル名、データベース名
- ファイルの配置アドレス ⇒ `META-INF\services\java.sql.Driver'
- Connectionクラスは抽象クラスであるため、newによるインスタンス化はできません

```sql
//第2引数は、方向を決める
//TYPE_FORWARD_ONLY
//TYPE_SCROLL_INSENSITIVE
//TYPE_SCROLL_SENSITIVE
//
//第3引数は、更新可能かどうか
//CONCUR_READ_ONLY
//CUNCUR_UPDATABLE
PreparedStatement pstmt = con.prepareStatement(
		sql,
		ResultSet.TYPE_SCROLL_INSENSITTIVE,
		ResultSet.CONCUR_UPDATABLE);
```

- ResultSetインターフェースでは、（local○○は取り扱っていない）
- CallableStatementオブジェクト
	- 取得：ConnectionインターフェースのprepareCall()メソッド
	- 登録：CallableStatementインターフェースのregisterOutParameter()メソッド
