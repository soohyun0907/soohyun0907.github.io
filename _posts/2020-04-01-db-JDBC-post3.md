---
title: "Database 개념3 - JDBC"
excerpt: "Java Database Connectivity"

categories:
 - Database
tags:
 - Database
layout: single
---

# JDBC

Java Database Connectivity

## JDBC 연결 순서

1. Connection 생성 : com.mysql.cj.jdbc.Driver 로딩

2. 연결 : jdbc:mysql://127.0.0.1:3306/[db이름]?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8

3. Statement 생성

4. execute() or executeQuery() 실행

5. preparedStatment

6. resultSet

   <img width="463" alt="JDBC" src="https://user-images.githubusercontent.com/33771279/78201045-95491800-74cb-11ea-9c56-806f9d18fadb.png">

### 코드 구현_executeQuery();

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBC1 {

	public static void main(String[] args) throws ClassNotFoundException, SQLException {
		// 1. Driver Loading
		Class.forName("com.mysql.cj.jdbc.Driver");
		// 2. Connection 연결
		Connection con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SCOTT?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8", "id","pwd");
		// 3. Statement Create
		Statement st = con.createStatement();
		// 4. SQL Execute
		ResultSet rs = st.executeQuery("select * from emp");
		while(rs.next()) {
			System.out.println(rs.getString("ename") + " , " + rs.getInt("sal"));
		}
		// 5. close
		rs.close();
		st.close();
		con.close();
	}

}
```

### 코드구현_execute();

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBC2 {

	public static void main(String[] args) throws ClassNotFoundException, SQLException {
		// 1. Driver Loading
		Class.forName("com.mysql.cj.jdbc.Driver");
		// 2. Connection 연결
		Connection con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SCOTT?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8","id","pwd");
		// 3. Statement Create
		Statement st = con.createStatement();
		// 4. SQL Execute
		String sql = "insert into emp(empno, ename, sal) values(9999,'또치',1000)";
		st.execute(sql);
		System.out.println("sql 실행 완료");
		// 5. close
		st.close();
		con.close();
	}

}
```

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBC3 {

	public static void main(String[] args) {
		Connection con = null;
		Statement st = null;
		ResultSet rs = null;
		try {
			// 1. Driver Loading
			Class.forName("com.mysql.cj.jdbc.Driver");
			// 2. Connection 연결
			con = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/SCOTT?serverTimezone=UTC&useUniCode=yes&characterEncoding=UTF-8","id","pwd");
			// 3. Statement Create
			st = con.createStatement();
			// 4. SQL Execute
			rs = st.executeQuery("select * from emp");
			while(rs.next()) {
				System.out.println(rs.getString("ename") + " , " + rs.getInt("sal"));
			}
		} catch (Exception e) {
			System.out.println("class loading fail");
		} finally {
			// 5. close
			try { if(rs != null) rs.close();} catch (SQLException e) {};
			try { if(st != null) st.close();} catch (SQLException e) {};
			try { if(con != null) con.close();} catch (SQLException e) {};
		}
	}

}
```

- ResultSet -> Statement -> Connection 순으로 close하는게 좋지만 Connection이 close되면 나머지도 자동 close된다.

- 쿼리를 실행하는 Statement의 메서드는 boolean execute() -모든 쿼리 사용 , int executeUpdate() - insert, update, delete에 사용, ResultSet execute() - select 쿼리에 주로 사용합니다.