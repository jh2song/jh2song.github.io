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

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring'); // 새로 생긴 부분

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
            <form action="http://localhost:3000/create_process" method="post">
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
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            console.log(post); // { title: 'nodejs', description: 'nodejs is ...' } 출력
        });
        response.writeHead(200);
        response.end('success'); 
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

![image](https://user-images.githubusercontent.com/43688074/115941015-94bc5e80-a4de-11eb-8578-158e8223124d.png)

- 로그를 찍으면 나오는 결과

![image](https://user-images.githubusercontent.com/43688074/115941020-97b74f00-a4de-11eb-9e8f-f07ee6cdd590.png)

<br>

# 43강 33. App 제작-파일생성과 리다이렉션

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

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
            <form action="http://localhost:3000/create_process" method="post">
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
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- 결과

![image](https://user-images.githubusercontent.com/43688074/115941370-119c0800-a4e0-11eb-95bd-c5e083a0c552.png)

![image](https://user-images.githubusercontent.com/43688074/115941385-1f518d80-a4e0-11eb-82ec-82246184a0bc.png)

<br>

# 44강 34. App 제작-글수정-수정링크생성

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

function templateHTML(title, list, body, control) {
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
            ${control}
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
                var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                    );
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
            <form action="http://localhost:3000/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(template); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- 홈 디렉토리에는 update 버튼이 없다.

![image](https://user-images.githubusercontent.com/43688074/115941755-c2ef6d80-a4e1-11eb-9b6e-82de17a101a2.png)

- 각 파일로 들어갈 시 update 버튼(링크)이 생긴다.

![image](https://user-images.githubusercontent.com/43688074/115941791-e3b7c300-a4e1-11eb-835d-da7e6d86b5bc.png)

- 버튼 클릭시 링크 확인하기

![image](https://user-images.githubusercontent.com/43688074/115941798-f0d4b200-a4e1-11eb-9551-1f2cb8a8a183.png)

<br>

# 45강 35. App 제작-글수정-수정할 정보 전송

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

function templateHTML(title, list, body, control) {
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
            ${control}
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
                var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                    );
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
            <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(template); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else if (pathname === '/update') {
        fs.readdir('./data', function(error, filelist) {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id;
                var list = templateList(filelist);
                var template = templateHTML(title, list, 
                `
                <form action="/update_process" method="post">
                <input type="hidden" name="id" value="${title}">
                <p><input type="text" name="title" placeholder="title" value="${title}"></p>
                <p>
                    <textarea name="description" placeholder="description">${description}</textarea>
                </p>
                <p>
                    <input type="submit">
                </p>
                </form>
                `,
                `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- update 수행 시 submit 버튼 누르면 id, title, description이 post로 넘어간다. {name: value}로

![image](https://user-images.githubusercontent.com/43688074/115955156-fb1d9d00-a52f-11eb-8eef-64014576c14d.png)

<br>

# 46강 36. App 제작-글수정-파일명 변경, 내용저장

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

function templateHTML(title, list, body, control) {
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
            ${control}
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
                var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                    );
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
            <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(template); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else if (pathname === '/update') {
        fs.readdir('./data', function(error, filelist) {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id;
                var list = templateList(filelist);
                var template = templateHTML(title, list, 
                `
                <form action="/update_process" method="post">
                <input type="hidden" name="id" value="${title}">
                <p><input type="text" name="title" placeholder="title" value="${title}"></p>
                <p>
                    <textarea name="description" placeholder="description">${description}</textarea>
                </p>
                <p>
                    <input type="submit">
                </p>
                </form>
                `,
                `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        });
    } else if (pathname === '/update_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            var title = post.title;
            var description = post.description;
            fs.rename(`data/${id}`, `data/${title}`, function(error) {
                fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                    response.writeHead(302, {Location: `/?id=${title}`});
                    response.end(); 
                });
            });  
        });
    }else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- 결과

![image](https://user-images.githubusercontent.com/43688074/115955501-cad6fe00-a531-11eb-8baa-e62179f4c2ac.png)

![image](https://user-images.githubusercontent.com/43688074/115955503-ce6a8500-a531-11eb-892b-bcf0d88bd97f.png)

<br>

# 47강 37. App 제작-글삭제-삭제버튼 구현

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

function templateHTML(title, list, body, control) {
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
            ${control}
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
                var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a>
                    <a href="/update?id=${title}">update</a>
                    <form action="delete_process" method="post">
                        <input type="hidden" name="id" value="${title}">
                        <input type="submit" value="delete">
                    </form>`
                    );
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
            <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(template); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else if (pathname === '/update') {
        fs.readdir('./data', function(error, filelist) {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id;
                var list = templateList(filelist);
                var template = templateHTML(title, list, 
                `
                <form action="/update_process" method="post">
                <input type="hidden" name="id" value="${title}">
                <p><input type="text" name="title" placeholder="title" value="${title}"></p>
                <p>
                    <textarea name="description" placeholder="description">${description}</textarea>
                </p>
                <p>
                    <input type="submit">
                </p>
                </form>
                `,
                `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        });
    } else if (pathname === '/update_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            var title = post.title;
            var description = post.description;
            fs.rename(`data/${id}`, `data/${title}`, function(error) {
                fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                    response.writeHead(302, {Location: `/?id=${title}`});
                    response.end(); 
                });
            });  
        });
    }else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- 결과

