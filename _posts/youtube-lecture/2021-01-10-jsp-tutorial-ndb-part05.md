---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 5강"
excerpt: "21/01/10 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-10"
toc: true
toc_sticky: true
---
# 5강 - 강의평가 메인화면 웹 디자인

## index.jsp

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
    <div class="container">
      <form method="get" action="./index.jsp" class="form-inline mt-3">
        <select name="lectureDivide" class="form-control mx-1 mt-2">
          <option value="전체">전체</option>
          <option value="전공">전공</option>
          <option value="교양">교양</option>
          <option value="기타">기타</option>
        </select>
        <input type="text" name="search" class="form-control mx-1 mt-2" placeholder="내용을 입력하세요.">
        <button type="submit" class="btn btn-primary mx-1 mt-2">검색</button>
        <a class="btn btn-primary mx-1 mt-2" data-toggle="modal" href="#registerModal">등록하기</a>
        <a class="btn btn-danger ml-1 mt-2" data-toggle="modal" href="#reportModal">신고</a>
      </form>
      <div class="card bg-light mt-3">
        <div class="card-header bg-light">
          <div class="row">
            <div class="col-8 text-left">컴퓨터개론&nbsp;<small>나동빈</small></div>
            <div class="col-4 text-right">
              종합 <span style="color: red;">A</span>
            </div>
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title">
            정말 좋은 강의에요.&nbsp;<small>(2017년 가을학기)</small>
          </h5>
          <p class="card-text">강의가 많이 널널해서, 솔직히 많이 배운 건 없는 것 같지만 학점도 잘 나오고 너무 좋은 것 같습니다.</p>
          <div class="row">
            <div class="col-9 text-left">
              성적 <span style="color: red;">A</span>
              널널 <span style="color: red;">A</span>
              강의 <span style="color: red;">B</span>
              <span style="color: green;">(추천: 15★)</span>
            </div>
            <div class="col-3 text-right">
              <a onclick="return confirm('추천하시겠습니까?')" href="./likeAction.jsp?evaluationID=">추천</a>
              <a onclick="return confirm('삭제하시겠습니까?')" href="./deleteAction.jsp?evaluationID=">삭제</a>
            </div>
          </div>
        </div>
      </div>
      <div class="card bg-light mt-3">
        <div class="card-header bg-light">
          <div class="row">
            <div class="col-8 text-left">컴퓨터그래픽스&nbsp;<small>홍길동</small></div>
            <div class="col-4 text-right">
              종합 <span style="color: red;">B</span>
            </div>
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title">
            나쁘지 않은 것 같습니다.&nbsp;<small>(2017년 여름학기)</small>
          </h5>
          <p class="card-text">컴퓨터그래픽스를 처음 배웠는데, 상당히 재미있었던 것 같아요.</p>
          <div class="row">
            <div class="col-9 text-left">
              성적 <span style="color: red;">B</span>
              널널 <span style="color: red;">C</span>
              강의 <span style="color: red;">B</span>
              <span style="color: green;">(추천: 1)</span>
            </div>
            <div class="col-3 text-right">
              <a onclick="return confirm('추천하시겠습니까?')" href="./likeAction.jsp?evaluationID=">추천</a>
              <a onclick="return confirm('삭제하시겠습니까?')" href="./deleteAction.jsp?evaluationID=">삭제</a>
            </div>
          </div>
        </div>
      </div>
      <div class="card bg-light mt-3">
        <div class="card-header bg-light">
          <div class="row">
            <div class="col-8 text-left">알고리즘&nbsp;<small>이순신</small></div>
            <div class="col-4 text-right">
              종합 <span style="color: red;">A</span>
            </div>
          </div>
        </div>
        <div class="card-body">
          <h5 class="card-title">
            강의력이 제일 좋은 강의입니다.&nbsp;<small>(2017년 2학기)</small>
          </h5>
          <p class="card-text">알고리즘 강의 가르치시는 교수님들 중에서 최고로 잘 가르치십니다.</p>
          <div class="row">
            <div class="col-9 text-left">
              성적 <span style="color: red;">A</span>
              널널 <span style="color: red;">C</span>
              강의 <span style="color: red;">A</span>
              <span style="color: green;">(추천: 0)</span>
            </div>
            <div class="col-3 text-right">
              <a onclick="return confirm('추천하시겠습니까?')" href="./likeAction.jsp?evaluationID=">추천</a>
              <a onclick="return confirm('삭제하시겠습니까?')" href="./deleteAction.jsp?evaluationID=">삭제</a>
            </div>
          </div>
        </div>
      </div>
    </div>
    <ul class="pagination justify-content-center mt-3">
      <li class="page-item">
        <a class="page-link" href="#">이전</a>
      </li>
      <li class="page-item">
        <a class="page-link" href="#">다음</a>
      </li>
    </ul>
    <div class="modal fade" id="registerModal" tabindex="-1" role="dialog" aria-labelledby="modal" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="modal">평가 등록</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form action="./evaluationRegisterAction.jsp" method="post">
              <div class="form-row">
                <div class="form-group col-sm-6">
                  <label>강의명</label>
                  <input type="text" name="lectureName" class="form-control" maxlength="20">
                </div>
                <div class="form-group col-sm-6">
                  <label>교수명</label>
                  <input type="text" name="professorName" class="form-control" maxlength="20">
                </div>
              </div>
              <div class="form-row">
                <div class="form-group col-sm-4">
                  <label>수강 연도</label>
                  <select name="lectureYear" class="form-control">
                    <option value="2011">2011</option>
                    <option value="2012">2012</option>
                    <option value="2013">2013</option>
                    <option value="2014">2014</option>
                    <option value="2015">2015</option>
                    <option value="2016">2016</option>
                    <option value="2017">2017</option>
                    <option value="2018" selected>2018</option>
                    <option value="2019">2019</option>
                    <option value="2020">2020</option>
                    <option value="2021">2021</option>
                    <option value="2022">2022</option>
                    <option value="2023">2023</option>
                  </select>
                </div>
                <div class="form-group col-sm-4">
                  <label>수강 학기</label>
                  <select name="semesterDivide" class="form-control">
                    <option name="1학기" selected>1학기</option>
                    <option name="여름학기">여름학기</option>
                    <option name="2학기">2학기</option>
                    <option name="겨울학기">겨울학기</option>
                  </select>
                </div>
                <div class="form-group col-sm-4">
                  <label>강의 구분</label>
                  <select name="lectureDivide" class="form-control">
                    <option name="전공" selected>전공</option>
                    <option name="교양">교양</option>
                    <option name="기타">기타</option>
                  </select>
                </div>
              </div>
              <div class="form-group">
                <label>제목</label>
                <input type="text" name="evaluationTitle" class="form-control" maxlength="20">
              </div>
              <div class="form-group">
                <label>내용</label>
                <textarea type="text" name="evaluationContent" class="form-control" maxlength="2048" style="height: 180px;"></textarea>
              </div>
              <div class="form-row">
                <div class="form-group col-sm-3">
                  <label>종합</label>
                  <select name="totalScore" class="form-control">
                    <option value="A" selected>A</option>
                    <option value="B">B</option>
                    <option value="C">C</option>
                    <option value="D">D</option>
                    <option value="F">F</option>
                  </select>
                </div>
                <div class="form-group col-sm-3">
                  <label>성적</label>
                  <select name="creditScore" class="form-control">
                    <option value="A" selected>A</option>
                    <option value="B">B</option>
                    <option value="C">C</option>
                    <option value="D">D</option>
                    <option value="F">F</option>
                  </select>
                </div>
                <div class="form-group col-sm-3">
                  <label>널널</label>
                  <select name="comfortableScore" class="form-control">
                    <option value="A" selected>A</option>
                    <option value="B">B</option>
                    <option value="C">C</option>
                    <option value="D">D</option>
                    <option value="F">F</option>
                  </select>
                </div>
                <div class="form-group col-sm-3">
                  <label>강의</label>
                  <select name="lectureScore" class="form-control">
                    <option value="A" selected>A</option>
                    <option value="B">B</option>
                    <option value="C">C</option>
                    <option value="D">D</option>
                    <option value="F">F</option>
                  </select>
                </div>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">취소</button>
                <button type="submit" class="btn btn-primary">등록하기</button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div class="modal fade" id="reportModal" tabindex="-1" role="dialog" aria-labelledby="modal" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="modal">신고하기</h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form method="post" action="./reportAction.jsp">
              <div class="form-group">
                <label>신고 제목</label>
                <input type="text" name="reportTitle" class="form-control" maxlength="20">
              </div>
              <div class="form-group">
                <label>신고 내용</label>
                <textarea type="text" name="reportContent" class="form-control" maxlength="2048" style="height: 180px;"></textarea>
              </div>
              <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">취소</button>
                <button type="submit" class="btn btn-danger">신고하기</button>
              </div>
            </form>
          </div>
        </div>
      </div>
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

## 메인화면
![image](https://user-images.githubusercontent.com/43688074/104112440-3524b380-5332-11eb-9f1b-bc77d6228131.png)


## 등록하기 버튼
![image](https://user-images.githubusercontent.com/43688074/104112446-4d94ce00-5332-11eb-8553-be4f18baf3b0.png)

## 신고 버튼
![image](https://user-images.githubusercontent.com/43688074/104112452-600f0780-5332-11eb-8cdc-e656177e3b22.png)

