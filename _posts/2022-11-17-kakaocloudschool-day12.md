---
title: ""
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, javascript, node]

toc: true
toc_sticky: true

date: 2022-11-17
last_modified_at: 2022-11-17
---

# 패키지 시스템

## NPM

### Node Package Manager

노드에서의 패키지 매니저이나 지금의 대다수의 자바스크립트 라이브러리들이  
저장소에 있기 때문에 자바스크립트 라이브러리들은 NPM을 이용해서 사용

#### 1. package.json

노드에서 패키지 관리를 위한 설정 파일

- java 에서의 build.gradle 이나 pom.xml 역할 수행

패키지를 설치하게 되면 패키지 정보 전부 작성

- package-lock.json 파일 생성
  - 직접 설치 뿐만 아니라 의존 관계가 있는 모든 패키지 정보 기재

#### 2. 패키지 설치

```javascript
npm install 패키지 이름 나열(공백으로 구분)
```

- 개발 과정에서만 사용하고 배포 할 때 제외

```javascript
--save-dev 패키지 이름
```

- 모든 프로젝트에서 사용

```javascript
-g 패키지 이름 // 현재 경고 발생
--location==global 패키지 이름 //권장
  Mac / Linux에서
  sudo npm install --location global 패키지 이름 //관리자 모드
```

- node_modules 디렉토리에 패키지 저장
  - 전역 설치 오류 시 로컬로 재설치
    - node 명령에 대한 path 설정 오류
    - node를 2개 이상 설치

#### 3. 패키지 설치

1. 프로젝트 생성

- package.json 생성 유무
- express 패키지 설치  
  웹 애플리케이션 서버를 만들어주는 패키지
  - 여러 패키지 설치일 경우 공백으로 구분
- nodemon 패키지  
  소스 코드 수정 시 자동 재실행 되고 개발 과정상 사용
- rimraf 패키지  
  윈도우에서 터미널에서 rm 명령을 사용하기 위해 설치

#### 4. 패키지 재설치

배포 나 코드를 가지고 갈 때 node_modules 폴더는 용량이 커 제외

- package.json 을 가지고 npm init 명령 수행

#### 5. 패키지 버전

> Major Version . Minor Version . Patch

- Major Version  
  하위 버전과 호환 어려울 수 있음
- Minor Version  
  기능 변경
- Patch  
  오류 수정 했을 때 변경

- alpha  
  개발자 환경 사용자 테스트 버전
- beta  
  사용자 환경 사용자 테스트 버전
- 화이트 박스  
  사용자 환경 정의 테스트
- 블랙 박스
  입/출력 등 정의 테스트

#### 6. npm 관련 명령어

- npm uninstall 패키지 이름  
  패키지 삭제
- npm search 검색어  
  패키지 검색
- npm publish  
  패키지 배포
- npm unpublish  
  패키지 배포 취소
  - 배포 한 후 24시간 이내에만 가능

# node를 이용한 웹 서비스 만들기

실제로는 express 모듈을 더 많이 이용함

## 1. 최근의 Web

### Web 3.0

- Semantic Web 개념 등장  
  로봇이 정보 자원의 뜻을 이해하고 논리적 추론까지 가능
  - 명확한 의미 전달 중요
  - **Rest API**
- 속도의 변화
- 인공지능
- 자신만의 컨텐츠 나 정보 를 구성할 수 있게 사용자 권한 증가
- 블록체인

### WOA(Web Oriented Architecture)

#### 사용자의 요구 사항 변경

여러 디바이스를 사용하고 디바이스끼리 끊어짐이 없는(Seamless) 서비스 요구 증대

- 이를 구현하기 위한 웹 기술 각광

기술의 변화

- 인프라 측면  
  클라우드 나 가상화 기술
- 소프트웨어 측면  
  WOA => `전체 시스템 아키텍처를 웹을 중심으로 설계`

### Framework 를 이용한 애플리케이션 개발

## 2. 웹 서비스 구축 방식

### 1. 정적 웹 서비스

요청이 오면 요청에 해당하는 HTML 페이지를 찾아서 출력하는 방식

### 2. CGI(Common Gateway Interface)

클라이언트의 요청이 오면 서버가 작업을 수행해서 결과 전송하거나 화면 전송

- Perl이 시초
  사용자의 요청을 별도의 프로세스로 만들어서 처리
  - 하나의 요청 처리 전까지 다른 요청 처리 불가

### 3. Application Server 방식

`사용자의 요청을 Thread를 만들어서 처리`

여러 사용자의 요청을 한꺼번에 처리하는 것처럼 처리

- Java > Servlet(JSP) > Spring Framework
- C# > asp.net
- JavaScript > node.js Framework
- PHP > laravel Framework
- Python > Flask 나 Django Framework
- Ruby > Rails