![image](https://user-images.githubusercontent.com/43688074/116161267-6c2aa380-a72e-11eb-82cc-08682bc60859.png)

![image](https://user-images.githubusercontent.com/43688074/116161285-70ef5780-a72e-11eb-84a1-df5a0ed98dfe.png)

<br>

# 48강 38. App 제작-글삭제 기능 완성

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

function templateHTML(title, list, body, control) {
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
            ${control}
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
                var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = templateList(filelist);
                    var template = templateHTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a>
                    <a href="/update?id=${title}">update</a>
                    <form action="delete_process" method="post">
                        <input type="hidden" name="id" value="${title}">
                        <input type="submit" value="delete">
                    </form>`
                    );
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
            <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(template); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else if (pathname === '/update') {
        fs.readdir('./data', function(error, filelist) {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id;
                var list = templateList(filelist);
                var template = templateHTML(title, list, 
                `
                <form action="/update_process" method="post">
                <input type="hidden" name="id" value="${title}">
                <p><input type="text" name="title" placeholder="title" value="${title}"></p>
                <p>
                    <textarea name="description" placeholder="description">${description}</textarea>
                </p>
                <p>
                    <input type="submit">
                </p>
                </form>
                `,
                `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                );
                response.writeHead(200);
                response.end(template); 
            });
        });
    } else if (pathname === '/update_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            var title = post.title;
            var description = post.description;
            fs.rename(`data/${id}`, `data/${title}`, function(error) {
                fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                    response.writeHead(302, {Location: `/?id=${title}`});
                    response.end(); 
                });
            });  
        });
    } else if (pathname === '/delete_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            fs.unlink(`data/${id}`, function(error) {
                response.writeHead(302, {Location: `/`});
                response.end(); 
            });
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

