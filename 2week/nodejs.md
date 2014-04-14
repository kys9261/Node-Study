Learn Nodejs
======

[![Screen](http://upload.wikimedia.org/wikipedia/commons/d/d9/Node.js_logo.svg)](http://upload.wikimedia.org/wikipedia/commons/d/d9/Node.js_logo.svg)

## Node.js
Node.js 는 **빠르고 쉽고 스케일을 바꾸기 쉬운** 네트워크 어플리케이션을 개발을 위한 크롬 자바스크립트 런타입을 기반 플랫폼이다.(단순 웹 프레임 워크가 아닌 플랫폼! TCP/UDP/File System 등등 많은 부분 지원.)

Node.js 는 가볍고 효율적이며 데이터 집약적인 리얼타임 어플리케이션을 만들기 위해 Event-Driven, Non-blocking I/O 모델을 사용한다.

> Event-Driven Non-bloking 모델이란 기존 웹서버에서 사용하는 Thread Process 기반으로 하는 모델과 비교되는 모델으로 좀더 많은 연결 처리와 Long-alive Connection 은 처리하기에 알맞은 모델이다.


해석하다보니 좀 이상함 ㅋㅋ

### Event-Driven Non-bloking
```js
puts("Enter your name: ");
var name = gets();
puts("Name: " + name);

```

우리가 보통 사용했던 코딩 방식! gets 가 입력을 받을때 까지 멈춰있는다!

```js
puts("Enter your name: ");
gets(function(name) {
     
puts("Name: " + name);
});
```
Event 기반 프로그래밍! 

인풋을 입력할때 받는 이벤트가 있고 프로그래밍은 돌아가는 상태!
기존 웹 프레임 워크는 Process Thread 기반으로 돌아가기 때문에 해당 프로세스가 순간 멈춰있어도 문제없지만
싱글 Thread 를 기반으로 하는 Node 는 절대 멈추도록 설계되면 안됨!
(지금 수준에서는 그냥 이정도만 알아도 충분할듯!)


Event 기반
* Node.js NGINX Vert.x 등등 요즘 트렌드임

Process, Thread 기반

* Apache PHP ROR Django 등등

### 기본 소스 내용 분석?
```js
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(3000, '127.0.0.1');
console.log('Server running at http://127.0.0.1:3000/');
```
1. ```var http = require('http')```
require 는 node 용 라이브러리 링크 함수이다. 
(node 에는 기본으로 require 되어있는 Global Object 들이있다.
process, console, buffer, module 등등이 이에 포함된다.
require 함수는 module 에 존재하므로 따로 require 함수 사용없이 사용할수 있다.)
C에 include 와 Java에 import 와 같은 역활
2. ```http.createServer(function (req, res)```
http 모듈로 서버 생성~ http 모듈은 기본적은 http procotol 을 구현해 놓은 모듈이다!
3. ``` res.writeHead(200, {'Content-Type': 'text/plain'});```
을 통해 Http Content-Type 해더를 text/plain 으로 설정!
```res.end('Hello World\n');``` Http 바디 값을 Hello World 로 설정!
4. ```.listen(3000, '127.0.0.1');```
로컬 호스트에 포트 3000번으로 해당 서버를 설정!
5. ```console.log('Server running at http://127.0.0.1:3000/');```
console.log 는 stdout 으로 아웃풋을 보여준다! console 도 global object 에 포함되어 있어서 어디에서나 사용가능!


### Reference
[Outsider](http://blog.outsider.ne.kr/480) 아웃사이더 블로그

> 추가적으로 궁금한 사항이나 정리할 내용 좋은 자료가 있다면 아래에 정리해주세요 ~