---
layout: post
title: jQuery <strong>Ajax & JSON</strong>
subtitle: 비동기식 처리 모델과 Ajax
categories: jquery
section: jquery
description: 브라우저에서 웹페이지를 요청하거나 링크를 클릭하면 화면 전환이 발생한다. 이것은 브라우저와 서버와의 통신에 의한 것이다. 서버는 요청받은 페이지(HTML)를 반환하는데 이때 HTML에서 로드하는 CSS나 JavaScript 파일들도 같이 반환된다. 클라이언트의 요청에 따라 서버는 정적인 파일을 반환할 수도 있고 서버 사이드 프로그램이 만들어낸 파일이나 데이터를 반환할 수도 있다. 서버로부터 웹페이지가 반환되면 클라이언트(브라우저)는 이를 렌더링하여 화면에 표시한다. Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 이용해서 비동기적(Asynchronous)으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미한다. 서버로부터 웹페이지가 반환되면 화면 전체를 갱신해야 하는데 페이지 일부만을 갱신하고도 동일한 효과를 볼 수 있도록 하는 것이 Ajax이다. 페이지 전체를 로드하여 렌더링할 필요가 없고 갱신이 필요한 일부만 로드하여 갱신하면 되므로 빠른 퍼포먼스와 부드러운 화면 표시 효과를 기대할 수 있다.
---

* TOC
{:toc}

# 1. Introduction

브라우저에서 웹페이지를 요청하거나 링크를 클릭하면 화면 전환이 발생한다. 이것은 브라우저와 서버와의 통신에 의한 것이다.

![Request & Response](/img/req_res.png)

서버는 요청받은 페이지(HTML)를 반환하는데 이때 HTML에서 로드하는 CSS나 JavaScript 파일들도 같이 반환된다. 클라이언트의 요청에 따라 서버는 정적인 파일을 반환할 수도 있고 서버 사이드 프로그램이 만들어낸 파일이나 데이터를 반환할 수도 있다. 서버로부터 웹페이지가 반환되면 클라이언트(브라우저)는 이를 렌더링하여 화면에 표시한다.

![traditional-webpage-lifecycle](./img/traditional-webpage-lifecycle.png)
{: .w-400}

Traditional Web Page Lifecycle
{: .desc-img}

Ajax(Asynchronous JavaScript and XML)는 자바스크립트를 이용해서 <strong>비동기적(Asynchronous)</strong>으로 서버와 브라우저가 데이터를 교환할 수 있는 통신 방식을 의미한다.

서버로부터 웹페이지가 반환되면 화면 전체를 갱신해야 하는데 페이지 일부만을 갱신하고도 동일한 효과를 볼 수 있도록 하는 것이 Ajax이다. 페이지 전체를 로드하여 렌더링할 필요가 없고 갱신이 필요한 일부만 로드하여 갱신하면 되므로 빠른 퍼포먼스와 부드러운 화면 표시 효과를 기대할 수 있다.

![ajax-webpage-lifecycle](./img/ajax-webpage-lifecycle.png)
{: .w-400}

Ajax Lifecycle
{: .desc-img}

서버는 HTML, XML, JSON등을 반환하는데 Ajax을 위한 데이터 형식은 JSON(JavaScript Object Notation)을 사용하는 것이 일반적이다.

# 2. 동기식 처리 모델 vs 비동기식 처리 모델

<img src='/img/block_nonblock.png'>

동기식 처리 모델과 비동기식 처리 모델
{: .desc-img}

동기식 처리 모델(Synchronous processing model)은 직렬적으로 작업을 수행한다. 즉, 작업은 순차적으로 실행되며 어떤 작업이 수행 중이면 다음 작업은 대기하게 된다. 예를 들어 서버에서 데이터를 가져와 화면에 표시하는 작업을 수행할 경우 서버에 데이터를 요청하고 데이터가 전달될 때까지 이후 작업들은 중단(Blocking)된다.

![synchronous](./img/synchronous.png)

동기식 처리 모델(Synchronous processing model)
{: .desc-img}

아래는 동기식으로 동작하는 코드이다. 순차적으로 실행된다.