- 결과

    - mongoDB 문서의 delete 버튼을 누른다.

    ![image](https://user-images.githubusercontent.com/43688074/116161947-b7918180-a72f-11eb-9a02-23887c31674e.png)

    - 삭제된다

    ![image](https://user-images.githubusercontent.com/43688074/116161965-c2e4ad00-a72f-11eb-901f-f22d33e03a84.png)

<br>

# 49강 39. JavaScript 객체의 형식

```javascript
// Array
var members = ['egoing', 'k8805', 'hoya'];
console.log(members[1]); // k8805

// Object
var roles = {
    'programmer':'egoing',
    'designer':'k8805',
    'manager':'hoya'
}
console.log(roles.designer); // k8805
```

<br>

# 50강 40. JavaScript-객체-반복

```javascript
// Array
var members = ['egoing', 'k8805', 'hoya'];
console.log(members[1]); // k8805

/*
array loop egoing
array loop k8805
array loop hoya
*/
var i = 0;
while(i < members.length) {
    console.log('array loop', members[i]);
    i = i + 1;
}

// Object
var roles = {
    'programmer':'egoing',
    'designer':'k8805',
    'manager':'hoya'
}
console.log(roles.designer); // k8805
console.log(roles['designer']); // k8805

/*
object =>  programmer value =>  egoing
object =>  designer value =>  k8805
object =>  manager value =>  hoya
*/
for (var name in roles) {
    console.log('object => ', name, 'value => ', roles[name]);
}
```

<br>

# 51강 41. JavaScript-객체-값으로서 함수

- if나 while문 같은 경우는 statement를 값으로 할당할 수 없다.

- 함수는 값으로 할당할 수 있다.

```javascript
var f = function () {
    console.log(1+1);
    console.log(1+2);
}
console.log(f); // [Function: f]

/*
2
3
*/
f(); 

/*
2
3
*/
var a = [f]; // 배열
a[0]();

/*
2
3
*/
var o = { // 객체
    func:f
}
o.func();
```

<br>

# 52강 42. JavaScript-객체-데이터와 처리 방법을 담는 그릇으로서 객체

```javascript
var o = {
    v1:'v1',
    v2:'v2',
    f1:function () {
        console.log(this.v1);
    },
    f2:function () {
        console.log(this.v2);
    }
}

/*
출력
v1
v2
*/
o.f1();
o.f2();
```

<br>

# 53강 43. App제작-템플릿 기능 정리정돈하기

- 객체에 프로퍼티를 사용해 코드 리펙토링

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

var template = {
    HTML:function(title, list, body, control) {
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
                ${control}
                ${body}
            </body>
        </html>
        `;
    },
    list:function(filelist) {
        var list = '<ul>'; 
        var i = 0;
        while(i < filelist.length) {
            list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
            i = i + 1;
        }
        list = list + '</ul>';
        return list;
    }
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
                var list = template.list(filelist);
                var html = template.HTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(html); 

            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = template.list(filelist);
                    var html = template.HTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a>
                    <a href="/update?id=${title}">update</a>
                    <form action="delete_process" method="post">
                        <input type="hidden" name="id" value="${title}">
                        <input type="submit" value="delete">
                    </form>`
                    );
                    response.writeHead(200);
                    response.end(html); 
                });
            });
        }
    } else if (pathname === '/create') {
        fs.readdir('./data', function(error, filelist) {
            var title = 'WEB - create';
            var list = template.list(filelist);
            var html = template.HTML(title, list, `
            <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(html); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else if (pathname === '/update') {
        fs.readdir('./data', function(error, filelist) {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id;
                var list = template.list(filelist);
                var html = template.HTML(title, list, 
                `
                <form action="/update_process" method="post">
                <input type="hidden" name="id" value="${title}">
                <p><input type="text" name="title" placeholder="title" value="${title}"></p>
                <p>
                    <textarea name="description" placeholder="description">${description}</textarea>
                </p>
                <p>
                    <input type="submit">
                </p>
                </form>
                `,
                `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                );
                response.writeHead(200);
                response.end(html); 
            });
        });
    } else if (pathname === '/update_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            var title = post.title;
            var description = post.description;
            fs.rename(`data/${id}`, `data/${title}`, function(error) {
                fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                    response.writeHead(302, {Location: `/?id=${title}`});
                    response.end(); 
                });
            });  
        });
    } else if (pathname === '/delete_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            fs.unlink(`data/${id}`, function(error) {
                response.writeHead(302, {Location: `/`});
                response.end(); 
            });
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

> "처음부터 이상적인 코드를 짤려고 하면 한 줄도 짤 수 없을 수 있다. 또 내가 하는 코딩이 즐겁지 않고 내가 만든 결과물이 부끄러울 수 있다. 그래서는 생산자가 되기 어렵다."

<br>

# 54강 44. Node.js 모듈의형식

- mpart.js

```javascript
// mpart.js

var M = {
    v:'v',
    f:function(){
        console.log(this.v);
    }
}

// M 객체를 바깥에서 이용하게 하겠다.
module.exports = M;
```

- muse.js

```javascript
// muse.js

var part = require('./mpart.js');
console.log(part); // { v: 'v', f: [Function: f] }

part.f(); // v
```

<br>

# 55강 45. App 제작 - 모듈의 활용

- lib/template.js

