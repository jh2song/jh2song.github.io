---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 4강"
excerpt: "21/01/03 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-03"
toc: true
toc_sticky: true
---
# 4강 - 프레임워크로 웹 디자인 틀 잡기

## 부트스트랩: 프레임워크 / jquery, popper: 라이브러리

&nbsp;

## custom.css

```css
@import url(https://fonts.googleapis.com/earlyaccess/jejugothic.css);
@import url(https://fonts.googleapis.com/earlyaccess/nanumgothic.css);
.navbar-brand, h1, h2, h3, h4 {
	font-family: 'Jeju Gothic';
}
h5 {
	font-family: 'Jeju Gothic';
	font-size: 18px
}
body {
	font-family: 'Nanum Gothic';
}
```

## index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
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
              <a class="dropdown-item" href="#">로그인</a>
              <a class="dropdown-item" href="#">회원가입</a>
              <a class="dropdown-item" href="#">로그아웃</a>
            </div>
          </li>
        </ul>
        <form class="form-inline my-2 my-lg-0">
          <input class="form-control mr-sm-2" type="search" placeholder="내용을 입력하세요." aria-label="Search">
          <button class="btn btn-outline-success my-2 my-sm-0" type="submit">검색</button>
        </form>
      </div>
    </nav>
    <!-- 제이쿼리 자바스크립트 추가하기 -->
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <!-- Popper 자바스크립트 추가하기 -->
    <script src="https://unpkg.com/popper.js@1.12.9/dist/umd/popper.min.js"></script>
    <!-- 부트스트랩 자바스크립트 추가하기 -->
    <script src="./js/bootstrap.min.js"></script>
  </body>
</html>
```

## 구조
![image](https://user-images.githubusercontent.com/43688074/103476622-dd022480-4dfa-11eb-8db6-678991b1144b.png)

## 동작 사진
![image](https://user-images.githubusercontent.com/43688074/103476633-f30fe500-4dfa-11eb-9617-66998bbe219c.png)
![image](https://user-images.githubusercontent.com/43688074/103476642-06bb4b80-4dfb-11eb-87dc-980ccd889fb8.png)