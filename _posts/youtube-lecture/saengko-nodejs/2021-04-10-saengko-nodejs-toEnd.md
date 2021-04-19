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

# 37강 28.2. Nodejs에서 동기와 비동기 2

```javascript
var fs = require('fs');

/*
// readFileSync
console.log('A');
var result = fs.readFileSync('syntax/sample.txt', 'utf8'); // 동기, 리턴한다
console.log(result);
console.log('C');

출력:
A
B
C
*/

console.log('A');
fs.readFile('syntax/sample.txt', 'utf8', function(err, result) {
    console.log(result);
}); // 비동기, readFile은 리턴하지 않는다.
console.log('C');

// 출력
// A
// C
// B
```

<br>

# 38강 28.3. JavaScript-callback

```javascript
/*
function a() {
    console.log('A');
}
*/
var a = function() {
    console.log('A');
}

function slowfunc(callback) {
    callback();
}

slowfunc(a);
```

# 39강 29. Node.js의 패키지 매니저와 PM2

- `npm install pm2 -g`

```
PS C:\Users\spec0\Desktop\Project\2021\03\nodejs> npm install pm2 -g      
npm WARN notice [SECURITY] lodash has the following vulnerability: 1 high. Go here for more details: https://www.npmjs.com/advisories?search=lodash&version=4.17.21 - Run `npm i npm@latest -g` to upgrade your npm version, and then `npm audit` to get more info.
C:\Users\spec0\AppData\Roaming\npm\pm2 -> C:\Users\spec0\AppData\Roaming\npm\node_modules\pm2\bin\pm2
C:\Users\spec0\AppData\Roaming\npm\pm2-dev -> C:\Users\spec0\AppData\Roaming\npm\node_modules\pm2\bin\pm2-dev
C:\Users\spec0\AppData\Roaming\npm\pm2-runtime -> C:\Users\spec0\AppData\Roaming\npm\node_modules\pm2\bin\pm2-runtime
C:\Users\spec0\AppData\Roaming\npm\pm2-docker -> C:\Users\spec0\AppData\Roaming\npm\node_modules\pm2\bin\pm2-docker
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@~2.3.1 (node_modules\pm2\node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.3.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","npm WARN ws@7.2.5 requires a peer of bufferutil@^4.0.1 but none is installed. You must install peer dependencies yourself.
npm WARN ws@7.2.5 requires a peer of utf-8-validate@^5.0.2 but none is installed. You must install peer dependencies yourself.

+ pm2@4.5.6
added 175 packages in 10.175s
```

> pm2 설치


- `pm2 start main.js`

```
PS C:\Users\spec0\Desktop\Project\2021\03\nodejs> pm2 start main.js
[PM2] Spawning PM2 daemon with pm2_home=C:\Users\spec0\.pm2
[PM2] PM2 Successfully daemonized
[PM2] Starting C:\Users\spec0\Desktop\Project\2021\03\nodejs\main.js in fork_mode (1 instance)
[PM2] Done.
┌─────┬─────────┬─────────────┬─────────┬─────────┬──────────┬────────┬──────┬───────────┬──────────┬──────────┬──────────┬──────────┐
│ id  │ name    │ namespace   │ version │ mode    │ pid      │ uptime │ ↺    │ status    │ cpu      │ mem      │ user     │ watching │
├─────┼─────────┼─────────────┼─────────┼─────────┼──────────┼────────┼──────┼───────────┼──────────┼──────────┼──────────┼──────────┤
│ 0   │ main    │ default     │ N/A     │ fork    │ 8520     │ 0s     │ 0    │ online    │ 0%       │ 30.7mb   │ spec0    │ disabled │
└─────┴─────────┴─────────────┴─────────┴─────────┴──────────┴────────┴──────┴───────────┴──────────┴──────────┴──────────┴──────────┘
```

- `pm2 monit`

![image](https://user-images.githubusercontent.com/43688074/115125619-574e6180-a004-11eb-9bb2-18696b8a94b3.png)

- `pm2 list`

![image](https://user-images.githubusercontent.com/43688074/115125662-a6949200-a004-11eb-8ea0-f066d8b2e293.png)

- `pm2 stop main`

![image](https://user-images.githubusercontent.com/43688074/115125683-d17ee600-a004-11eb-882c-666c1b2e41f0.png)

- `pm2 start main.js --watch`

수정 후 node 재시작 없이 리로드 하면 바로 반영

- `pm2 log`

로그를 볼 수 있음

<br>

# 40강 30. HTML-form

```html
<form action="http://localhost:3000/process_create" method="post">
    <p><input type="text" name="title"></p>
    <p>
        <textarea name="description"></textarea>
    </p>
    <p>
        <input type="submit">
    </p>
</form>
```

![image](https://user-images.githubusercontent.com/43688074/115126270-bb732480-a008-11eb-815e-c34f8332db77.png)

![image](https://user-images.githubusercontent.com/43688074/115126284-e198c480-a008-11eb-9b7a-1cdb46a85aa9.png)

> 서버에 데이터를 수정, 삭제, 생성 같은걸 할 때는 method를 post로 해야 한다 !!! (보안 상에 이유, 데이터를 get으로 보내면 짤릴 수 있다. 너무 큰 데이터일 때)

<br>

# 41강 31. App 제작-글생성 UI 만들기

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
            <a href="/create">create</a>
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
    } else if (pathname === '/create') {
        fs.readdir('./data', function(error, filelist) {
            var title = 'WEB - create';
            var list = templateList(filelist);
            var template = templateHTML(title, list, `
            <form action="http://localhost:3000/process_create" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `);
            response.writeHead(200);
            response.end(template); 
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- 결과

![image](https://user-images.githubusercontent.com/43688074/115313439-3aee2880-a1ae-11eb-85a4-1eddbfcabb75.png)

![image](https://user-images.githubusercontent.com/43688074/115313445-3de91900-a1ae-11eb-97bc-bbf02ba27b99.png)

<br>

# 42강 32. App 제작-POST 방식으로 전송된 데이터 받기

<br>