```javascript
module.exports = {
    HTML:function(title, list, body, control) {
        return `
        <!doctype html>
        <html>
            <head>
                <title>WEB2 - ${title}</title>
                <meta charset="utf-8">
            </head>
            <body>
                <h1><a href="/">WEB</a></h1>
                ${list}
                ${control}
                ${body}
            </body>
        </html>
        `;
    },
    list:function(filelist) {
        var list = '<ul>'; 
        var i = 0;
        while(i < filelist.length) {
            list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
            i = i + 1;
        }
        list = list + '</ul>';
        return list;
    }
}
```

- main.js

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');
var qs = require('querystring');

var template = require('./lib/template.js'); // 강조

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if (pathname === '/') {
        if (queryData.id === undefined) { // 홈일 때  
            fs.readdir('./data', function(error, filelist) {
                var title = 'Welcome';
                var description = 'Hello, Node.js';
                var list = template.list(filelist);
                var html = template.HTML(title, list, `<h2>${title}</h2>${description}`,
                `<a href="/create">create</a>`
                );
                response.writeHead(200);
                response.end(html); 

            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id;
                    var list = template.list(filelist);
                    var html = template.HTML(title, list, `<h2>${title}</h2>${description}`,
                    `<a href="/create">create</a>
                    <a href="/update?id=${title}">update</a>
                    <form action="delete_process" method="post">
                        <input type="hidden" name="id" value="${title}">
                        <input type="submit" value="delete">
                    </form>`
                    );
                    response.writeHead(200);
                    response.end(html); 
                });
            });
        }
    } else if (pathname === '/create') {
        fs.readdir('./data', function(error, filelist) {
            var title = 'WEB - create';
            var list = template.list(filelist);
            var html = template.HTML(title, list, `
            <form action="/create_process" method="post">
            <p><input type="text" name="title" placeholder="title"></p>
            <p>
                <textarea name="description" placeholder="description"></textarea>
            </p>
            <p>
                <input type="submit">
            </p>
            </form>
            `, '');
            response.writeHead(200);
            response.end(html); 
        });
    } else if (pathname === '/create_process') { // post로 값 받기
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var title = post.title;
            var description = post.description;
            fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                response.writeHead(302, {Location: `/?id=${title}`});
                response.end(); 
            });
        });
    } else if (pathname === '/update') {
        fs.readdir('./data', function(error, filelist) {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id;
                var list = template.list(filelist);
                var html = template.HTML(title, list, 
                `
                <form action="/update_process" method="post">
                <input type="hidden" name="id" value="${title}">
                <p><input type="text" name="title" placeholder="title" value="${title}"></p>
                <p>
                    <textarea name="description" placeholder="description">${description}</textarea>
                </p>
                <p>
                    <input type="submit">
                </p>
                </form>
                `,
                `<a href="/create">create</a> <a href="/update?id=${title}">update</a>`
                );
                response.writeHead(200);
                response.end(html); 
            });
        });
    } else if (pathname === '/update_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            var title = post.title;
            var description = post.description;
            fs.rename(`data/${id}`, `data/${title}`, function(error) {
                fs.writeFile(`data/${title}`, description, 'utf8', function(err) {
                    response.writeHead(302, {Location: `/?id=${title}`});
                    response.end(); 
                });
            });  
        });
    } else if (pathname === '/delete_process') { 
        var body = '';
        request.on('data', function(data) {
            body = body + data;
        });
        request.on('end', function() {
            var post = qs.parse(body);
            var id = post.id;
            fs.unlink(`data/${id}`, function(error) {
                response.writeHead(302, {Location: `/`});
                response.end(); 
            });
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }
});
app.listen(3000);
```

<br>

# 56강 46. App 제작-입력정보에 대한 보안



<br>

# 57강 47.1. App제작-출력정보에 대한 보안

<br>

# 58강 47.2. App제작-출력정보에 대한 보안

<br>

# 59강 47.3. App제작-출력정보에 대한 보안

<br>

# 60강 48. API와 CreateServer

<br>

# 61강 49. 수업을마치며

<br>

# 62강 Long take

<br>

# 63강 49. 부록 - pm2 보충학습