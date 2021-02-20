---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 3강"
excerpt: "21/01/03 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-03"
toc: true
toc_sticky: true
---
# 3강 - MySQL과 JSP 연동 및 실습

## Mysql(MariaDB) 테이블 생성
```sql
C:\Users\spec0>mysql -u root -p
MariaDB [(none)]> CREATE DATABASE TUTORIAL;
MariaDB [(none)]> USE TUTORIAL;

MariaDB [TUTORIAL]> SHOW TABLES;
Empty set (0.001 sec)

MariaDB [TUTORIAL]> CREATE TABLE USER (
    -> userID VARCHAR(20),
    -> userPassword VARCHAR(64)
    -> );
Query OK, 0 rows affected (0.039 sec)

MariaDB [TUTORIAL]> SHOW TABLES;
+--------------------+
| Tables_in_tutorial |
+--------------------+
| user               |
+--------------------+
1 row in set (0.000 sec)

MariaDB [TUTORIAL]> INSERT INTO USER VALUES ('1', '123');
Query OK, 1 row affected (0.048 sec)

MariaDB [TUTORIAL]> SELECT * FROM USER;
+--------+--------------+
| userID | userPassword |
+--------+--------------+
| 1      | 123          |
+--------+--------------+
1 row in set (0.003 sec)
```

## 이클립스에서 패키지 만들기
![image](https://user-images.githubusercontent.com/43688074/103469603-4574e580-4daa-11eb-8cf3-4ae775266dae.png)
![image](https://user-images.githubusercontent.com/43688074/103469606-502f7a80-4daa-11eb-81ba-be25a66c1124.png)

## UserDTO 생성
* DTO: JSP 프로그램 안에서 임시적으로 하나의 데이터 단위를 담기 위한 용도로 정의되는 객체

![image](https://user-images.githubusercontent.com/43688074/103469621-9389e900-4daa-11eb-9eed-c6bfe6402912.png)
![image](https://user-images.githubusercontent.com/43688074/103469623-9a186080-4daa-11eb-90d5-5cf81aa5abc0.png)

## UserDTO 소스
![image](https://user-images.githubusercontent.com/43688074/103469664-fda28e00-4daa-11eb-94c7-c6ebeae31159.png)

```java
// userDTO.java
package user;

public class UserDTO {
	String userID;
	String userPassword;
	
	public String getUserID() {
		return userID;
	}
	public void setUserID(String userID) {
		this.userID = userID;
	}
	public String getUserPassword() {
		return userPassword;
	}
	public void setUserPassword(String userPassword) {
		this.userPassword = userPassword;
	}
}
```

## UserDAO 생성
* DAO: 데이터베이스 연동

![image](https://user-images.githubusercontent.com/43688074/103469682-422e2980-4dab-11eb-86b6-eac77e1f40df.png)
![image](https://user-images.githubusercontent.com/43688074/103469697-5eca6180-4dab-11eb-8ad8-5bc8c0494343.png)

## Util 생성
![image](https://user-images.githubusercontent.com/43688074/103469717-9df8b280-4dab-11eb-8279-5038a554f396.png)
![image](https://user-images.githubusercontent.com/43688074/103469729-c41e5280-4dab-11eb-8284-9300c0b987e1.png)
![image](https://user-images.githubusercontent.com/43688074/103469734-e4e6a800-4dab-11eb-8443-a3a6289d7c5c.png)
![image](https://user-images.githubusercontent.com/43688074/103469742-fe87ef80-4dab-11eb-9b0d-e7852add77ee.png)

## DatabaseUtil.java 소스
```java
// DatabaseUtil.java
package util;

import java.sql.Connection;
import java.sql.DriverManager;

public class DatabaseUtil {
	public static Connection getConnection() {
		try {
			String dbURL = "jdbc:mariadb://localhost:3306/TUTORIAL";
			String dbID = "root";
			String dbPassword = "1234";
			Class.forName("org.mariadb.jdbc.Driver");
			return DriverManager.getConnection(dbURL, dbID, dbPassword);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return null;
	}
}
```

## UserDAO.java 소스
```java
// UserDAO.java
package user;

import java.sql.Connection;
import java.sql.PreparedStatement;

import util.DatabaseUtil;

public class UserDAO {
	public int join(String userID, String userPassword) {
		String SQL = "INSERT INTO USER VALUES (?, ?)";
		try {
			Connection conn = DatabaseUtil.getConnection();
			PreparedStatement pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, userID);
			pstmt.setString(2, userPassword);
			return pstmt.executeUpdate();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return -1;
	}
}
```

## index.jsp 소스
```jsp
<%@ page language="java" contentType="text/html; charset=EUC-KR"
    pageEncoding="EUC-KR"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>우리의 첫 번째 페이지</title>
</head>
<body>
	Hello World!
	<form action="./userJoinAction.jsp" method="post">
		<input type="text" name="userID">
		<input type="password" name="userPassword">
		<input type="submit" value="회원가입">
	</form>
</body>
</html>
```

## userJoinAction.jsp 생성
![image](https://user-images.githubusercontent.com/43688074/103469878-021c7600-4dae-11eb-8653-86e6f68c1e28.png)
![image](https://user-images.githubusercontent.com/43688074/103469871-f466f080-4dad-11eb-890d-c2cc7025d86c.png)


## userJoinAction.jsp 소스
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="user.UserDTO"%>
<%@ page import="user.UserDAO"%>
<%@ page import="java.io.PrintWriter"%>
<%
	request.setCharacterEncoding("UTF-8");
	String userID = null;
	String userPassword = null;
	if(request.getParameter("userID") != null) {
		userID = (String)request.getParameter("userID");
	}
	if(request.getParameter("userPassword") != null) {
		userPassword = (String)request.getParameter("userPassword");
	}
	if(userID == null || userPassword == null) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('입력이 안 된 사항이 있습니다.');");
		script.println("history.back();");
		script.println("</script>");
		script.close();
		return;
	}
	UserDAO userDAO = new UserDAO();
	int result = userDAO.join(userID, userPassword);
	if (result == 1) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('회원가입에 성공했습니다.');");
		script.println("location.href = 'index.jsp';");
		script.println("</script>");
		script.close();
		return;
	}
%>
```

## MariaDB 드라이버 프로젝트 포함
![image](https://user-images.githubusercontent.com/43688074/103470035-3f820300-4db0-11eb-9abe-66ca3df291bd.png)

## 최종 결과
![image](https://user-images.githubusercontent.com/43688074/103470052-81ab4480-4db0-11eb-82c9-717c872eabeb.png)
![image](https://user-images.githubusercontent.com/43688074/103470046-6f310b00-4db0-11eb-8a40-ba8bd5a9156d.png)
![image](https://user-images.githubusercontent.com/43688074/103470062-97b90500-4db0-11eb-98ad-87746a4e0a5d.png)
