---
title: "React back-end"
comments: true
categories:
  - react
tags:
  - javascript
  - react
  - node
  - redux
  - router
  - code splitting
  - server side rendering
date: "2020-02-07 03:35"
---

### 1. Koa 로 웹서버 만들기

a. Koa 란?

Node.js에서 가장 인기있던 웹 프레임워크인 Express.js 개발팀이 Koa 라는 웹프레임워크를 새로 만들었다.
Express 와의 차이점은 Koa가 훨씬 가볍고, Node.js v7 의 async/await 기능을 아주 편하게 사용할 수 있다는 것이다.

async/await 를 자체적으로 사용할 수 있기 때문에 Koa 는 Node.js v7 이상 버전에서 사용하는 것이 권장된다.


b. Koa 기본 사용법

-서버를 여는 방법-

src/index.js
```java
const Koa = require('koa');
const app = new Koa();

app.use(ctx => {
    ctx.body = 'Hello Koa';
});

app.listen(4000, () => {
    console.log('heurm server is listening to port 4000');
});
```
위 코드를 작성하여 실행시킨 후 브라우저에서 http://localhost:4000/ 에 들어가면, Hello Koa 라는 텍스트가 보인다.


-async/await 사용하기- 

```java
const Koa = require('koa');
const app = new Koa();

app.use(async (ctx, next) => {
    console.log(1);
    const started = new Date();
    await next();
    console.log(new Date() - started + 'ms');
});

app.listen(4000, () => {
    console.log('heurm server is listening to port 4000');
});
```

app.use 라는 함수는 미들웨어다. Koa의 미들웨어 함수에서는 두가지 파라미터를 받는다.
ctx는 웹요청과 응답에 대한 정보를 가지고 있고, next 는 다음 미들웨어를 실행시키는 함수다.
next를 호출하지 않는다면 그 부분에서 요청처리를 완료시키고 응답을 하게 된다.

async/await 문법을 사용하면 콜백이 여러개 겹치는 문제가 사라진다.
이것은 서버에서도 자주 사용하게 되는데 이는 데이터베이스에 요청을 할 때 매우 유용하게 사용된다.


-REST API 사용하기-

REST API 는 요청의 종류에 따라 다른 HTTP 메소드를 사용한다.

  1.  GET: 데이터를 가져올 때 사용한다.
  2.  POST: 데이터를 등록 할 때 사용한다. 혹은, 인증작업을 거칠때도 사용된다.
  3.  DELETE: 데이터를 지울 때 사용된다.
  4.  PUT: 데이터를 교체 할 때 사용된다.
  5.  PATCH: 데이터의 특정 필드를 수정 할 때 사용된다.


라우터를 사용하여 각 메소드에 대한 요청을 준비해보자.

src/api/rest_api/rest_api.controller.js
```java
exports.list = (ctx) => {
    ctx.body = 'listed';
};

exports.create = (ctx) => {
    ctx.body = 'created';
};

exports.delete = (ctx) => {
    ctx.body = 'deleted';
};

exports.replace = (ctx) => {
    ctx.body = 'replaced';
};

exports.update = (ctx) => {
    ctx.body = 'updated';
};
```

src/rest_api/index.js
```java
const Router = require('koa-router');

const rest_api = new Router();
const rest_apiCtrl = require('./rest_api.controller');

rest_api.get('/', rest_apiCtrl.list);
rest_api.post('/', rest_apiCtrl.create);
rest_api.delete('/', rest_apiCtrl.delete);
rest_api.put('/', rest_apiCtrl.replace);
rest_api.patch('/', rest_apiCtrl.update);

module.exports = rest_api;
```

코드가 복잡해지면 모듈화하여 파일을 분리시키는 것이 좋다.
분리시킬수록 코드의 가독성이 높아지고 유지보수 하기에도 편해진다.


### 2. DB 연동하기

서버 폴더에서 connection 파일를 만들어 각 항목의 값을 자신의 DB 설정에 맞게 입력한다.
먼저 사용할 DB와 dotenv를 인스톨 해야한다.

프로젝트의 루트경로에 .env 파일을 만들고 mongoDB의 웹서버의 포트번호와 DB주소를 넣는다.
보안을 위해 따로 파일을 만들어서(.env) DB의 정보를 환경변수로 빼내는 것이다.

dotenv가 여기서 환경변수를 파일안에 넣어서 불러올 수 있도록 해준다.


a. MySQL과 연동하는 방법.

```java
var mysql = require('mysql');

var dbConfig = {
   host: 'localhost',
   user: 'root',
   password: '1234',
   port: 3306,
   database: 'nelp'
   connectionLimit : 50
};

var pool = mysql.createPool(dbConfig);
// Get Connection in Pool
pool.getConnection(function(err,connection){
  if(!err){
    //connected!
  }
  // 커넥션을 풀에 반환
  connection.release();
});


  1.  다수의 Connection 관리 기법
  2.  Pool에서 Connection 얻어서 사용하고 Pool에 반납
  3.  waitForConnections : 풀에 여유 커넥션이 없는 경우 대기 여부
  4.  connectionLimit : 최대 커넥션 개수, 기본 10개
```

b. Mongoose를 사용하여 MongoDB 연동하는 방법.

.env
```java
PORT=4000
MONGO_URI=mongodb://localhost/project_name
```