```javascript
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  console.log('func2');
  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

비동기식 처리 모델(Asynchronous processing model or Non-Blocking processing model)은 병렬적으로 작업을 수행한다. 즉, 작업이 종료되지 않은 상태라도 대기하지 않고 다음 작업을 실행한다는 의미이다. 예를 들어 서버에서 데이터를 가져와 화면에 표시하는 작업을 수행할 경우 서버에 데이터를 요청한 이후 서버로부터 데이터가 전달될 때까지 대기하지 않고(Non-Blocking) 즉시 다음 작업을 수행한다. 이후 서버로부터 데이터가 전달되면 이벤트가 발생되고 이벤트 핸들러가 데이터를 가지고 수행할 작업을 계속하여 수행한다.

자바스크립트의 대부분의 DOM 이벤트와 Timer 함수(setTimeout, setInterval), Ajax  요청은 비동기적으로 동작한다.

![asynchronous](./img/asynchronous.png)

비동기식 처리 모델(Asynchronous processing model)
{: .desc-img}

아래는 비동기식으로 동작하는 코드이다. 순차적으로 실행되지 않는다.

```javascript
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function() {
    console.log('func2');
  }, 0);

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

# 3. Ajax 요청 및 응답 처리

브라우저는 <strong>XMLHttpRequest 객체</strong>를 이용하여 Ajax 요청을 생성한다. 서버가 브라우저의 요청에 대해 응답을 반환하면 같은 XMLHttpRequest 객체가 그 결과를 처리한다.

다음은 요청 처리의 예이다.

```javascript
// XMLHttpRequest 객체의 생성
var req = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
req.open('GET', 'data/test.json', true);
// Request를 전송한다
req.send();
```

