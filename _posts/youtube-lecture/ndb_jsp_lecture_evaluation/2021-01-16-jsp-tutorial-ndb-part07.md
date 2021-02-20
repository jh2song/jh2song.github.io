---
title: "JSP 강의평가 웹 사이트 개발하기 강의노트 - 7강"
excerpt: "21/01/16 - 강의 노트"
tags: ["youtube-lecture", "jsp"]
categories: ["youtube-lecture"]
last_modified_at: "2021-01-16"
toc: true
toc_sticky: true
---
# 7강 - 강의평가 데이터베이스 구축하기

```sql
Microsoft Windows [Version 10.0.19041.746]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\Users\spec0>mysql -u root -p
Enter password: ****
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.5.5-MariaDB mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> CREATE DATABASE LectureEvaluation;
Query OK, 1 row affected (0.001 sec)

MariaDB [(none)]> USE LectureEvaluation;
Database changed
MariaDB [LectureEvaluation]> CREATE TABLE USER (
    -> userID varchar(20),
    -> userPassword varchar(64),
    -> userEmail varchar(50),
    -> userEmailHash varchar(64),
    -> userEmailChecked boolean
    -> );
Query OK, 0 rows affected (0.020 sec)

MariaDB [LectureEvaluation]> DESC USER;
+------------------+-------------+------+-----+---------+-------+
| Field            | Type        | Null | Key | Default | Extra |
+------------------+-------------+------+-----+---------+-------+
| userID           | varchar(20) | YES  |     | NULL    |       |
| userPassword     | varchar(64) | YES  |     | NULL    |       |
| userEmail        | varchar(50) | YES  |     | NULL    |       |
| userEmailHash    | varchar(64) | YES  |     | NULL    |       |
| userEmailChecked | tinyint(1)  | YES  |     | NULL    |       |
+------------------+-------------+------+-----+---------+-------+
5 rows in set (0.022 sec)

MariaDB [LectureEvaluation]> ALTER TABLE USER ADD PRIMARY KEY (userID);
Query OK, 0 rows affected (0.031 sec)
Records: 0  Duplicates: 0  Warnings: 0

MariaDB [LectureEvaluation]> DESC USER;
+------------------+-------------+------+-----+---------+-------+
| Field            | Type        | Null | Key | Default | Extra |
+------------------+-------------+------+-----+---------+-------+
| userID           | varchar(20) | NO   | PRI | NULL    |       |
| userPassword     | varchar(64) | YES  |     | NULL    |       |
| userEmail        | varchar(50) | YES  |     | NULL    |       |
| userEmailHash    | varchar(64) | YES  |     | NULL    |       |
| userEmailChecked | tinyint(1)  | YES  |     | NULL    |       |
+------------------+-------------+------+-----+---------+-------+
5 rows in set (0.020 sec)

MariaDB [LectureEvaluation]> CREATE TABLE EVALUATION (
    -> evaluationID int PRIMARY KEY AUTO_INCREMENT,
    -> userID varchar(20),
    -> lectureName varchar(50),
    -> professorName varchar(20),
    -> lectureYear int,
    -> semesterDivide varchar(20),
    -> lectureDivide varchar(10),
    -> evaluationTitle varchar(50),
    -> evaluationContent varchar(2048),
    -> totalScore varchar(5),
    -> creditScore varchar(5),
    -> comfortableScore varchar(5),
    -> lectureScore varchar(5),
    -> likeCount int
    -> );
Query OK, 0 rows affected (0.020 sec)

MariaDB [LectureEvaluation]> DESC EVALUATION;
+-------------------+---------------+------+-----+---------+----------------+
| Field             | Type          | Null | Key | Default | Extra          |
+-------------------+---------------+------+-----+---------+----------------+
| evaluationID      | int(11)       | NO   | PRI | NULL    | auto_increment |
| userID            | varchar(20)   | YES  |     | NULL    |                |
| lectureName       | varchar(50)   | YES  |     | NULL    |                |
| professorName     | varchar(20)   | YES  |     | NULL    |                |
| lectureYear       | int(11)       | YES  |     | NULL    |                |
| semesterDivide    | varchar(20)   | YES  |     | NULL    |                |
| lectureDivide     | varchar(10)   | YES  |     | NULL    |                |
| evaluationTitle   | varchar(50)   | YES  |     | NULL    |                |
| evaluationContent | varchar(2048) | YES  |     | NULL    |                |
| totalScore        | varchar(5)    | YES  |     | NULL    |                |
| creditScore       | varchar(5)    | YES  |     | NULL    |                |
| comfortableScore  | varchar(5)    | YES  |     | NULL    |                |
| lectureScore      | varchar(5)    | YES  |     | NULL    |                |
| likeCount         | int(11)       | YES  |     | NULL    |                |
+-------------------+---------------+------+-----+---------+----------------+
14 rows in set (0.019 sec)

MariaDB [LectureEvaluation]> CREATE TABLE LIKEY (
    -> userID varchar(20),
    -> evaluationID int,
    -> userIP varchar(50)
    -> );
Query OK, 0 rows affected (0.021 sec)

MariaDB [LectureEvaluation]> show tables;
+-----------------------------+
| Tables_in_lectureevaluation |
+-----------------------------+
| evaluation                  |
| likey                       |
| user                        |
+-----------------------------+
3 rows in set (0.000 sec)

MariaDB [LectureEvaluation]> DESC EVALUATION;
+-------------------+---------------+------+-----+---------+----------------+
| Field             | Type          | Null | Key | Default | Extra          |
+-------------------+---------------+------+-----+---------+----------------+
| evaluationID      | int(11)       | NO   | PRI | NULL    | auto_increment |
| userID            | varchar(20)   | YES  |     | NULL    |                |
| lectureName       | varchar(50)   | YES  |     | NULL    |                |
| professorName     | varchar(20)   | YES  |     | NULL    |                |
| lectureYear       | int(11)       | YES  |     | NULL    |                |
| semesterDivide    | varchar(20)   | YES  |     | NULL    |                |
| lectureDivide     | varchar(10)   | YES  |     | NULL    |                |
| evaluationTitle   | varchar(50)   | YES  |     | NULL    |                |
| evaluationContent | varchar(2048) | YES  |     | NULL    |                |
| totalScore        | varchar(5)    | YES  |     | NULL    |                |
| creditScore       | varchar(5)    | YES  |     | NULL    |                |
| comfortableScore  | varchar(5)    | YES  |     | NULL    |                |
| lectureScore      | varchar(5)    | YES  |     | NULL    |                |
| likeCount         | int(11)       | YES  |     | NULL    |                |
+-------------------+---------------+------+-----+---------+----------------+
14 rows in set (0.014 sec)

MariaDB [LectureEvaluation]>
```