Node.js 설치
===

[http://nodejs.org/](http://nodejs.org/) 방문해서 각각 OS 에 맞는 부분을 설치해주시면 됩니다!

설치후에 커맨드 라인에 which node 라고 입력했을때 
``` /usr/local/bin/node ``` 
처럼 (사람마다 다를수 있음) node 경로가 나온다면 재대로 설치된것 입니다!


```js
var http = require('http');
http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(3000, '127.0.0.1');
console.log('Server running at http://127.0.0.1:3000/');
```

해당 파일을 app.js 파일로 저장한후에
 ``` node app.js ```
를 실행한후 브라우져에서 127.0.0.1:3000 에 접속 해서 hello world 가 나오는것을 확인해주세요~



> 추가적으로 궁금한 사항이나 정리할 내용 좋은 자료가 있다면 아래에 정리해주세요 ~