### 4. 웹 프로그래밍의 구조

웹 브라우저 <-> 웹 서버 <-> 애플리케이션 서버(Controller, Service, Repository) <-> 데이터 저장소

- 언어 나 프레임워크 는 애플리케이션 서버를 만들기 위한 기술
- Serverless  
  서버가 없는 것이 아니라 `서버 직접 구현 불필요`
- **`Request`**  
  웹 브라우저에서 서버에게 요청하는 것
- **`Response`**  
  서버가 웹 브라우저에게 대답 하는 것

## 3. http 모듈

내장 모듈이므로 별도 설치 불필요

### 1. 서버 생성

```javascript
http모듈.createServer((request, response) => {
  내용;
});
```

### 2. 서버 실행

```javascript
서버객체.listen(포트번호, 아이피주소);
//현재 컴퓨터에 여러 ip가 있을 경우만
```

### 3. 서버 종료

```javascript
서버객체.close();
```

### 4. 서버에 발생하는 이벤트

- request  
  클라이언트의 요청이 있을 때
- connection  
  클라이언트가 접속했을 때
- clientError  
  클라이언트 오류가 발생했을 때

### 5. Request 객체

- url  
  요청한 URL
- method  
  요청 방식
  - GET, POST, PUT, PATCH, DELETE, OPTION

### 6. 웹 서버 만들고 응답 생성하기

1. js 파일 추가 후 작성

2. 서버 실행

3. 클라이언트에서 접속

4. 서버 중지  
   기본 커맨드는 Ctrl + C

5. 서버에서 html 파일 읽어서 출력

### 8. Request 객체

URL  
클라이언트의 요청 경로

- 요청 경로를 만들 때는 이해하기 쉬운 경로로 만들어 주어야하고,  
  `_ 사용은 하지 않는 것을 권장`

- Method  
  요청 방식
  - **Get**  
    서버 자원을 가져올 때 사용(조회 - Read)
  - **POST**  
    서버에 자원을 등록하고자 할 때 사용(삽입 - Create)
  - **PUT**  
    서버의 자원을 수정하고자 할 때 사용(수정 - UPDATE)
  - PATCH  
    서버 자원을 일부분만 수정하고자 할 때 사용(수정 - UPDATE, 비권장)
  - **DELETE**  
    서버 자원을 삭제하고자 할 때 사용(삭제 - DELETE)
  - OPTIONS  
    요청을 하기 전에 통신 옵션을 설명하기 위해서

### 9. REST(Representational State Transfer) API

서버의 자원을 정의하고 자원에 대한 URL을 지정하는 방법

- `URL과 Method 만으로 작업을 예측할 수 있도록 하는 것`
  - Url은 /member 이고 Method 는 POST 라면 회원 가입

`클라이언트의 종류에 상관없이 동일한 작업은 동일한 URL로 처리`

> 클라이언트 애플리케이션와 서버 애플리케이션 분리 구현

- 서버는 클라이언트의 뷰를 만들지 않고, 데이터를 전송
  > 하나의 프로젝트로 구현하면 모바일의 Native App 과 Browser의 요청 응답을 동일한 URL로 처리할 수 없음

`이렇게 만들어진 서버를 RESTful 하다고 함`

### 10. AXIOS 라이브러리

브라우저 나 Node.js 에서 Promise API를 이용해서 http 비동기 통신 도와주는 API

- 자바스크립트의 Fetch API를 사용하기 쉽게 해주는 라이브러리

```
axios           fetch api
별도 설치       설치 불필요

XSRF 보허       별도 제공 안함

응답 결과는       .json() 호출로
JSON 파싱       파싱 결과 생성 가능

요청 취소,
타임아웃        해당 기능 없음
설정 가능

download 진행
여부 확인 가능  해당 기능 없음
```

- XSRF(Cross-Site Request Forgery)  
  쿠키만으로 인증하는 서비스의 취약점을 이용해서 사용자 모르게 특정 명령 처리

  - 브라우저 삽입 요청을 위한 폼의 URL을 복사해 다른 곳에서 데이터 삽입

- Promise 를 이용한 삽입

```javascript
const axios = require("axios");
axios
  .요청메서드(url)
  .then((response) => {
    //성공했을 때 내용
    //response는 가져온 데이터를 파싱한 결과
  })
  .catch((error) => {
    //에러 발생했을 때
    //에러 객체
  })
  .then(() => {
    //성공 실패 상관 없이 수행
  });
```

### 11. Cookie 와 Session

HTTP 나 HTTPS 는 상태가 없음

- 기본적으로 요청에 의해 연결되고, 응답 하면 연결 해제되어 이전 상태 알 수 없음
  - 클라이언트 와 서버가 이전에 어떤 상태였는지 알기 위하여

#### Cookie

클라이언트에 저장해서 클라이언트가 서버에게 요청할 때마다 전송되는 객체