[XMLHttpRequest](https://developer.mozilla.org/ko/docs/XMLHttpRequest) 객체의 인스턴스를 생성하고 XMLHttpRequest.open() 메소드를 사용하여 서버에의 요청을 준비한다. XMLHttpRequest.send() 메소드로 준비된 요청을 서버에 전달한다.

다음은 응답 처리의 예이다.

```javascript
// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출한다.
req.onreadystatechange = function (e) {
  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (req.readyState === XMLHttpRequest.DONE) {
    // status는 response 상태 코드를 반환 : 200 => 정상 응답
    if(req.status == 200) {
      console.log(req.responseText);
    } else {
      console.log("Error!");
    }
  }
};
```

위 코드를 좀 더 자세히 살펴보도록 하자.

XMLHttpRequest.send() 메소드를 통해 서버에 Request를 전송하면 서버는 Response를 반환하여 주는데 언제 Response가 클라이언트에 도달할 지는 알 수 없다. [XMLHttpRequest.onreadystatechange](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange)는 Response가 클라이언트에 도달하여 발생된 이벤트를 감지하고 콜백함수를 실행하여 준다. 이때 이벤트는 Request에 어떠한 변화가 발생한 경우 즉 [XMLHttpRequest.readyState](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/readyState) 프로퍼티가 변경된 경우 발생한다.


따라서 [XMLHttpRequest.onreadystatechange](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange)는 [XMLHttpRequest.readyState](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/readyState) 프로퍼티가 변경될 때마다 호출되는 EventHandler이다.

```javascript
// XMLHttpRequest 객체의 생성
var req = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
req.open('GET', 'data/test.json', true);
// Request를 전송한다
req.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출한다.
req.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출된다.
};
```

XMLHttpRequest 객체는 response가 클라이언트에 도달했는지를 추적할 수 있는 프로퍼티를 제공한다. 이 프로퍼티가 바로 XMLHttpRequest.readyState이다. 만일 XMLHttpRequest.readyState의 값이 4인 경우, 정상적으로 Response가 돌아온 경우이다.

readXMLHttpRequest.readyState의 값은 아래와 같다.

| Value | State   | Description
|:-----:|:--------|:----------------------------
| 0     | UNSENT  | XMLHttpRequest.open() 메소드 호출 이전
| 1     | OPENED  | XMLHttpRequest.open() 메소드 호출 완료
| 2     | HEADERS_RECEIVED | XMLHttpRequest.send() 메소드 호출 완료
| 3     | LOADING | 서버 응답 중(XMLHttpRequest.responseText 미완성 상태)
| 4     | DONE    | 서버 응답 완료

```javascript
// XMLHttpRequest 객체의 생성
var req = new XMLHttpRequest();
// 비동기 방식으로 Request를 오픈한다
req.open('GET', 'data/test.json', true);
// Request를 전송한다
req.send();

// XMLHttpRequest.readyState 프로퍼티가 변경(이벤트 발생)될 때마다 콜백함수(이벤트 핸들러)를 호출한다.
req.onreadystatechange = function (e) {
  // 이 함수는 Response가 클라이언트에 도달하면 호출된다.

  // readyStates는 XMLHttpRequest의 상태(state)를 반환
  // readyState: 4 => DONE(서버 응답 완료)
  if (req.readyState === XMLHttpRequest.DONE) {
    // status는 response 상태 코드를 반환 : 200 => 정상 응답
    if(req.status == 200) {
      console.log(req.responseText);
    } else {
      console.log("Error!");
    }
  }
};
```

XMLHttpRequest의.readyState가 4인 경우, 서버 응답이 완료된 상태이므로 이후 [XMLHttpRequest.status](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest/status)가 200(정상 응답)임을 확인하고 정상인 경우, [XMLHttpRequest.responseText](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseText)를 취득한다. XMLHttpRequest.responseText에는 서버가 전송한 데이터가 담겨 있다.

# 4. JSON

Ajax 요청에 대한 서버의 응답은 주로 HTML, XML, JSON이 사용된다. 이 중 가장 일반적인 데이터 형식은 JSON(JavaScript Object Notation)이다.

```json
{
  "name": "Lee",
  "gender": "male",
  "age": 20,
  "alive": true
}
```

자바스크립트의 객체 리터럴과 매우 흡사하다. 하지만 JSON은 순수한 텍스트로 구성된 데이터이다.

**키는 반드시 큰따옴표(작은따옴표 사용불가)로 둘러싸야 한다.**

JSON.stringify() 메소드는 객체를 JSON 형식의 문자열로 변환한다.

```javascript
var o = {
  name: "Lee",
  gender: "male"
};

// 객체 => JSON 형식의 문자열
var strObject = JSON.stringify(o);
console.log(typeof strObject, strObject); // string '{"name":"Lee","gender":"male"}'

var arr = [1, 5, "false"];

// 배열 객체 => 문자열
var strArray = JSON.stringify(arr);
console.log(typeof strArray, strArray); // string '[1, 5, "false"]'
```

JSON.parse() 메소드는 JSON 데이터를 가진 문자열을 객체로 변환한다.

서버로부터 브라우저로 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체화하여야 하는데 이를 역직렬화(Deserializing)이라 한다. 역직렬화를 위해서 내장 객체 JSON의 static 메소드인 JSON.parse()를 사용한다.
{: .info}

```javascript
// JSON 형식의 문자열 => 객체
var obj = JSON.parse(strObject);
console.log(typeof obj, obj); // object { name: 'Lee', gender: 'male' }

// 문자열 => 배열 객체
var objArray = JSON.parse(strArray);
console.log(typeof objArray, objArray); // object [1, 5, "false"]
```

# 5. Web Server

웹서버(Web Server)는 브라우저와 같은 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램이다.

![client & server](/img/cs.png)
{: .w-300}

Ajax는 웹서버와의 통신이 필요하므로 예제를 실행하기 위해서는 웹서버가 필요하다. 아래의 방법 중에서 한가지를 선택하여 웹서버를 설치하도록 하자.

## 5.1 Simple Webserver with Express

[Node.js](./nodejs-basics#2-install)가 설치되어 있다면 [Express](./express-basics)로 간단한 웹서버를 생성한다.

```bash
## 데스크탑에 webserver-express 폴더가 생성된다.
$ cd ~/Desktop
$ git clone https://github.com/ungmo2/webserver-express.git
$ cd webserver-express
## install express
$ npm i
```

webserver-express디렉터리 내에 있는 <strong>public</strong>가 루트 디렉터리이다. 이곳에 아래와 같은 HTML 파일을 생성하고 이름을 exam.html이라 하자.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Example document</title>
  </head>
  <body>
    <h1>Hello world!</h1>
  </body>
</html>
```

웹서버를 실행한다.

```bash
## start webserver
$ npm start
```

[http://localhost:3000/exam.html](http://localhost:3000/exam.html)에 접속하여 Hello world!가 표시되면 웹서버가 정상 동작하는 것이다.

## 5.2 MAMP

다양한 웹서버가 있으나 가장 대중적으로 사용되는 아파치 서버를 설치한다. 편의상 Apache, MySQL, PHP를 한번의 설치로 간편히 사용할 수 있는 MAMP를 사용하도록 한다.

* [MAMP download](https://www.mamp.info/en/downloads/)

위 링크에서 자신의 OS에 맞는 MAMP를 다운로드하여 설치한다. MAMP PRO는 유료버전이니 MAMP를 설치하도록 한다.

[http://localhost:8888/](http://localhost:8888/)에 접속하여 본다. 문제가 없으면 설치가 성공한 것이다.

MAMP가 설치된 디렉터리 내에 있는 <strong>htdocs</strong>가 루트 디렉터리이다. 이곳에 아래와 같은 HTML 파일을 생성하고 이름을 exam.html이라 하자.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Example document</title>
  </head>
  <body>
    <h1>Hello world!</h1>
  </body>
</html>
```

[http://localhost:8888/exam.html](http://localhost:8888/exam.html)에 접속하여 Hello world!가 표시되면 웹서버가 정상 동작하는 것이다.

## 5.3 Mongoose Web Server

[Mongoose Web Server](https://cesanta.com/)는 간단히 사용할 수 있는 Mongoose Web Server이다.

위 링크에서 자신의 OS에 맞는 Free Edition을 다운로드하여 설치한다.

원하는 위치에 루트 디렉터리로 사용할 폴더를 생성하고 메뉴의 'Edit Configuration'을 클릭하여 document\_root에 패스를 기입한다. 포트를 변경하기 위해서는 'listening\_port'에 포트를 기입한다.

루트 디렉터리에 있는 아래와 같은 HTML 파일을 생성하고 이름을 exam.html이라 하자.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>Example document</title>
  </head>
  <body>
    <h1>Hello world!</h1>
  </body>
</html>
```

[http://localhost:8080/exam.html](http://localhost:8080/exam.html)에 접속하여 Hello world!가 표시되면 웹서버가 정상 동작하는 것이다.

# 6. Ajax

## 6.1 Load HTML

Ajax를 이용하여 웹페이지에 추가하기 가장 손쉬운 데이터 형식은 HTML이다. 별도의 작업없이 전송받은 데이터를 DOM에 추가하면 된다.

<!-- [css link](/files/ajax.css) -->

```html
<!-- 루트 폴더(htdocs)/loadhtml.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id="content"></div>

    <script>
      // XMLHttpRequest 객체의 생성
      var req = new XMLHttpRequest();
      // 비동기 방식으로 Request를 오픈한다
      req.open('GET', 'data/data.html', true);
      // Request를 전송한다
      req.send();

      // Event Handler
      req.onreadystatechange = function () {
        // 서버 응답 완료 && 정상 응답
        if (req.readyState === XMLHttpRequest.DONE) {
          if (req.status == 200) {
            console.log(req.responseText);

            document.getElementById('content').innerHTML = req.responseText;

            // document.getElementById('content').insertAdjacentHTML('beforeend', req.responseText);
          } else {
            console.log('[' + req.status + ']: ' + req.statusText);
          }
        }
      };
    </script>
  </body>
</html>
```

```html
<!-- 루트 폴더(htdocs)/data/data.html -->
<div id="tours">
  <h1>Guided Tours</h1>
  <ul>
    <li class="usa tour">
      <h2>New York, USA</h2>
      <span class="details">$1,899 for 7 nights</span>
      <button class="book">Book Now</button>
    </li>
    <li class="europe tour">
      <h2>Paris, France</h2>
      <span class="details">$2,299 for 7 nights</span>
      <button class="book">Book Now</button>
    </li>
    <li class="asia tour">
      <h2>Tokyo, Japan</h2>
      <span class="details">$3,799 for 7 nights</span>
      <button class="book">Book Now</button>
    </li>
  </ul>
</div>
```

[http://localhost:8080/loadhtml.html](http://localhost:8080/loadhtml.html)

## 6.2 Load JSON

서버로부터 브라우저로 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체화하여야 하는데 이를 역직렬화(Deserializing)이라 한다. 역직렬화를 위해서 내장 객체 JSON의 static 메소드인 JSON.parse()를 사용한다.

<!-- [css link](/files/ajax.css) -->

```html
<!-- 루트 폴더(htdocs)/loadjson.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id="content"></div>

    <script>
      // XMLHttpRequest 객체의 생성
      var req = new XMLHttpRequest();

      // 비동기 방식으로 Request를 오픈한다
      req.open('GET', 'data/data.json', true);
      // Request를 전송한다
      req.send();

      req.onreadystatechange = function () {
        // 서버 응답 완료 && 정상 응답
        if (req.readyState === XMLHttpRequest.DONE) {
          if (req.status == 200) {
            console.log(req.responseText);

            // Deserializing (String → Object)
            responseObject = JSON.parse(req.responseText);

            // JSON → HTML String
            var newContent = '';
            newContent += '<div id="tours">';
            newContent += '<h1>Guided Tours</h1>';
            newContent += '<ul>';
            for (var i = 0; i < responseObject.tours.length; i++) {
              newContent += '<li class="' + responseObject.tours[i].region + ' tour">';
              newContent += '<h2>' + responseObject.tours[i].location + '</h2>';
              newContent += '<span class="details">' + responseObject.tours[i].details + '</span>';
              newContent += '<button class="book">Book Now</button>';
              newContent += '</li>';
            }
            newContent += '</ul></div>';

            document.getElementById('content').innerHTML = newContent;

            // document.getElementById('content').insertAdjacentHTML('beforeend', newContent);

          } else {
            console.log('[' + req.status + ']: ' + req.statusText);
          }
        }
      };
    </script>
  </body>
</html>
```

경로: 루트 폴더(htdocs)/data/data.json

```json
{
  "tours": [
    {
      "region": "usa",
      "location": "New York, USA",
      "details": "$1,899 for 7 nights"
    },
    {
      "region": "europe",
      "location": "Paris, France",
      "details": "$2,299 for 7 nights"
    },
    {
      "region": "asia",
      "location": "Tokyo, Japan",
      "details": "$3,799 for 7 nights"
    }
  ]
}
```

[http://localhost:8080/loadjson.html](http://localhost:8080/loadjson.html)

## 6.3 Load JSONP

요청에 의해 웹페이지가 전달된 서버와 동일한 도메인의 서버로 부터 전달된 데이터는 문제없이 처리할 수 있다. 하지만 보안상의 이유로 다른 도메인(http와 https, 포트가 다르면 다른 도메인으로 간주한다)으로의 요청(크로스 도메인 요청)은 제한된다. 이것을 동일출처원칙(Same-origin policy)이라고 한다.

![cross domain request](/img/cdr.jpg)

동일출처원칙(Same-origin policy)
{: .desc-img}

동일출처원칙을 우회하는 방법은 세가지가 있다.

**1. 웹서버의 프록시 파일**
  서버에 원격 서버로부터 데이터를 수집하는 별도의 기능을 추가하는 것이다. 이를 프록시(Proxy)라 한다.

**2. JSONP**
  script 태그의 원본 주소에 대한 제약이 존재하지 않는데 이것을 이용하여 다른 도메인의 서버에서 데이터를 수집하는 방법이다. 자신의 서버에 함수를 정의하고 다른 도메인의 서버에 얻고자 하는 데이터를 인수로 하는 함수 호출문을 로드하는 것이다.

![comparison_between_ajax_and_jsonp](/img/comparison_between_ajax_and_jsonp.png)

```html
<!-- 루트 폴더(htdocs)/loadjsonp.html -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id='content'></div>

    <script>
    function showTours(data) {
      console.log(data); // data: object

      // JSON → HTML String
      var newContent = '';
      newContent += '<div id="tours">';
      newContent += '<h1>Guided Tours</h1>';
      newContent += '<ul>';
      for (var i = 0; i < data.tours.length; i++) {
        newContent += '<li class="' + data.tours[i].region + ' tour">';
        newContent += '<h2>' + data.tours[i].location + '</h2>';
        newContent += '<span class="details">' + data.tours[i].details + '</span>';
        newContent += '<button class="book">Book Now</button>';
        newContent += '</li>';
      }
      newContent += '</ul></div>';

      document.getElementById('content').innerHTML = newContent;
    }
    </script>
    <script src='http://poiemaweb.com/assets/data/data-jsonp.js'></script>
    <!-- <script>
      showTours({
        "tours": [
          {
            "region": "usa",
            "location": "New York, USA",
            "details": "$1,899 for 7 nights"
          },
          {
            "region": "europe",
            "location": "Paris, France",
            "details": "$2,299 for 7 nights"
          },
          {
            "region": "asia",
            "location": "Tokyo, Japan",
            "details": "$3,799 for 7 nights"
          }
        ]
      });
    </script> -->
  </body>
</html>
```

```javascript
// http://poiemaweb.com/assets/data/data-jsonp.js
showTours({
  "tours": [
    {
      "region": "usa",
      "location": "New York, USA",
      "details": "$1,899 for 7 nights"
    },
    {
      "region": "europe",
      "location": "Paris, France",
      "details": "$2,299 for 7 nights"
    },
    {
      "region": "asia",
      "location": "Tokyo, Japan",
      "details": "$3,799 for 7 nights"
    }
  ]
});
```

[http://localhost:8080/loadjsonp.html](http://localhost:8080/loadjsonp.html)

**3. Cross-Origin Resource Sharing**
  HTTP 헤더에 추가적으로 정보를 추가하여 브라우저와 서버가 서로 통신해야 한다는 사실을 알게하는 방법이다. W3C 명세에 포함되어 있지만 최신 브라우저에서만 동작하며 서버에 HTTP 헤더를 설정해 주어야 한다.

# 7. Ajax with jQuery

jQuery는 Ajax 요청과 응답을 위해 유용한 메소드들을 제공한다.

## 7.1 Low-Level Interface

```javascript
jQuery.ajax( url [, settings ] )  // Returns: jqXHR
jQuery.ajax( [settings ] )        // Returns: jqXHR
```

settings는 Ajax 요청 설정 정보로서 key/value의 쌍으로 이루어진 객체이다. 모든 settings는 옵션이다.

대표적인 settings는 아래와 같다.

| settings	  | Description                    | 비고
| :---------- |:------------------------------ |:-----------------
| url         | 요청이 전송될 url                 |
| method      | http 요청 방식 (default: 'GET')  | version added: 1.9.0
| type        | method의 alias (default: 'GET') | 1.9.0 이전 버전에서 사용
| data        | 서버로 전달될 데이터
| dataType    | 서버로부터 반환될 데이터의 type. default: Intelligent Guess (xml, json, jsonp, script, html)
| async       | 요청 시 동기화 여부. 기본은 비동기(asynchronous) 요청 (default: true)
| timeout     | 요청 제한 시간. 제한 시간 안에 요청이 완료되지 않으면 요청을 취소하거나 error 콜백을 호출.
| jsonpCallback | JSONP 요청을 위한 콜백 함수 이름
| success     | 요청 성공 이벤트 핸들러
| error       | 요청 실패 이벤트 핸들러
| complete    | 요청 완료 이벤트 핸들러



### 7.1.1 Load HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id='content'></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script>
    $.ajax({
      url: "data/data.html",
      cache: false
    })
      .done(function(data, textStatus, jqXHR) {
        $("#content").html(data);
      })
      .fail(function(jqXHR, textStatus, errorThrown){
        console.log("fail: ", jqXHR);
      })
      .always(function(data, textStatus, jqXHR){
        console.log("always: ", data);
      });
    </script>
  </body>
</html>
```

### 7.1.2 Load JSON

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id='content'></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>

    <script>
    $.ajax({
      url: "data/data.json",
      dataType: "json"
    })
      .done(function(data) {
        var newContent = '';
        newContent += '<div id="tours">';
        newContent += '<h1>Guided Tours</h1>';
        newContent += '<ul>';
        for (var i = 0; i < data.tours.length; i++) {
          newContent += '<li class="' + data.tours[i].region + ' tour">';
          newContent += '<h2>' + data.tours[i].location + '</h2>';
          newContent += '<span class="details">' + data.tours[i].details + '</span>';
          newContent += '<button class="book">Book Now</button>';
          newContent += '</li>';
        }
        newContent += '</ul></div>';

        $("#content").html(newContent);
      })
      .fail(function(jqXHR, textStatus, errorThrown){
        console.log("fail: ", jqXHR);
      })
      .always(function(data, textStatus, jqXHR){
        console.log("always: ", data);
      });
    </script>
  </body>
</html>
```

### 7.1.3 Load JSONP

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id="content"></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script>
    $.ajax({
      url: "http://poiemaweb.com/assets/data/data-jsonp.js",
      dataType: "jsonp",
      jsonpCallback: "showTours"
    })
      .done(function(data, textStatus, jqXHR){
        console.log("done: ", data);

        var newContent = '';
        newContent += '<div id="tours">';
        newContent += '<h1>Guided Tours</h1>';
        newContent += '<ul>';
        for (var i = 0; i < data.tours.length; i++) {
          newContent += '<li class="' + data.tours[i].region + ' tour">';
          newContent += '<h2>' + data.tours[i].location + '</h2>';
          newContent += '<span class="details">' + data.tours[i].details + '</span>';
          newContent += '<button class="book">Book Now</button>';
          newContent += '</li>';
        }
        newContent += '</ul></div>';

        $("#content").html(newContent);
      })
      .fail(function(jqXHR, textStatus, errorThrown){
        console.log("fail: ", jqXHR);
      })
      .always(function(data, textStatus, jqXHR){
        console.log("always: ", data);
      });
    </script>
  </body>
</html>
```

## 7.2 Shorthand Method

### 7.2.1 jQuery.get()

HTTP GET request를 사용하여 서버로부터 데이터를 로드한다.

```javascript
jQuery.get( url [, data ] [, success ] [, dataType ] ) // Returns: jqXHR
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id='content'></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script>
      $.get("data/data.html", function(data){
        $("#content").html(data);
      });
    </script>
  </body>
</html>
```

### 7.2.2 jQuery.getJSON()

HTTP GET request를 사용하여 서버로부터 JSON-encoded 데이터를 로드한다.

```javascript
jQuery.getJSON( url [, data ] [, success ] ) // Returns: jqXHR
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id='content'></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script>
      $.getJSON("data/data.json", function(data){

        var newContent = '';
        newContent += '<div id="tours">';
        newContent += '<h1>Guided Tours</h1>';
        newContent += '<ul>';
        for (var i = 0; i < data.tours.length; i++) {

          newContent += '<li class="' + data.tours[i].region + ' tour">';
          newContent += '<h2>' + data.tours[i].location + '</h2>';
          newContent += '<span class="details">' + data.tours[i].details + '</span>';
          newContent += '<button class="book">Book Now</button>';
          newContent += '</li>';
        }
        newContent += '</ul></div>';

        $("#content").html(newContent);
      });
    </script>
  </body>
</html>
```

### 7.2.3 jQuery.getScript()

HTTP GET request를 사용하여 서버로부터 JavaScript 파일을 로드한 후 실행한다.

```javascript
jQuery.getScript( url [, success ] ) // Returns: jqXHR
```

### 7.2.4 jQuery.post()

HTTP GET request를 사용하여 서버로부터 데이터를 로드한다.

```javascript
jQuery.post( url [, data ] [, success ] [, dataType ] ) // Returns: jqXHR
```

### 7.2.5 .load()

서버로부터 HTML 데이터를 로드하고 매치드셋에 적용한다.

```javascript
.load( url [, data ] [, complete ] ) // Returns: jQuery
```

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <link rel="stylesheet" href="http://poiemaweb.com/assets/css/ajax.css">
  </head>
  <body>
    <div id='content'></div>

    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script>
    <script>
      $("#content").load("data/data.html", function(){
        console.log("Load was performed.");
      })
    </script>
  </body>
</html>
```

# Reference

* [jQuery API: Ajax](http://api.jquery.com/category/ajax/)
