---
title: "생활코딩 WEB2 - Node.js 강의노트 - 30강 까지"
excerpt: "21/03/29 - 강의 노트"
tags: ["youtube-lecture", "nodejs"]
categories: ["youtube-lecture"]
last_modified_at: "2021-03-29"
toc: true
toc_sticky: true
---
# 1강 1. 수업소개

## HTML의 문제

- HTML을 직접 타이핑해서 웹 페이지를 수동으로 만드는 거에 지침
- HTML의 구조를 바꾸려면 수많은 웹페이지의 HTML 코드를 수동으로 수정해야 했음
- 웹사이트의 소유자만이 컨텐츠를 추가할 수 있었음
- __성장의 한계!!__

## Nodejs의 발현

- JS로 Server Side Application(웹페이지를 자동으로 생성)을 만들자!!
- 구글이 크롬 웹브라우저에서 동작하는 자바스크립트의 성능을 개선하기 위해서 V8엔진을 개발하고 오픈소스로 공개
- Nodejs의 창시자는 V8엔진을 기반으로 하는 Nodejs를 만듬
- 태초의 자바스크립트가 웹브라우저를 제어하는 것이라면, Nodejs는 자바스크립트를 이용하여 웹브라우저가 아닌 컴퓨터 자체를 제어함!!
- 마치 Python, Java, PHP와 같이...

<br>

# 2강 2. 수업의 목적

- 기존: HTML파일을 직접 만들어야 되서 사용자의 참여가 제한되었다.
- 목적: 사용자에게 컨텐츠의 읽기 뿐만이 아니라 CRUD를 수행하게 함을 목적

<br>

# 3강 3. 설치