- http 나 https 요청의 헤더에 저장
- 이름 과 값의 구조

```javascript
//node 의 http 모듈
response.writeHead(코드, { "Set-Cookie": "쿠키이름=값" });
//여러 개의 쿠키는 ;로 구분
```

- 쿠키 옵션
  - 이름
  - Expires(만료시간)
    - 날짜
  - Max-age(만료시간)
    - 초
  - Domain  
    도메인
  - Path  
    URL
  - Secure  
    Https인 경우만 전송
  - HttpOnly  
    자바스크립트에서 수정 못하도록 하는 경우

#### Session

클라이언트의 정보를 서버에 저장하는 기술

- 서버의 메모리에 생성
  - 한계가 있고, 휘발성
    - 파일(느리지만 저비용), DB(빠르지만 고비용)으로 해결

클라이언트에 저장할 경우 노출될 때 수정할 수 있기 때문에 보안에 취약  
`노출이 되면 안되는 데이터를 서버 저장`  
클라이언트에는 구별할 수 있는 `세션 키만 저장`

- 클라이언트 와 서버가 동일한 도메인 일 경우만 가능
  - 쿠키를 이용하면 달라도 가능
  - 로그인 정보 저장했
    - `JWT(Json Web Token)` 권장되어 빈도 감소

#### 대안

Web Storage, Web SQL, Index DB 같은 HTML5 API 이용

### 12. https 모듈

http 서버를 https 로 변경하기 위한 모듈

- https 는 암호화를 위한 인증서가 필요  
  무료 / 유료 인증서를 발급받아야 함

https 는 데이터가 암호화되어 전송되어 가로채도 변경 불가

- 전송 간에 암호화 불필요하나 혹시 모르니 권고

https 서버거 아니면 접속 못하게 하기도 함

- 스마트폰에서는 http 접속 위해 별도 설정 필요
- https 모듈 속도를 개선한 https2 모듈 있ㅇ므

### 13. Cluster

CPU 코어를 전부 사용할 수 있도록 해주는 모듈

여러 개의 연산을 동시에 수행할 수 있도록 해주는 모듈

- 직접 서버 설정을 할 경우 이용되나 Cloud로 직접 설정 빈도 감소

## express 모듈을 이용한 웹 서버 생성 및 실행

### 1. Express 모듈

- 내장 모듈이 아님

http 모듈을 가지고 웹 서버를 만들 수 있는데, 가독성 과 확장성 낮음

http 모듈보다는 코드 관리가 용이하고 편의성이 높은 모듈

- 웹 서버 생성 모듈은 여러가지가 있고 생기는 중
- 가장 대중적인 웹 서버 모듈

### 2. 패키지 설치

- Express(웹 서버 제작 모듈)
- Nodemon(소스 코드 수정 시 자동 재시작 해주는 모듈)

### 3. package.josn 파일 설정 수정

- main 속성 시작 파일 이름 설정
- script 속성에 "start" : "nodemon app" 태그 추가
  - npm start 명령에 nodemon app 수행

### 4. express web server의 기본 틀

```javascript
//express 모듈 가져오기

//웹 서버 인스턴스 생성
const app = express();

//포트 설정
app.set("port", 포트번호);

//사용자의 요청 처리 코두

app.listen(app.get("port"), () => {
  //서버 정상 구동 시 내용
});
```

### 5. 사용자 요청 처리 함수

#### 1. 종류

요청 방식에 맞는 함수를 적용

- app.get
- app.post
- app.delete
- app.put
- app.patch
- app.options

#### 2. 함수 매개변수

- URL
- 2개의 매개변수를 갖는 콜백 함수
  - url 요청이 왔을 때 이 함수가 호출
    - 사용자의 요청 객체(req)
    - 사용자에게 응답 객체(res)

#### 3. 사용자에게 응답

```javascript
response.send(문자열) //직접 작성
response.sendFile(html 파일 경로) //파일로 출력
```

### 6. 웹 서버 만들기

#### 1. 출력할 파일 하나 생성

#### 2. package.json 파일에 entry point 설정 파일 생성

#### 3. 웹 서버 코드 작성

#### 4. 구동 후 브라우저에서 확인

- 일반적으로 npm start 명령어로 진행되길 추천

### 7. 요청 객체

일반적으로 request 객체

- 클라이언트의 요청 정보를 저장하고 있는 객체

  ```javascript
  request.app; // app 객체에 접근

  request.body; // body-parser 미들웨어가 만드는 요청 본문 해석 객체
  //post 나 put 요청의 파라메터

  request.cookies; // 쿠기 정보 가지는 객체
  request.ip; // 요청 전송한 클라이언트 ip 정보

  request.params; // 라우투 매개변수
  request.query; // query string
  // get 이나 delete 요청에서 파라미터 읽기
  request.get(헤더이름); // 헤더 값 가져오기
  // 주로 인증에서 사용
  // API 사용 권한을 토큰 이용 발급하고
  // 토큰 값을 헤더에 저장해서 전송
  request.signedCookies; // 서명된 쿠키 정보
  request.session; // 세션 객체
  ```

