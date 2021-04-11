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

# 33강 25.3. JavaScript-함수의 출력

```javascript
function sum2(first, second) {
    return first + second;
}

console.log(sum2(4,5)); // 9
```

<br>

# 34강 26. App 제작-함수를 이용해서 정리 정돈하기

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

function templateHTML(title, list, body) {
    return `
    <!doctype html>
    <html>
        <head>
            <title>WEB1 - ${title}</title>
            <meta charset="utf-8">
        </head>
        <body>
            <h1><a href="/">WEB</a></h1>
            ${list}
            ${body}
        </body>
    </html>
    `;
}
function templateList(filelist) {
    var list = '<ul>'; 
    var i = 0;
    while(i < filelist.length) {
        list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
        i = i + 1;
    }
    list = list + '</ul>';
    return list;
}


var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if (pathname === '/') {
        if (queryData.id === undefined) { // 홈일 때  
            fs.readdir('./data', function(error, filelist) {
                var title = 'Welcome';
                var description = 'Hello, Node.js';
                var list = templateList(filelist);
                var template = templateHTML(title, list, `<h2>${title}</h2>${description}`);
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`);
                    response.writeHead(200);
                    response.end(template); 
                });
            });
        }
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

<br>

# 35강 27. 수업의 정상

- 이제 본격적으로 시작 !!!!

<br>

# 36강 28.1. Nodejs에서 동기와 비동기 1

- synchronous & asynchronous

![image](https://user-images.githubusercontent.com/43688074/114288369-28873700-9aaa-11eb-8c41-90e1f797cd05.png)

- Nodejs는 비동기적 처리를 하기 위한 좋은 기능을 가지고 있다.

<br>