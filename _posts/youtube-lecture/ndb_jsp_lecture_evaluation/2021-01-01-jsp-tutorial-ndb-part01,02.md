---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 1~2강"
excerpt: "21/01/01 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-02"
toc: true
toc_sticky: true
---
# 1강 - 강의 소개 및 강사 소개
* 웹 브라우저: 크롬(Chrome)
* 프로그래밍 개발 환경: 자바 개발 키트(JDK)
* JEE 개발 환경: 이클립스(Eclipse)
* 데이터베이스: MySQL
* 웹 컨테이너: 톰캣(Tomcat)

&nbsp;

* 웹 컨테이너

> 웹 컨테이너(web container, 또는 서블릿 컨테이너)는 웹 서버의 컴포넌트 중 하나로 자바 서블릿과 상호작용한다. 웹 컨테이너는 서블릿의 생명주기를 관리하고, URL과 특정 서블릿을 맵핑하며 URL 요청이 올바른 접근 권한을 갖도록 보장한다.



> 웹 컨테이너는 서블릿, 자바서버 페이지(JSP) 파일, 그리고 서버-사이드 코드가 포함된 다른 타입의 파일들에 대한 요청을 다룬다. 웹 컨테이너는 서블릿 객체를 생성하고, 서블릿을 로드와 언로드하며, 요청과 응답 객체를 생성하고 관리하고, 다른 서블릿 관리 작업을 수행한다.



> 웹 컨테이너는 웹 컴포넌트 자바 EE 아키텍처 제약을 구현하고, 보안, 병행성, 생명주기 관리, 트랜잭션, 배포 등 다른 서비스를 포함하는 웹 컴포넌트의 실행 환경을 명세한다.

&nbsp;

# 2강 - JSP 개발환경 구축 및 테스트
* 오라클을 깔면 Tomcat 기본 포트인 8080과 겹친다.
* 현재 Tomcat은 8008포트로 수정
* http://xxx.com:80 (http 포트)
* https://naver.com:443 (https 포트)

&nbsp;

## JSP 프로젝트 만들기
![image](https://user-images.githubusercontent.com/43688074/103453561-b1117100-4d1e-11eb-8403-b3da30f35a0a.png)
## 런타임 찾기
![image](https://user-images.githubusercontent.com/43688074/103453576-cf776c80-4d1e-11eb-8732-24df0ebe7720.png)
## 톰캣 버전 선택
![image](https://user-images.githubusercontent.com/43688074/103453578-d30af380-4d1e-11eb-8bb2-4065f053a446.png)
## 톰캣 설치 위치를 복사
![image](https://user-images.githubusercontent.com/43688074/103453581-d605e400-4d1e-11eb-9000-2d63a565cd0b.png)
## 최종 프로젝트 생성창
![image](https://user-images.githubusercontent.com/43688074/103453610-0483bf00-4d1f-11eb-849c-318ace28a3f1.png)
## JSP 파일 만들기
![image](https://user-images.githubusercontent.com/43688074/103453614-0a79a000-4d1f-11eb-9cff-e3c52cec1aee.png)
## index.jsp
![image](https://user-images.githubusercontent.com/43688074/103453615-0c436380-4d1f-11eb-8c9e-5d441b1c46f8.png)
## index.jsp 코드
![image](https://user-images.githubusercontent.com/43688074/103453616-0e0d2700-4d1f-11eb-8706-fd6aad5e14b8.png)
## 서버 실행
![image](https://user-images.githubusercontent.com/43688074/103453618-11081780-4d1f-11eb-9a0b-368d21f5c7d5.png)
## 서버 선택
![image](https://user-images.githubusercontent.com/43688074/103453621-12d1db00-4d1f-11eb-927b-3362bde10f84.png)
## 최종 화면
![image](https://user-images.githubusercontent.com/43688074/103455227-b70f4e00-4d2e-11eb-93a6-1a857863a44e.png)
