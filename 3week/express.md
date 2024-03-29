Express 작성
===
### 소개
http 모듈만으로는 많은 기능을 가진 API 나 웹페이지를 만들기에는 부족하다. 파라메터를 파싱한다거나 쿠키나 세션을 사용한다거나 HTML 랜더링 엔진을 사용한다거나 하려면
좀더 많은 기능이 필요하다. 이런 기능들을 가지고 있는 Node.js 용 웹 프레임 워크가 Express 이다.

| 언어       | 기본적인 웹프레임워크          | 좀더 많은 기능이있는 프레임워크  |
| ------------- |:-------------:| :-----:|
| Ruby     | Sintra | RubyOnRails  |
| Pyhton     | Flask | Django  |
| Javascript     | Node.js http | Express  |

구지 비교를 했지만 정확한 분류는 아니다!
너무나 많은 프레임 워크가 존재하므로 가장 많이 사용하는 프레임 워크 기준으로 공부하는게 좋다.

### 설치
```npm install -g express```

### Express Init
```express myproject```
Express 용 Project 생성!

```npm install```
해당 프로젝트에 종속 라이브러리들을 설치!

```node app.js```
Express 서버 실행

### app.js

```js
/**
 * Module dependencies.
 */

// 사용할 라이브러리들을 포함 시킨다.
var express = require('express'); // 우리가 사용할 express 엔진 
var routes = require('./routes'); // express 에서 기본 생성해주는 라우팅 해당 파일을 통해 여러 url 들을 라우팅한다.
var user = require('./routes/user'); // User 관련 라우팅
var http = require('http'); // node 기본 모듈인 http
var path = require('path'); // node 기본 모듈인 path

var app = express(); # express 생성 함수 호출

// all environments
app.set('port', process.env.PORT || 3000); // || 연산자는 process.env.PORT 가 NULL 값이라면 3000을, 값이 존재한다면 그값을 그대로 사용하도록 한다. process 는 node 에 기본으로 있는 global object 라서 어디에서나 사용 가능하다. node 를 실행시킬때 PORT=3001 node app.js 실행 해주면 process.env.PORT 값이 3001이 된다. 그렇게 되는 이유는 노드가 알아서 그렇게 하기 때문! app.set 은 port 키값으로 해당 값을 저장하는것이다.
app.set('views', path.join(__dirname, 'views')); // views 키 값에 __dirname (현재 Root 디렉토리)/views 경로를 지정
app.set('view engine', 'jade'); // 기본 view engine 을 jade 로 설정, html 파일을 표현할수 있는 여러 view engine 이 존재한다. ejb jade 등등
// Express 에서 사용할 미들웨어들 정의
app.use(express.favicon()); // favicon 미들웨어 ( 웹브라우져 주소창에 보이는 작은 이미지)
app.use(express.logger('dev')); // express 에 log level 을 dev 로 설정 ( dev 로 하면 좀더 많은 로그들을 출력함)
app.use(express.json()); // json 을 알아서 파싱함 
app.use(express.urlencoded()); // url 을 인코딩함
app.use(express.methodOverride()); // methodOverride 기능 ( 웹 브라우져에서 PUT PATCH 는 지원하지 않으므로 해당 기능을 지원하기 위해 _method 변수를 사용 - 몰라도 됨)
app.use(app.router); // 유저가 정의한 라우팅 사용
app.use(express.static(path.join(__dirname, 'public'))); // static file ( 이미지 동영상 css js) 등을 서비스

// development only
if ('development' == app.get('env')) { // 개발 버젼으로 서버가 돌아가고 있다면 (NODE_ENV=development node app.js 로 실행하면! - development 가 기본)
  app.use(express.errorHandler());  // express 의 errorHandler 를 사용한다.
}

app.get('/', routes.index); // http://127.0.0.1 로 요청이 들어오면 routes.index 함수로 보낸다
app.get('/users', user.list); // http://127.0.0.1/users 로 요청이 오면 user.list 함수로 보낸다.

http.createServer(app).listen(app.get('port'), function(){ // 서버 생성 하고 listen 이벤트 생성
  console.log('Express server listening on port ' + app.get('port'));
});
```

### 미들웨어
미들웨어란 웹 프레임워크와 나의 프로그램과 사이에 존재하는 프로그램이라고 생각하면된다.
실제 웹 프레임 워크에 있는 모든 기능을 사용하지는 않는다. 모바일용 API 를 만든다면 해당 웹 프레임워크에 쿠키나 MethodOverride 등등 많은 기능을 사용하지 않는다.
해당 기능을 제공하는 미들웨어를 제거해서 좀더 가볍고 빠른 서버를 만드는게 가능하다!
(사용하지 않는 기능이 있으면 불필요하게 특정 상황을 체크한다거나 HTTP 해더에 불필요한 정보를 포함한다거나 한다!)
Express 에서 미들웨어에는 순서가 중요하다!

Node.js 기본 프로젝트를 실행시킨후 웹브라우져로 접속하면 요청이 두번들어오는것을 알수있다.
> http://127.0.0.1

> http://127.0.0.1/favicon // 브라우져가 해당 웹싸이트에 favicon 을 찾기위해 추가적인 요청을 진행한다.

맨위에 ```app.use(express.favicon()); ``` favicon 미들웨어를 사용하면
favicon 요청은 해당 미들웨어에서 처리 되고 아래로 추가적인 요청이 들어가지 않는다.
favicon 을 서비스하기 위해 구지 json urlencoded 미들웨어를 사용할 이유가 없다!

추가적으로 앱 API 서버를 만든다면 구지 favicon methodOverride 기능은 필요하지 않다!
즉 삭제해도 좋다!

미들웨어를 사용하면 추가적인 기능을 추가하거나 필요없는 기능을 쉽게 뺄수있어좋다! 다른 웹 프레임 워크에서 미들웨어 개념이 있지만 Express 만큼 유연하지 못하다! 그래서 Express 가 짱임!

추가적으로 나만의 미들웨어를 사용 해 볼 수 있다.
```js
app.use(function(req,res){
  console.log("My MiddleWare");
});
```
위에 내용울 ```app.router``` 아래에 두면 My middleware 로그가 남지 남지 않는다.
즉 웹브라우져의 요청은 app.route 에서 처리되고 아래로 내려가지 않는다!
만악에 http://127.0.0.1/images/test.png 로 접속한다면 로그는 남게 될것이다. image 파일을 처리하기 위해서는 express.static 미들웨어 까지 내려가야 하기 때문이다!

자세한 내용은 뒤에 추가 설명해주세요~

### 라우팅 처리
> 정리할 사람~

### Exports
> 정리할 사람~

### Reference
[Express Doc](http://firejune.io/express/) 한글로 되어있는 Document

> 추가적으로 궁금한 사항이나 정리할 내용 좋은 자료가 있다면 아래에 정리해주세요 ~