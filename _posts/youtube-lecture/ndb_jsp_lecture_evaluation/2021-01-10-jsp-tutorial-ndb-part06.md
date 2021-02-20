---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 6강"
excerpt: "21/01/10 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-10"
toc: true
toc_sticky: true
---
# 6강 - 로그인 및 로그아웃 화면 디자인

## userLogin.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
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
              <a class="dropdown-item" href="userRegister.jsp">회원가입</a>
              <a class="dropdown-item" href="userLogout.jsp">로그아웃</a>
            </div>
          </li>
        </ul>
        <form action="./index.jsp" method="get" class="form-inline my-2 my-lg-0">
          <input type="text" name="search" class="form-control mr-sm-2" placeholder="내용을 입력하세요.">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">검색</button>
        </form>
      </div>
    </nav>
    <div class="container mt-3" style="max-width: 560px;">
      <form method="post" action="./userLoginAction.jsp">
        <div class="form-group">
          <label>아이디</label>
          <input type="text" name="userID" class="form-control">
        </div>
        <div class="form-group">
          <label>비밀번호</label>
          <input type="password" name="userPassword" class="form-control">
        </div>
        <button type="submit" class="btn btn-primary">로그인</button>
      </form>
    </div>
    <footer class="bg-dark mt-4 p-5 text-center" style="color: #FFFFFF;">
      Copyright ⓒ 2021 송지훈 All Rights Reserved.
    </footer>
    <!-- 제이쿼리 자바스크립트 추가하기 -->
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <!-- Popper 자바스크립트 추가하기 -->
    <script src="https://unpkg.com/popper.js@1.12.9/dist/umd/popper.min.js"></script>
    <!-- 부트스트랩 자바스크립트 추가하기 -->
    <script src="./js/bootstrap.min.js"></script>
  </body>
</html>
```

## userRegister.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
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
              <a class="dropdown-item" href="userRegister.jsp">회원가입</a>
              <a class="dropdown-item" href="userLogout.jsp">로그아웃</a>
            </div>
          </li>
        </ul>
        <form action="./index.jsp" method="get" class="form-inline my-2 my-lg-0">
          <input type="text" name="search" class="form-control mr-sm-2" placeholder="내용을 입력하세요.">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">검색</button>
        </form>
      </div>
    </nav>
    <div class="container mt-3" style="max-width: 560px;">
      <form method="post" action="./userRegisterAction.jsp">
        <div class="form-group">
          <label>아이디</label>
          <input type="text" name="userID" class="form-control">
        </div>
        <div class="form-group">
          <label>비밀번호</label>
          <input type="password" name="userPassword" class="form-control">
        </div>
        <div class="form-group">
          <label>이메일</label>
          <input type="email" name="userEmail" class="form-control">
        </div>
        <button type="submit" class="btn btn-primary">회원가입</button>
      </form>
    </div>
    <footer class="bg-dark mt-4 p-5 text-center" style="color: #FFFFFF;">
    	Copyright ⓒ 2021 송지훈 All Rights Reserved.
    </footer>
    <!-- 제이쿼리 자바스크립트 추가하기 -->
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <!-- Popper 자바스크립트 추가하기 -->
    <script src="https://unpkg.com/popper.js@1.12.9/dist/umd/popper.min.js"></script>
    <!-- 부트스트랩 자바스크립트 추가하기 -->
    <script src="./js/bootstrap.min.js"></script>
  </body>
</html>
```

## 결과 화면

![image](https://user-images.githubusercontent.com/43688074/104116409-a036b000-535b-11eb-9f0e-3cf1ebce7982.png)

![image](https://user-images.githubusercontent.com/43688074/104116416-adec3580-535b-11eb-9ae8-fb10be936096.png)