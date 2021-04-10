---
title: "생활코딩 WEB2 - Node.js 강의노트 - 끝까지"
excerpt: "21/04/10 - 강의 노트"
tags: ["youtube-lecture", "nodejs"]
categories: ["youtube-lecture"]
last_modified_at: "2021-04-10"
toc: true
toc_sticky: true
---

# 31강 25.1. JavaScript 함수의 기본 문법

```javascript
f123();
console.log("Test1");
f123();
console.log("Test2");
f123();
console.log("Test3");

function f123() {
    console.log(1);
    console.log(2);
    console.log(3);
    console.log(4);
}
```

<br>

# 32강 25.2. JavaScript 함수의 입력

```javascript
console.log(Math.round(1.6)); // 2
console.log(Math.round(1.4)); // 1

function sum(first, second) { // parameter
    console.log(first+second);
}

sum(2, 4); // 6 // argument
```

<br>

