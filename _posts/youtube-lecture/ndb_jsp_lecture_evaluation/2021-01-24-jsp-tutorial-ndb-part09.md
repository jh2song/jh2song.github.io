---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 9강"
excerpt: "21/01/24 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-24"
toc: true
toc_sticky: true
---
# 9강 - 회원가입 및 이메일 인증 구현하기

## 프로그램 설치

![image](https://user-images.githubusercontent.com/43688074/105612607-b1be9400-5e00-11eb-9d74-1be1fac47581.png)

![image](https://user-images.githubusercontent.com/43688074/105612689-dca8e800-5e00-11eb-8345-a177098ebc38.png)

## 프로그램 디렉터리 구조

![image](https://user-images.githubusercontent.com/43688074/105612759-5640d600-5e01-11eb-8e05-1af554c73412.png)

## SHA256.java

```java
package util;

import java.security.MessageDigest;

public class SHA256 {
	
	public static String getSHA256(String input) {
		StringBuffer result = new StringBuffer();
		try {
			MessageDigest digest = MessageDigest.getInstance("SHA-256");
			byte[] salt = "Hello! This is Salt.".getBytes();
			digest.reset();
			digest.update(salt);
			byte[] chars = digest.digest(input.getBytes("UTF-8"));
			for(int i = 0; i < chars.length; i++) {
				String hex = Integer.toHexString(0xff & chars[i]);
				if(hex.length() == 1) result.append('0');
				result.append(hex);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}

		return result.toString();
	}
}
```

## Gmail.java

```java
package util;

import javax.mail.Authenticator;
import javax.mail.PasswordAuthentication;

public class Gmail extends Authenticator {
	
	@Override
    protected PasswordAuthentication getPasswordAuthentication() {
        return new PasswordAuthentication("아이디","비밀번호");
    }

}
```

## 프로그램 디렉터리 구조

![image](https://user-images.githubusercontent.com/43688074/105612907-71601580-5e02-11eb-9e60-35268df7a849.png)

## 구글 액세스 관리

* https://myaccount.google.com/security
* 보안 수준이 낮은 앱 허용

![image](https://user-images.githubusercontent.com/43688074/105612881-3c53c300-5e02-11eb-8c74-cf58a1ed4bd2.png)

## userRegisterAction.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ page import="user.UserDTO"%>
<%@ page import="user.UserDAO"%>
<%@ page import="util.SHA256"%>
<%@ page import="java.io.PrintWriter"%>
<%
	request.setCharacterEncoding("UTF-8");
	String userID = null;
	String userPassword = null;
	String userEmail = null;

	if(request.getParameter("userID") != null) {
		userID = (String) request.getParameter("userID");
	}

	if(request.getParameter("userPassword") != null) {
		userPassword = (String) request.getParameter("userPassword");
	}
	
	if(request.getParameter("userEmail") != null) {
		userEmail = (String) request.getParameter("userEmail");
	}

	if (userID == null || userPassword == null || userEmail == null) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('입력이 안 된 사항이 있습니다.');");
		script.println("history.back();");
		script.println("</script>");
		script.close();
	} else {
		UserDAO userDAO = new UserDAO();
		int result = userDAO.join(new UserDTO(userID, userPassword, userEmail, SHA256.getSHA256(userEmail), false));
		if (result == -1) {
			PrintWriter script = response.getWriter();
			script.println("<script>");
			script.println("alert('이미 존재하는 아이디입니다.');");
			script.println("history.back();");
			script.println("</script>");
			script.close();
		} else {
			session.setAttribute("userID", userID);
			PrintWriter script = response.getWriter();
			script.println("<script>");
			script.println("location.href = 'emailSendAction.jsp';");
			script.println("</script>");
			script.close();
		}
	}
%>
```

## MariaDB 드라이버 lib에 넣기

![image](https://user-images.githubusercontent.com/43688074/105613110-cfd9c380-5e03-11eb-9622-47ba3eb0bff3.png)


## DB

* 현재 비어있는 유저 테이블

![image](https://user-images.githubusercontent.com/43688074/105613033-46c28c80-5e03-11eb-82fd-35bdcd0ea22c.png)

&nbsp;

* 회원가입 이벤트

![image](https://user-images.githubusercontent.com/43688074/105613144-1a5b4000-5e04-11eb-9e61-f3900e4859c5.png)

![image](https://user-images.githubusercontent.com/43688074/105613159-4676c100-5e04-11eb-9cf0-946e7a1bf207.png)

![image](https://user-images.githubusercontent.com/43688074/105613176-66a68000-5e04-11eb-8a67-455871a7a244.png)

## emailSendAction.jsp

```jsp
<%@page import="javax.mail.Transport"%>
<%@page import="javax.mail.Message"%>
<%@page import="javax.mail.Address"%>
<%@page import="javax.mail.internet.InternetAddress"%>
<%@page import="javax.mail.internet.MimeMessage"%>
<%@page import="javax.mail.Session"%>
<%@page import="javax.mail.Authenticator"%>
<%@page import="java.util.Properties"%>
<%@page import="java.io.PrintWriter"%>
<%@page import="user.UserDAO"%>
<%@page import="util.SHA256"%>
<%@page import="util.Gmail"%>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	UserDAO userDAO = new UserDAO();
	String userID = null;
	
	if(session.getAttribute("userID") != null) {
		userID = (String) session.getAttribute("userID");
	}

	if(userID == null) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('로그인을 해주세요.');");
		script.println("location.href = 'userLogin.jsp'");
		script.println("</script>");
		script.close();
		return;
	}

	boolean emailChecked = userDAO.getUserEmailChecked(userID);

	if(emailChecked == true) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('이미 인증 된 회원입니다.');");
		script.println("location.href = 'index.jsp'");
		script.println("</script>");
		script.close();		
		return;
	}

	// 사용자에게 보낼 메시지를 기입합니다.
	String host = "http://localhost:8008/Lecture_Evaluation/";
	String from = "이메일 아이디";
	String to = userDAO.getUserEmail(userID);
	String subject = "강의평가를 위한 이메일 확인 메일입니다.";
	String content = "다음 링크에 접속하여 이메일 확인을 진행하세요." +
		"<a href='" + host + "emailCheckAction.jsp?code=" + new SHA256().getSHA256(to) + "'>이메일 인증하기</a>";

	// SMTP에 접속하기 위한 정보를 기입합니다.
	Properties p = new Properties();
	p.put("mail.smtp.user", from);
	p.put("mail.smtp.host", "smtp.googlemail.com");
	p.put("mail.smtp.port", "465");
	p.put("mail.smtp.starttls.enable", "true");
	p.put("mail.smtp.auth", "true");
	p.put("mail.smtp.debug", "true");
	p.put("mail.smtp.socketFactory.port", "465");
	p.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
	p.put("mail.smtp.socketFactory.fallback", "false");

	try{
	    Authenticator auth = new Gmail();
	    Session ses = Session.getInstance(p, auth);
	    ses.setDebug(true);
	    MimeMessage msg = new MimeMessage(ses); 
	    msg.setSubject(subject);
	    Address fromAddr = new InternetAddress(from);
	    msg.setFrom(fromAddr);
	    Address toAddr = new InternetAddress(to);
	    msg.addRecipient(Message.RecipientType.TO, toAddr);
	    msg.setContent(content, "text/html;charset=UTF-8");
	    Transport.send(msg);
	} catch(Exception e){
	    e.printStackTrace();
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('오류가 발생했습니다..');");
		script.println("history.back();");
		script.println("</script>");
		script.close();		
	    return;
	}
%>

<!doctype html>
<html>
  <head>
    <title>강의평가 웹 사이트</title>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <!-- 부트스트랩 CSS 추가하기 -->
    <link rel="stylesheet" href="./css/bootstrap.min.css">
    <!-- 커스텀 CSS 추가하기 -->
    <link rel="stylesheet" href="./css/custom.css">
  </head>

  <body>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <a class="navbar-brand" href="index.jsp">강의평가 웹 사이트</a>
      <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbar">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbar">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item active">
            <a class="nav-link" href="index.jsp">메인</a>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" id="dropdown" data-toggle="dropdown">
              	회원 관리
            </a>
            <div class="dropdown-menu" aria-labelledby="dropdown">
              <a class="dropdown-item" href="userLogin.jsp">로그인</a>
            </div>
          </li>
        </ul>
        <form action="./index.jsp" method="get" class="form-inline my-2 my-lg-0">
          <input type="text" name="search" class="form-control mr-sm-2" placeholder="내용을 입력하세요.">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">검색</button>
        </form>
      </div>
    </nav>
	<div class="container">
		 <div class="alert alert-success mt-4" role="alert">
		  이메일 주소 인증 메일이 전송되었습니다. 이메일에 들어가셔서 인증해주세요.
		</div>
    </div>
    <footer class="bg-dark mt-4 p-5 text-center" style="color: #FFFFFF;">
      Copyright ⓒ 2021 송지훈 All Rights Reserved.
    </footer>
    <!-- 제이쿼리 자바스크립트 추가하기 -->
    <script src="./js/jquery.min.js"></script>
    <!-- Popper 자바스크립트 추가하기 -->
    <script src="./js/popper.min.js"></script>
    <!-- 부트스트랩 자바스크립트 추가하기 -->
    <script src="./js/bootstrap.min.js"></script>
  </body>
</html>
```

## emailCheckAction.jsp

```jsp
<%@page import="java.io.PrintWriter"%>
<%@page import="util.SHA256"%>
<%@page import="user.UserDAO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
	request.setCharacterEncoding("UTF-8");
	String code = request.getParameter("code");
	UserDAO userDAO = new UserDAO();
	String userID = null;

	if(session.getAttribute("userID") != null) {
		userID = (String) session.getAttribute("userID");
	}

	if(userID == null) {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('로그인을 해주세요.');");
		script.println("location.href = 'userLogin.jsp'");
		script.println("</script>");
		script.close();
		return;
	}

	String userEmail = userDAO.getUserEmail(userID);
	boolean rightCode = (new SHA256().getSHA256(userEmail).equals(code)) ? true : false;
	
	if(rightCode == true) {
		userDAO.setUserEmailChecked(userID);
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('인증에 성공했습니다.');");
		script.println("location.href = 'index.jsp'");
		script.println("</script>");
		script.close();		
		return;
	} else {
		PrintWriter script = response.getWriter();
		script.println("<script>");
		script.println("alert('유효하지 않은 코드입니다.');");
		script.println("location.href = 'index.jsp'");
		script.println("</script>");
		script.close();		
		return;
	}
%>
```

## 결과 확인

![image](https://user-images.githubusercontent.com/43688074/105613366-61960080-5e05-11eb-9154-b52f0e92dfd5.png)

&nbsp;

* 회원가입 버튼 누르면 나오는 페이지

![image](https://user-images.githubusercontent.com/43688074/105613532-77f08c00-5e06-11eb-9b19-c123ef08209a.png)

&nbsp;

* 전송된 메일 (이메일 인증하기 링크 클릭)

![image](https://user-images.githubusercontent.com/43688074/105613566-9f475900-5e06-11eb-91b2-37e831957b9d.png)

&nbsp;

* 인증 성공 알람

![image](https://user-images.githubusercontent.com/43688074/105613877-b2f3bf00-5e08-11eb-9b6e-aae24f47ea92.png)

&nbsp;

* 테이블에 정상 반영

![image](https://user-images.githubusercontent.com/43688074/105613900-dcace600-5e08-11eb-9b96-32fbbc4c5f18.png)