### 8. 응답 객체

일반적으로 response 객체

```javascript
response.cookie(키, 값, 옵션) // 쿠키 생성
response.clearCookie(키, 값, 옵션) // 쿠키 삭제

response.end() // 데이터 없이 응답 전송

response.json(JSON 문자열) // json 형식으로 응답
response.redirect(URL) // 리다이렉트할 URL
response.render(뷰이름, 데이터)
// 템플릿 엔진을 이용해서 서버의 데이터를 html에 출력
// 서버 렌더링

response.send(메시지) // 메시지 화면에 출력
response.sendFile(파일 경로) // 파일 일어서 화면에 출력

response.set(헤더이름, 값) // 헤더이름 설정

response.status(코드) // 응답 코드 설정
```

### 9. dotenv

.env 파일을 읽어서 process.env 로 생성해주는 패키지

```javascript
// .env 파일
작성 내용

// 소스 코드 파일
process.env.객체 //사용
```

`환경의 변화(개발 환경, 운영 환경, 테스트 환경)로 변경되는 설정`을 `별도의 텍스트 파일`에 만들어두면 환경 변화될 때 텍스트 파일의 내용만 변경하면 되어 재컴파일이나 빌드 불필요

- 데이터베이스 접속 위치
- API 키 등

### 10. Middle Ware

사용자 요청 처리 전이나 후에 동작할 내용을 수행하는 객체  
`원래는 서버와 클라이언트 사이에 존재해 프로세스의 플로우를 조절하는 것`

- 필터링  
  요청을 처리 하기 전에 수행하는 일
  - 유효성 검사 작업
  - 로그인 확인 작업
- 변환  
  요청을 처리한 후 수행하는 일
  - wrapping

```javascript
app.use(미들웨어); //node middleware
//모든 요청에서 동작

app.use(url, 미들웨어); // 해당 url 에서만 미들웨어 동작

next(); //현재 미들웨어 에서 다음으로 넘어가는 함수
```

#### 1. morgan

로그 기록 미들웨어

```javascript
morgan(format, options);
//format
dev; //개발용, 배포 할 때 삭제
tiny;
short;
common;
combined;

//options
immediate; //response 가 아닌 req 기록(에러 발생해도 기록)
skip; //로깅을 스킵하기 위한 조건 설정
stream; // 로그는 화면 출력이지만, 파일 출력을 원할 때
```

로그 파일을 생성해주는 패키지

```javascript
npm install file-stream-rotator
```

- 주기적으로 파일을 생성해서 로그 기록 가능

로그 형식
HTTP요청방식 요청url 상태코드 응답속도 트래픽

- 자세한 로그 원할 경우 winston 패키지 사용

#### 2. 날짜 별로 로그 파일에 로그 기록

- 패키지 설치
  - morgan, file-stream-rotator

#### 3. Static(정적)

내용이 변하지 않는;

- 정적인 파일의 경로를 설정하는 미들웨어

  ```javascript
  app.use(url, express.static(실제 경로));
  //url 요청이 오면 실제 경로에 있는 파일 출력
  //요청 경로와 실제 파일 경로를 다르게 하기 위해 사용
  ```

#### 4. body-parser

요청의 본문을 해석해주는 미들웨어로 express에 딸려옴

> `클라이언트에서 post 방식이나 put(patch) 방식으로 데이터를 전송할 때 그 데이터를 읽기 위한 미들웨어`

```javascript
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
```

- 파일을 전송하는 경우에는 다른 미들웨어 사용 필요

#### 5. compression

> `데이터를 압축해서 전송하기 위한 미들웨어`

- 클라이언트에게 결과를 전송할 때 `압축을 해서 전송하여 트래픽 감소`

  ```javascript
  // npm install compression

  const compression = require("compression");
  app.use(compression());
  ```

#### 6. 쿠키 해석 미들웨어

`브라우저에 저장`

```javascript
app.use(cookieParser(키)); // 서버에서 쿠키 읽기 가능
request.객체.cookies; // 쿠키 넘겨 받을 수 있음
```

#### 7. express-session

`세션(사용자의 정보를 서버에 저장) 관리를 위한 미들웨어`

- 클라이언트 측에서 이전 작업에 이어서 다른 작업을 할 때
- 크거나 많아지면 성능 저하
  - 파일이나 DB 에 유지

#### 8. 세션 예제

express-session 패키지로 세션 설치
브라우저에서 접속을 하고 새로 고침 한 후 다른 브라우저와 비교
