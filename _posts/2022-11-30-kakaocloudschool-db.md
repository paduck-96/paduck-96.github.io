---
title: "Node와 MongoDB 에 대해 학습한다"
excerpt: "Node.js의 데이터베이스 Mongo를 활용하고
Passport 모듈을 활용한 인증에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, node, mongodb, authentication, passport]

toc: true
toc_sticky: true

date: 2022-11-30
last_modified_at: 2022-11-30
---

# Node + MongoDB + mongoose(Node 의 ODM)

## RDBMS 와 NoSQL 의 차이

- RDBMS  
  `스키마를 생성하고 데이터를 저장`하는 형식  
  데이터의 값으로 `다른 테이블의 데이터나 배열을 삽입할 수 없기` 때문에  
  `테이블을 분할`, **Foreign Key** 와 **Join**을 이용해서 여러 종류의 데이터를 저장  
  일반적으로 `엄격한 트랜잭션`을 적용

- NoSQL  
  `스키마를 생성하지 않고 데이터를 저장하는 것이 가능`  
  `스키마 구조 변경` 되도 `데이터 구조 변경`할 **필요없이** 데이터 저장이 가능  
  데이터의 값으로 `객체 나 배열이 가능`  
  `하나의 Collection에 여러 종류의 데이터`를 저장

  - Join 을 하지 않아도 되기 때문에 처리 속도가 빠를

  Embedding 이나 Linking의 개념을 사용  
  `느슨한 트랜잭션`을 적용

  - 복잡한 거래가 없는 경우나 비정형 데이터만을 저장하기 위한 용도로 많이 사용

## ODM

Relation 이라는 개념 대신에 `Document를 하나의 객체에 매핑`하는 방식  
하나의 `Document에 대한 모양`을 만들고 사용해야 하기 때문에 NoSQL의 `Collection 도 하나의 정형화된 모양` 필요

- **mongoose**  
  MongoDB에서 ODM을 도와주는 대표적인 라이브러리

# Authentication

## 1.Authentication(인증) 과 Authorization(인가)

=>**인증**  
계정 관련, 로그인 관련  
=>**인가**  
권한 관련

## 2.인증을 구현하는 방법

=>로컬 로그인  
회원 정보를 `저장하고 있다가 인증`  
회원 정보를 저장할 때는 `비밀번호는 복호화가 불가능한 방식`을 사용

- `개인을 식별할 수 있는 정보`를 `마스킹 처리`
- `복호화가 가능한 방식의 암호화`를 활용해야 합니다.

=>OAuth(공통된 인증 방식) 로그인  
다른 서버(카카오 나 구글)에 `저장된 인증 정보를 활용`해서 인증을 하는 방식

## 3.인증을 위한 프로젝트 기본 설정

=>로그인을 할 수 있도록 회원 가입을 하고 로그인 처리를 수행하고 간단한 글 과 파일을 업로드 할 수 있는 프로젝트

### 프로젝트 생성 및 패키지 설치

```javascript
// 매번 동일한 양식 위해 작성
npm install express morgan dotenv compression morgan file-stream-rotator multer cookie-parser express-session express-mysql-session mysql2 sequelize sequelize-cli nunjucks
// nodemon
npm install --save-dev nodemon

//sequelize(node 의 ORM) 초기화
npx sequelize init
```

### 디렉토리 생성

=>**views**  
`화면에 출력할 파일(View)`이 저장되는 디렉토리

=>**routes**  
`사용자의 요청이 왔을 때 처리하는(Controller) 라우팅 파일`이 저장되는 디렉토리

=>**public**  
`정적인 파일(resource)`들이 저장되는 디렉토리

### 프로젝트에 .env 파일을 생성하고 작성

- 소스 코드에 노출되서 안되는 내용
- 개발 환경에서 운영 환경으로 이행(Migration)할 때 변경될 내용

`이 내용은 실행 중에는 변경되지 않는 내용`

- 데이터베이스 접속 정보 나 암호화를 하기 위한 키 또는 서버 포트 번호
- 대부분 실행 중에는 변경되지 않지만  
  `개발 환경에서 운영 환경으로 이행 할 때 변경될 가능성이 높은 내용`

### 모듈화를 활용한 Router 파일 작성

### View layout 활용한 파일 작성

### DB 설계

- 테이블 구조 작성
  - 로그인
  - POST 테이블
  - HashTag 테이블
- 테이블 관계 설정
  - User Post  
    1:N
  - HashTag Post  
    N:N
  - User User  
    N:N

### 각 테이블 위한 모델 내용 작성

- 저장할 항목 그리고 테이블 간의 어떤 상관관계를 맺고 있는지 고민
- 각 테이블 모델 정의 후 index.js 에서 통합
- .env 효율적으로 활용

### config.json 파일 등 수정

### 서버 실행 시 테이블 생성

### Passport 모듈

Node 의 인증 작업 도우미 모듈

- 인증 작업  
  로그인 성공 시 세션을 생성해 아이디 나 기타 정보 저장  
  이후, 세션 정보 존재 여부를 확인해 로그인 여부를 판단
  - 로그아웃 시 세션 정보 삭제

세션이나 쿠키 처리를 직접하지 않고 이 모듈의 도움을 받아 쉽게 구현

Social 로그인 작업 쉽게 처리

### 로컬 로그인 구현

```javascript
passport, passport - local, bcrypt; // 모듈 설치
```

- app.js 에 passport 설정 추가
- passport 모듈 사용

  - passport/index.js 설정
    - 로그인 여부는 request 객체의 isAuthenticated() 함수

- 로그인 여부 판단 함수 작성

  - middleware 로 작성해 필요 구문에 넣기

- 회원 가입, 로그인, 로그아웃 처리를 위한 함수 작성
- passport 디렉토리에 로컬 로그인 파일 생성

### 카카오 로그인 구현

```javascript
passport - kakao;
// https://www.passportjs.org/packages/passport-kakao/
```

- 모듈 설치
- 카카오 로그인 설정

  - api 키 받아오기
  - 플랫폼 등록
  - 로그인 활성화

- .env 설정