- 기존 HTML 계층구조 
![image](https://user-images.githubusercontent.com/43688074/112773062-6da17700-906f-11eb-9ee8-f67c51ec1042.png)

- Nodejs 계층구조 
![image](https://user-images.githubusercontent.com/43688074/112773066-7134fe00-906f-11eb-8634-1b334314ad44.png)

<br>

# 4강 3.1. 설치 (Windows)

[https://nodejs.org/download/release/v8.11.2/](https://nodejs.org/download/release/v8.11.2/)

- 설치 과정

![image](https://user-images.githubusercontent.com/43688074/112773349-bb6aaf00-9070-11eb-9851-477529fea8b7.png)

![image](https://user-images.githubusercontent.com/43688074/112773353-be659f80-9070-11eb-9130-3f68ca9365d8.png)

![image](https://user-images.githubusercontent.com/43688074/112773355-c1609000-9070-11eb-8348-f1915bbb1ff9.png)

![image](https://user-images.githubusercontent.com/43688074/112773359-c32a5380-9070-11eb-9ec7-0ea1d02216ed.png)

![image](https://user-images.githubusercontent.com/43688074/112773363-c58cad80-9070-11eb-9436-763eda53f2c7.png)

![image](https://user-images.githubusercontent.com/43688074/112773366-c7ef0780-9070-11eb-8b70-e6e619d269c3.png)

```
Microsoft Windows [Version 10.0.19041.867]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\Users\spec0>node -v
v8.11.2

C:\Users\spec0>node
> console.log(1+1);
2
undefined
>
(To exit, press ^C again or type .exit)
>

C:\Users\spec0>
```

![image](https://user-images.githubusercontent.com/43688074/112773720-03d69c80-9072-11eb-89ee-d86ee0cefd1c.png)

<br>

# 5강 3.2. 설치 (MacOS)

- 생략

<br>

# 6강 3.3. 설치 (Linux & Codeanywhere)

- 생략

<br>

# 7강 4. 공부방법

- JavaScript
    > 문법을 적당히 공부

- Node.js runtime
    > Nodejs의 기능들을 학습

- Node.js Application
    > Nodejs 기능들을 이용해서 애플리케이션을 제작

- 이걸 반복

<br>

# 8강 5. Node.js로 웹서버 만들기

[https://github.com/jh2song/WEB2-Nodejs/tree/main/02](https://github.com/jh2song/WEB2-Nodejs/tree/main/02)

```javascript
var http = require('http');
var fs = require('fs');
var app = http.createServer(function(request, response) {
    var url = request.url;
    if (request.url == '/') {
        url = '/index.html';
    }
    if (request.url == '/favicon.ico') {
        return response.writeHead(404);
    }
    response.writeHead(200);
    response.end(fs.readFileSync(__dirname + url));
});
app.listen(3000);
```

<br>

# 9강 6.1. JavaScript 문법 - Number Data type

```javascript
console.log(1+1);
console.log(4-1);
console.log(2*2);
console.log(10/2);
```

<br>

# 10강 6.2. JavaScript 문법 - String

```javascript
console.log('1'+'1');
console.log('Hello World!!!'.length);
```

<br>

# 11강 7.1. JavaScript 문법 - 변수의 형식

```javascript
a = 1;
console.log(a); // 1
a = 2;
console.log(a); // 2
// 1 = 2;  // error!

var c = 1; // 변수의 자료형을 가급적이면 붙이자!
```

<br>

# 12강 7.2. JavaScript 문법 - 변수의 활용

```javascript
var name = 'Jihoon';
var letter = 'Dear ' + name + ' fsdkajahsflsjlhsajd ' + name + ' sjasjdhla s hdjahfsadhjfds' + name + ' sjdhjlasdhfa dh dsajh ' + name + ' dsjslad lasd lsdafhj';
console.log(letter);
```

<br>

# 13강 8. JavaScript 문법 - Template Literal

```javascript
var name = 'k8805';
// var letter = 'Dear ' + name + ' \n\nfsdkajahsflsjlhsajd ' + name + ' sjasjdhla s hdjahfsadhjfds' + name + ' sjdhjlasdhfa dh dsajh ' + name + ' dsjslad lasd lsdafhj';

var letter = `Dear ${name} 

fsdkajahsflsjlhsajd ${name} sjasjdhla s ${1+1} hdjahfsadhjfds ${name} sjdhjlasdhfa dh dsajh ${name} dsjslad lasd lsdafhj`;

console.log(letter);
/*
출력: 
Dear k8805 

fsdkajahsflsjlhsajd k8805 sjasjdhla s hdjahfsadhjfds k8805 sjdhjlasdhfa dh dsajh k8805 dsjslad lasd lsdafhj
*/
```

<br>

# 14강 9. URL의 이해

___URL: <u>http</u>://<u>opentutorials.org</u>:<u>3000</u>/<u>main</u>?<u>id=HTML&page=12</u>___

- __http__: protocol (통신규칙 - 사용자가 서버에 접속할때 어떤 방식으로 통신 할 것인가?)
    > http: HyperText Transfer Protocol: 웹브라우저와 웹서버가 서로 데이터를 주고 받기 위해서 만든 통신 규칙

- __opentutorials.org__: host(domain) (인터넷에 접속되어 있는 각각의 컴퓨터들)

- __3000__: port (한 대의 컴퓨터안에 여러대의 서버가 있을 수 있다. 그러면 클라이언트가 접속했을 때 그 중에 어떤 서버와 통신할 지 결정)

- __main__: path (그 컴퓨터 안에 있는 어떤 디렉토리에 어떤 파일인지를 가리킨다)

- __id=HTML&page=12__: query string (query string의 시작은 물음표로 하기로 약속, 값과 값은 &를 쓰기로 약속, key와 value는 = 으로 구분되어 있도록 약속)

<br>

# 15강 10. URL을 통해서 입력된 값 사용하기

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    
    console.log(_url);  // /?id=HTML
    console.log(queryData); // { id: 'CSS' }
    console.log(queryData.id); // HTML

    if (_url == '/') {
        _url = '/index.html';
    }
    if (_url == '/favicon.ico') {
        return response.writeHead(404);
    }
    response.writeHead(200);
    // response.end(fs.readFileSync(__dirname + _url));
    response.end(queryData.id); // 화면에 key인 id에 대한 value가 웹페이지에 표시된다. (예, CSS)
});
app.listen(3000);
```

<br>

# 16강 11. App 제작-동적인 웹페이지 만들기

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var title = queryData.id
    console.log(queryData.id); // HTML
    if (_url == '/') {
        title = 'Welcome';
    }
    if (_url == '/favicon.ico') {
        return response.writeHead(404);
    }
    response.writeHead(200);
    var template = `
    <!doctype html>
    <html>
    <head>
        <title>WEB1 - ${title}</title>
        <meta charset="utf-8">
    </head>
    <body>
        <h1><a href="/">WEB</a></h1>
        <ol>
            <li><a href="/?id=HTML">HTML</a></li>
            <li><a href="/?id=CSS">CSS</a></li>
            <li><a href="/?id=JavaScript">JavaScript</a></li>
        </ol>
        <h2>${title}</h2>
        <p>
            <a href="https://www.w3.org/TR/html5/" target="_blank" title="html5 speicification">Hypertext Markup Language (HTML)
            </a> 
            is the standard markup language for <strong>creating <u>web</u> pages</strong> and web applications.Web browsers receive HTML documents from a web server or from local storage and render them into multimedia web pages. HTML describes the structure of a web page semantically and originally included cues for the appearance of the document.
            <img src="coding.jpg" width="100%">
        </p>
        <p style="margin-top:45px;">HTML elements are the building blocks of HTML pages. With HTML constructs, images and other objects, such as interactive forms, may be embedded into the rendered page. It provides a means to create structured documents by denoting structural semantics for text such as headings, paragraphs, lists, links, quotes and other items. HTML elements are delineated by tags, written using angle brackets.
        </p>
    </body>
    </html>
    `;
    response.end(template); 
});
app.listen(3000);
```

<br>

# 17강 12. Node.js의 파일 읽기 기능

- 모듈: 기본적으로 제공하는 기능들을 그룹핑 해놓은 각각의 그룹들 

- 파일 읽기 실습 

```javascript
var fs = require('fs');
/*
fs.readFile('sample.txt', function(err, data) {
    console.log(data);
});

출력
<Buffer 6c 6f 72 65 6d 20 6c 6f 72 65 6d 20 6c 6f 72 65 6d 20 6c 6f 72 65 6d 20 6c 6f 72 65 6d 
20 6c 6f 72 65 6d 20 6c 6f 72 65 6d 20 6c 6f 72 65 6d 6c 6f 72 ... >
*/

fs.readFile('sample.txt', 'utf8', function(err, data) {
    console.log(data);
});
```

<br>

# 18강 13. App 제작- 파일을 이용해 본문 구현

- main.js가 수정되면 껐다 켜야 되고 파일로 분할해서 파일을 수정하면 껐다 킬 필요 없다.

- [https://github.com/jh2song/WEB2-Nodejs/tree/main/data](https://github.com/jh2song/WEB2-Nodejs/tree/main/data)

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var title = queryData.id
    console.log(queryData.id); // HTML
    if (_url == '/') {
        title = 'Welcome';
    }
    if (_url == '/favicon.ico') {
        return response.writeHead(404);
    }
    response.writeHead(200);
    fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
        var template = `
            <!doctype html>
            <html>
                <head>
                    <title>WEB1 - ${title}</title>
                    <meta charset="utf-8">
                </head>
                <body>
                    <h1><a href="/">WEB</a></h1>
                    <ol>
                        <li><a href="/?id=HTML">HTML</a></li>
                        <li><a href="/?id=CSS">CSS</a></li>
                        <li><a href="/?id=JavaScript">JavaScript</a></li>
                    </ol>
                    <h2>${title}</h2>
                    <p>${description}</p>
                </body>
            </html>
    `;
    response.end(template); 
    });
});
app.listen(3000);
```

<br>

# 19강 14. JavaScript-Boolean

```javascript
console.log(true); // true 출력
console.log(false); // false 출력
```

<br>

# 20강 15. JavaScript-비교연산자

```javascript
console.log(1==1); // true
console.log(1==2); // false
console.log(1>2); // false
console.log(1<2); // true

// 정말 같은가?
console.log(1===1); // true
console.log(1===2); // false
```

<br>

# 21강 16. JavaScript-제어문

효율적인 코드를 위해(ex. 중복 제거) 제어문(조건문, 반복문)을 사용하자.

<br>

# 22강 17. JavaScript-조건문

```javascript
console.log('A');
console.log('B');
if (true) {
    console.log('C1');
} else {
    console.log('C2');
}
console.group('D');
```

<br>

# 23강 18. NodeJS-콘솔에서의 입력값

```javascript
var args = process.argv;
console.log(args);
/*
출력
[ 'C:\\Program Files\\nodejs\\node.exe',
  'C:\\Users\\spec0\\Desktop\\Project\\2021\\03\\nodejs\\syntax\\conditional.js' ]
*/

console.log('A');
console.log('B');
if (args[2] === '1') { // idx 2부터 파라미터가 들어온다
    console.log('C1');
} else {
    console.log('C2');
}
console.group('D');
```

<br>

# 24강 19.1. App 제작-Not found 구현

- url에 루트가 아닌 경로는 Not found를 출력시킴
- HTML, CSS, ... 등등의 파일은 쿼리스트링으로 구현해서 잘 동작함

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    var title = queryData.id
    
    if (pathname === '/') {
        fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
            var template = `
                <!doctype html>
                <html>
                    <head>
                        <title>WEB1 - ${title}</title>
                        <meta charset="utf-8">
                    </head>
                    <body>
                        <h1><a href="/">WEB</a></h1>
                        <ol>
                            <li><a href="/?id=HTML">HTML</a></li>
                            <li><a href="/?id=CSS">CSS</a></li>
                            <li><a href="/?id=JavaScript">JavaScript</a></li>
                        </ol>
                        <h2>${title}</h2>
                        <p>${description}</p>
                    </body>
                </html>
            `;
            response.writeHead(200);
            response.end(template); 
        });
    } else {
        response.writeHead(404);
        response.end('Not found'); 
    }

    console.log(url.parse(_url, true));
    /*
    출력
    Url {
        protocol: null,
        slashes: null,
        auth: null,
        host: null,
        port: null,
        hostname: null,
        hash: null,
        search: '',
        query: {},
        pathname: '/',
        path: '/',
        href: '/' }
    Url {
        protocol: null,
        slashes: null,
        auth: null,
        host: null,
        port: null,
        hostname: null,
        hash: null,
        search: '',
        query: {},
        pathname: '/favicon.ico',
        path: '/favicon.ico',
        href: '/favicon.ico' }
    */

});
app.listen(3000);
```

<br>

# 25강 19.2. App 제작-홈페이지 구현

- 기존 웹은 홈이 구현이 안되어있다. 각각의 페이지로 들어갔을땐 잘 동작한다.

![image](https://user-images.githubusercontent.com/43688074/113950528-89193880-984c-11eb-8d7b-3f5be788fb42.png)

![image](https://user-images.githubusercontent.com/43688074/113950590-b1089c00-984c-11eb-8c1f-a7680a790f94.png)

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if (pathname === '/') {
        if (queryData.id === undefined) { // 홈일 때
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = 'Welcome';
                var description = 'Hello, Node.js';
                var template = `
                    <!doctype html>
                    <html>
                        <head>
                            <title>WEB1 - ${title}</title>
                            <meta charset="utf-8">
                        </head>
                        <body>
                            <h1><a href="/">WEB</a></h1>
                            <ol>
                                <li><a href="/?id=HTML">HTML</a></li>
                                <li><a href="/?id=CSS">CSS</a></li>
                                <li><a href="/?id=JavaScript">JavaScript</a></li>
                            </ol>
                            <h2>${title}</h2>
                            <p>${description}</p>
                        </body>
                    </html>
                `;
                response.writeHead(200);
                response.end(template); 
            });
        } else {
            fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                var title = queryData.id
                var template = `
                    <!doctype html>
                    <html>
                        <head>
                            <title>WEB1 - ${title}</title>
                            <meta charset="utf-8">
                        </head>
                        <body>
                            <h1><a href="/">WEB</a></h1>
                            <ol>
                                <li><a href="/?id=HTML">HTML</a></li>
                                <li><a href="/?id=CSS">CSS</a></li>
                                <li><a href="/?id=JavaScript">JavaScript</a></li>
                            </ol>
                            <h2>${title}</h2>
                            <p>${description}</p>
                        </body>
                    </html>
                `;
                response.writeHead(200);
                response.end(template); 
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

# 26강 20. JavaScript-반복문

```javascript
console.log('A');
console.log('B');

var i = 0;
while(i < 2) {
    console.log('C1');
    console.log('C2');
    i = i + 1;
}

console.log('D');
```

<br>

# 27강 21. JavaScript-배열

```javascript
var arr = ['A', 'B', 'C', 'D'];
console.log(arr[1]); // B
console.log(arr[3]); // D
arr[2] = 3;
console.log(arr); // A, B, 3, D
console.log(arr.length); // 4
arr.push('E');
console.log(arr); // A, B, 3, D, E
```

<br>

# 28강 22. JavaScript-배열과 반복문

```javascript
var number = [1, 400, 12, 34];
//하나씩 출력
//var i = 0;
//while(i < number.length) {
//  console.log(number[i]);
//  i = i + 1;
//}

// 합 구하기
var i = 0;
var total = 0;
while(i < number.length) {
    total = total + number[i];
    i = i + 1;
}
console.log(`total : ${total}`); // total : 447
```

<br>

# 29강 23. Node.js에서 파일목록 알아내기

```javascript
var testFolder = './data'
var fs = require('fs');

fs.readdir(testFolder, function(error, filelist) {
    console.log(filelist);
});
// 출력: [ 'CSS', 'HTML', 'JavaScript', 'main.js' ]
```

<br>

# 30강 24. App 제작-글목록 출력하기

```javascript
var http = require('http');
var fs = require('fs');
var url = require('url');

var app = http.createServer(function(request, response) {
    var _url = request.url;
    var queryData = url.parse(_url, true).query;
    var pathname = url.parse(_url, true).pathname;
    
    if (pathname === '/') {
        if (queryData.id === undefined) { // 홈일 때  
            fs.readdir('./data', function(error, filelist) {
                var title = 'Welcome';
                var description = 'Hello, Node.js';
                var list = '<ul>'; 
                var i = 0;
                while(i < filelist.length) {
                    list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
                    i = i + 1;
                }
                list = list + '</ul>';
                var template = `
                    <!doctype html>
                    <html>
                        <head>
                            <title>WEB1 - ${title}</title>
                            <meta charset="utf-8">
                        </head>
                        <body>
                            <h1><a href="/">WEB</a></h1>
                            ${list}
                            <h2>${title}</h2>
                            <p>${description}</p>
                        </body>
                    </html>
                `;
            response.writeHead(200);
            response.end(template); 
            });
        } else {
            fs.readdir('./data', function(error, filelist) {
                var title = 'Welcome';
                var description = 'Hello, Node.js';
                var list = '<ul>'; 
                var i = 0;
                while(i < filelist.length) {
                    list = list + `<li><a href="/?id=${filelist[i]}">${filelist[i]}</a></li>`;
                    i = i + 1;
                }
                list = list + '</ul>';
                fs.readFile(`data/${queryData.id}`, 'utf8', function(err, description) {
                    var title = queryData.id
                    var template = `
                        <!doctype html>
                        <html>
                            <head>
                                <title>WEB1 - ${title}</title>
                                <meta charset="utf-8">
                            </head>
                            <body>
                                <h1><a href="/">WEB</a></h1>
                                ${list}
                                <h2>${title}</h2>
                                <p>${description}</p>
                            </body>
                        </html>
                    `;
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