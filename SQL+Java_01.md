# java + SQL Basic (2019.05.30)


java
package db1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Insert {

	public static void main(String[] args) {
		// 1. JDBC Driver Loading..
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			// 자동으로 oracle에 접속
			// Oracle/Maria DB 등에 접속
		} catch (ClassNotFoundException e) {
			System.out.println("Driver Loading Error");
		}
		// 2. Connection 1521 db(workspace) db(pwd)
		String id = "db";
		String password = "db";
		String url = "jdbc:oracle:thin:@70.12.50.227:1521:xe"; //ip adress
		
		Connection con = null;
		try {
			con = DriverManager.getConnection(url, id, password);
		} catch (SQLException e) {
			System.out.println("Connection Error");
			e.printStackTrace();
		}
		
		// 3. SQL 전송
		String sql = "INSERT INTO T_USER VALUES (?,?,?)";
		PreparedStatement pstmt = null;
		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, "id20");
			pstmt.setString(2, "pwd20");
			pstmt.setString(3, "수키");
			int result = pstmt.executeUpdate();
			System.out.println(result);
			} catch (SQLException e) {
			e.printStackTrace();
		}
		
		// 4. close
		try {
			pstmt.close();
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
	}

}
```



```java
package db1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Delete {

	public static void main(String[] args) {
		// 1. JDBC Driver Loading..
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
		} catch (ClassNotFoundException e) {
			System.out.println("Driver Loading Error");
		}
		
		// 2. Connection 1521 db(workspace) db(pwd)
		String id = "db";
		String password = "db";
		String url = "jdbc:oracle:thin:@70.12.50.227:1521:xe"; //ip adress
		
		Connection con = null;
		try {
			con = DriverManager.getConnection(url, id, password);
		} catch (SQLException e) {
			System.out.println("Connection Error");
			e.printStackTrace();
		}
		
		// 3. SQL 전송
		String sql = "DELETE FROM T_USER WHERE ID=?";
		PreparedStatement pstmt = null;
		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, "id20");
			int result = pstmt.executeUpdate();
			System.out.println(result);
			} catch (SQLException e) {
			e.printStackTrace();
		}
		
		// 4. close
		try {
			pstmt.close();
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
	}

}

```



```java
package db1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;

public class Update {

	public static void main(String[] args) {
		// 1. JDBC Driver Loading..
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
		} catch (ClassNotFoundException e) {
			System.out.println("Driver Loading Error");
		}
		
		// 2. Connection 1521 db(workspace) db(pwd)
		String id = "db";
		String password = "db";
		String url = "jdbc:oracle:thin:@70.12.50.227:1521:xe"; //ip adress
		
		Connection con = null;
		try {
			con = DriverManager.getConnection(url, id, password);
		} catch (SQLException e) {
			System.out.println("Connection Error");
			e.printStackTrace();
		}
		
		// 3. SQL 전송
		String sql = "UPDATE T_USER SET PWD=?, NAME=? WHERE ID=?";
		PreparedStatement pstmt = null;
		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, "0000");
			pstmt.setString(2, "아키");
			pstmt.setString(3, "ID10");
			int result = pstmt.executeUpdate();
			System.out.println(result);
			} catch (SQLException e) {
			e.printStackTrace();
		}
		
		// 4. close
		try {
			pstmt.close();
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
	}

}

```



```java
package db1;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

public class Select {

	public static void main(String[] args) {
		// 1. JDBC Driver Loading..
		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
		} catch (ClassNotFoundException e) {
			System.out.println("Driver Loading Error");
		}
		
		// 2. Connection 1521 db(workspace) db(pwd)
		String id = "db";
		String password = "db";
		String url = "jdbc:oracle:thin:@70.12.50.227:1521:xe"; //ip adress
		
		Connection con = null;
		try {
			con = DriverManager.getConnection(url, id, password);
		} catch (SQLException e) {
			System.out.println("Connection Error");
			e.printStackTrace();
		}
		
		// 3. SQL 전송 & Receive
		String sql = "SELECT * FROM T_USER WHERE ID=?";
		PreparedStatement pstmt = null;
		ResultSet rset = null;
		try {
			pstmt = con.prepareStatement(sql);
			pstmt.setString(1, "ID01");
			rset = pstmt.executeQuery();
			rset.next(); //한칸옮기고
			String uid = rset.getString("ID");//Column명칭
			String upwd = rset.getString("PWD");
			String uname = rset.getString("NAME");
			System.out.println(uid+" "+upwd+" "+uname);
			} catch (SQLException e) {
			e.printStackTrace();
		}
		
		// 4. close
		try {
			rset.close();
			pstmt.close();
			con.close();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
	}

}

```

