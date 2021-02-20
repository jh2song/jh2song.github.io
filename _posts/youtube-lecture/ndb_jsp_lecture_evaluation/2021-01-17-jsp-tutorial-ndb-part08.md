---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 8강"
excerpt: "21/01/17 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-17"
toc: true
toc_sticky: true
---
# 8강 - 회원 데이터 모델링

## 소스 구조

![image](https://user-images.githubusercontent.com/43688074/104828424-90afed80-58ac-11eb-8e10-42adfe936802.png)

## util-DatabaseUtil.java

```java
package util;

import java.sql.Connection;
import java.sql.DriverManager;

public class DatabaseUtil {
	public static Connection getConnection() {
		try {
			String dbURL = "jdbc:mariadb://localhost:3306/LectureEvaluation";
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

## user-UserDTO.java

```java
package user;

public class UserDTO {

	private String userID;
	private String userPassword;
	private String userEmail;
	private String userEmailHash;
	private boolean userEmailChecked;

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

	public String getUserEmail() {
		return userEmail;
	}

	public void setUserEmail(String userEmail) {
		this.userEmail = userEmail;
	}

	public String getUserEmailHash() {
		return userEmailHash;
	}

	public void setUserEmailHash(String userEmailHash) {
		this.userEmailHash = userEmailHash;
	}

	public boolean isUserEmailChecked() {
		return userEmailChecked;
	}

	public void setUserEmailChecked(boolean userEmailChecked) {
		this.userEmailChecked = userEmailChecked;
	}

	public UserDTO() {
		
	}
	
	public UserDTO(String userID, String userPassword, String userEmail,
			String userEmailHash, boolean userEmailChecked) {
		this.userID = userID;
		this.userPassword = userPassword;
		this.userEmail = userEmail;
		this.userEmailHash = userEmailHash;
		this.userEmailChecked = userEmailChecked;
	}
}
```

## user-UserDAO.java

```java
package user;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

import util.DatabaseUtil;

public class UserDAO {
	
	public int login(String userID, String userPassword) {
		String SQL = "SELECT userPassword FROM USER WHERE userID = ?";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = DatabaseUtil.getConnection();
			pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, userID);
			rs = pstmt.executeQuery();
			if(rs.next()) {
				if(rs.getString(1).equals(userPassword))
					return 1; // 로그인 성공
				else
					return 0; // 비밀번호 틀림
			}
			return -1; // 아이디 없음
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {if (conn != null) conn.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (pstmt != null) pstmt.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (rs != null) rs.close(); } catch (Exception e) {e.printStackTrace();}
		}
		return -2; // 데이터베이스 오류
	}

	public int join(UserDTO user) {
		String SQL = "INSERT INTO USER VALUES (?, ?, ?, ?, false)";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = DatabaseUtil.getConnection();
			pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, user.getUserID());
			pstmt.setString(2, user.getUserPassword());
			pstmt.setString(3, user.getUserEmail());
			pstmt.setString(4, user.getUserEmailHash());
			return pstmt.executeUpdate();
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {if (conn != null) conn.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (pstmt != null) pstmt.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (rs != null) rs.close(); } catch (Exception e) {e.printStackTrace();}
		}
		return -1; // 회원가입 실패
	}

	public String getUserEmail(String userID) {
		String SQL = "SELECT userEmail FROM USER WHERE userID = ?";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = DatabaseUtil.getConnection();
			pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, userID);
			rs = pstmt.executeQuery();
			while(rs.next()) {
				return rs.getString(1); // 이메일 주소 반환
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {if (conn != null) conn.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (pstmt != null) pstmt.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (rs != null) rs.close(); } catch (Exception e) {e.printStackTrace();}
		}
		return null; // 데이터베이스 오류
	}

	public boolean getUserEmailChecked(String userID) {
		String SQL = "SELECT userEmailChecked FROM USER WHERE userID = ?";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = DatabaseUtil.getConnection();
			pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, userID);
			rs = pstmt.executeQuery();
			while(rs.next()) {
				return rs.getBoolean(1); // 이메일 등록 여부 반환
			}
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {if (conn != null) conn.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (pstmt != null) pstmt.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (rs != null) rs.close(); } catch (Exception e) {e.printStackTrace();}
		}
		return false; // 데이터베이스 오류
	}

	public boolean setUserEmailChecked(String userID) {
		String SQL = "UPDATE USER SET userEmailChecked = true WHERE userID = ?";
		Connection conn = null;
		PreparedStatement pstmt = null;
		ResultSet rs = null;
		try {
			conn = DatabaseUtil.getConnection();
			pstmt = conn.prepareStatement(SQL);
			pstmt.setString(1, userID);
			pstmt.executeUpdate();
			return true; // 이메일 등록 설정 성공
		} catch (SQLException e) {
			e.printStackTrace();
		} finally {
			try {if (conn != null) conn.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (pstmt != null) pstmt.close(); } catch (Exception e) {e.printStackTrace();}
			try {if (rs != null) rs.close(); } catch (Exception e) {e.printStackTrace();}
		}
		return false; // 이메일 등록 설정 실패
	}
}
```