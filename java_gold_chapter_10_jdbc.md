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
