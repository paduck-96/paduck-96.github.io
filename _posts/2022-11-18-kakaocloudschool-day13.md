---
title: ""
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop]

toc: true
toc_sticky: true

date: 2022-11-18
last_modified_at: 2022-11-18
---

# Node Express

node 라이브러리 중에서 웹 에플리케이션 서버를 만드는 가장 많이 쓰는 라이브러리

## 1. Middle Ware

Filter, AoP, Proxy Pattern

- Proxy Server  
  예시로, js는 브라우저 밖으로 나갈 수 없기 때문에 프록시 서버에 요청해서 가져오는 것
- Filter  
  작업 과정 상 필터링이 들어가는 것
- AoP
  시점 차이의 기준으로 각 시점의 차이로 인하면 반드시 따로 구분

- 클라이언트의 요청을 처리하기 전이나 처리 한 후에 공통으로 해야 할 작업을 미리 만들어두고 사용하는 것

## 2. session 예제

`세션을 메모리에 저장하면 재사용 안되고, 다른 서버와 공유 어려움`

- 파일에 저장해서 공유하고 재사용 가능하게 변경

  - 필요한 의존성 설치  
    npm i session-file-store

- session 폴더 생성 후 파일 자동 업로드

## 2. 미들웨어 사용

프로젝트에 .env 파일

- 여기에 작성된 내용은 process.env.이름으로 사용 가능

## 3. 파일 업로드 처리

multer 미들 웨어 활용

웹 서비스에서 파일 업로드하려면 `multipart/form-data 형태`로 전송

node-multer로 파일 처리 할 때

- none  
  파일 업로드가 없을 때
- single  
  하나의 파일만 업로드 될 때
- array  
  한 번에 여러 개의 파일이 업로드 가능한데 하나의 파라미터로 업로드
- fields  
  여러 개의 파일을 여러 개의 파라미터로 업로드 하는 경우

`파일 업로드 처리를 할 때 파일 이름을 유일하게 변경하는 경우`가 있음

- UUID 나 현재 시간을 파일 이름에 추가해 생성

실제 운영을 할 때는 애플리케이션 서버 디스크가 아닌  
`별도의 디스크(AWS-S3 서비스 Google-Firebase 서비스) 에 저장 필요`

> 저장을 할 때 디렉토리는 미리 만들어져 있어야 한다

```javascript
multer 객체 생성
  disckStorage // 공간 정보 설정
    destination //경로
    filename //파일명(파일명+날짜+확정식)
  limits // 사이즈
```

#### multer 사용

- 파일 업로드 위한 준비
- 디렉토리 생성 코드 추가
- app.js 파일에 업로드 해주는 설정 추가

1. 파일 1개 업로드

- 파일을 업로드 할 수 있는 클라이언트 파일 생성
  - input file
- app.js 파일에 처리 코드 추가

- 파일에 한글이 포함되어 있을 때 한글이 깨지는 문제  
  파일을 업로드할 때 파일의 원래 이름 같이 전송해 DB에 저장하고, 다운 시 변경  
  || 인코딩 변경

2. 여러 개 파일 업로드를 ajax 형태로

- 클라이언트 파일 생성

3. 여러 개의 파라미터로 전송

- upload.fields([{name:파라미터이름},{name:파라미터이름}...])

## 4. Routing

### 최적의 경로를 탐색하는 것을 의미

#### Node 에서는

`사용자의 요청을 처리하는 부분을 모듈화하는 것`

- 웹 애플리케이션 서버가 커지면 처리해야 할 URL이 늘어나는데  
  하나에서 전부 처리하면 가독성 감소되어 url 모듈화 후 처리

#### 라우팅 기본 요청 과 user 가 포함된 요청, board 가 포함된 요청 분리 구현

- index.js 파일 생성 후 기본 처리 요청 코드 작성
- user.js 파일 생성 후 user 포함된 요청 처리 코드 작성
- board.js 파일 생성 후 board 포함된 요청 처리 코드 작성
- router로 구분하기ㅌ
- url 과 매핑

## 5. URL의 일부 파라미터로 사용

파라미터가 1개일 경우 파라미터를 만들지 않고 URL 에 포함시켜 전송

처리하는 URL을 설정할 때 경로

```javascript
URL/:변수명

//내부
req.params.변수명 으로 사용
```

## 6. Front Controller 패턴

클라이언트의 모든 요청을 app.js 가 받아 각 라우팅 파일에서 처리 진행

- 서버 애플리케이션 설정은 app.js
- 실제 처리는 라우팅 파일
  - app.js는 Front Controller
  - 실 처리 담당 라우팅 파일은 Page Controller

# Template Engine

## SSR

서버가 처리한 결과를 클라이언트로 전달하면  
클라이언트는 해석해서 출력을 함

- `서버의 결과`를 가져오는 것
  - ajax
  - Fetch API
  - Web Push
  - Web Socket
- html 은 구조적으로 서버 데이터를 출력할 수 없기 때문에  
   서버는 엔진을 만들어서 그 엔진으로 서버 데이터를 html 로 출력

> 서버가 처리한 결과를 html 에 출력하도록 해주는 것

## 탬플릿 엔진이란

서버의 데이터를 HTML 과 합쳐서 출력할 수 있도록 해주는 라이브러리

- 거의 모든 웹 프레임워크 들이 가지고 잇으며 종류는 다양

` 서버에서 뷰를 만들어서 클라이언트에게 제공한다는 의미`

- 템플릿 엔진에 대한 학습이 필요하다는 단점

### 1. Jade

저작권 문제로 Pug 로 개명

- https://pugjs.org/api/getting-started.html

- 설치  
  npm i pug
- 설정
  ```javascript
  app.set("views", path.join(__dirname, "출력할 html 위치"));
  app.set("view engine", "pug");
  ```
- 처리
  ```javascript
  res.render("html 파일 경로", 데이터);
  ```
- 데이터 출력

### 2. nunjucks

### 3. 실제 node에서의 출력

템플릿 엔진을 이용한 출력보다는 node를 이용해서 결과를 전송하고  
**ajax나 Fetch API**를 이용해 데이터 출력