다음은 src/index.js 파일 상단에 dotenv를 적용한다.

src/index.js
```java
require('dotenv').config(); // .env 파일에서 환경변수 불러오기

const Koa = require('koa');
const Router = require('koa-router');

const app = new Koa();
const router = new Router();
const api = require('./api');

const mongoose = require('mongoose');

mongoose.Promise = global.Promise; // Node 의 네이티브 Promise 사용
// mongodb 연결
mongoose.connect(process.env.MONGO_URI, {
  useMongoClient: true
}).then(
    (response) => {
        console.log('Successfully connected to mongodb');
    }
).catch(e => {
    console.error(e);
});

const port = process.env.PORT || 4000; // PORT 값이 설정되어있지 않다면 4000 을 사용합니다.

router.use('/api', api.routes()); // api 라우트를 /api 경로 하위 라우트로 설정

app.use(router.routes()).use(router.allowedMethods());

app.listen(port, () => {
    console.log('heurm server is listening to port ' + port);
});
```

DB가 연결되면 스키마를 작성해야 한다.
간단하게 말해서 해당 컬렉션에 어떤 항목이 들어가는지, 각 데이터의 형식이 무엇인지
어떤 종류의 값이 들어가는지 정의하는 것이다.

간단한 예로 books에 대한 데이터 스키마를 준비하고 모델을 만들어보자.

책의 정보를 담는 데이터는 어떤 값들이 필요한가?

  1.  책 이름
  2.  저자
  3.  출판일
  4.  가격
  ...

각 항목들이 어떤 형식으로 이루어져야 하는지 생각해보자.

  1.  책 이름: 문자열
  2.  저자: 저자의 정보(저자명, email, 저자의 수 등)
  3.  출판일: 날짜
  4.  가격: 숫자
  ...

위의 데이터로 스키마를 만들어보자.

```java
const mongoose = require('mongoose');
const { Schema } = mongoose;

// Book 에서 사용할 서브다큐먼트의 스키마입니다.
const Author = new Schema({
    name: String,
    email: String
});

const Book = new Schema({
    title: String,
    authors: [Author], // 위에서 만든 Author 스키마를 가진 객체들의 배열형태로 설정했습니다.
    publishedDate: Date,
    price: Number,
    tags: [String],
    createdAt: { // 기본값을 설정할땐 이렇게 객체로 설정해줍니다
        type: Date,
        default: Date.now // 기본값은 현재 날짜로 지정합니다.
    }
});

// 스키마를 모델로 변환하여, 내보내기 합니다.
module.exports = mongoose.model('Book', Book);
```

위의 코드에서 Author와 Book이 스키마이고, 하단에 mongoose.model 을 통하여
만든 스키마를 모델로 변환하여 다른 파일에서 불러와 사용 할 수 있도록 내보낸 것이다.

MongoDB가 설치되어 있다면, mongo 명령어를 통하여 데이터베이스에 접속해 데이터를 조회할 수 있을 것이다.


### 3. SQL과 NoSQL

a. SQL 이란?


SQL은 '관계형 데이터베이스' 또는 '구조화 된 쿼리 언어(Structured Query Language)'의
약자로 특정 유형의 데이터베이스와 상호 작용하는데 사용하는 쿼리 언어라고 한다.

SQL의 데이터는 정해진 데이터 스키마를 따라 데이터베이스 테이블에 저장된다.
각 테이블에는 명확하게 정의된 구조가 있다. 따라서 스키마를 준수하지 않는 레코드는 추가할 수 없다.
명확하게 정의된 구조로 데이터의 무결성을 보장하고, 관계는 각 데이터를 중복없이 한번만 저장한다.
SQL을 사용하면 관계형 데이터베이스 관리 시스템(RDBMS)에서 데이터를 저장, 수정, 삭제 및 검색 할 수 있다.



b. NoSQL 이란?


기본적으로 SQL과 반대되는 접근방식을 따르기 때문에 지어진 이름이다.
스키마와 데이터간의 관계가 없다. NoSQL 에서는 레코드를 문서(documents)라고 부른다.
핵심적인 차이는 SQL에서는 정해진 스키마를 따르지 않는다면 데이터를 추가할 수 없지만,
NoSQL에서는 다른 구조의 데이터를 같은 컬렉션(=SQL에서 테이블)에 추가할 수 있다.

위에서 스키마를 따로 만들어주어야 하는것은 MongDB가 NoSQL이기 때문이다.

NoSQL의 문서는 JSWON데이터와 비슷한 형태를 가지고 있다. 또한 일반적으로 관련 데이터를
동일한 컬렉션에 모두 담고 있다.
이런 방식은 데이터가 중복되기 때문에 불안정하다는 단점이 있다.
예를들면, 실수로 B컬렉션에서는 데이터를 수정하지 않았는데, A컬렉션에서만 데이터를 업데이트 하는 경우이다.

그럼에도 불구하고, 이 방식의 가장 큰 장점은 복잡한 조인을 사용할 필요가 없다는 것이다.
이미 하나의 컬렉션에 모든 데이터가 저장되어 있기 때문이다.
특히 자주 변경되지 않는 데이터 일때 더 큰 장점이 된다.


어떤 데이터베이스를 쓸 것이냐는 어떠한 데이터를 다루는지, 어떤 애플리케이션에서 사용되는지 고려해야 한다.




