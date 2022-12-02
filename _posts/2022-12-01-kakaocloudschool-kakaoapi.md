---
title: "API Server과 JWT 인증 그리고 PASSPORT 모듈"
excerpt: "API 로그인을 구현하기 위한 패키지에 대해 학습하고,
API Server의 개념과 활용에 대해 학습하고,
이를 진행할 수 있는 JWT 인증에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, node, passport, api server, jwp]

toc: true
toc_sticky: true

date: 2022-12-01
last_modified_at: 2022-12-01
---

## 카카오 로그인 구현

### 인증 위해 카카오 passport 구현

- index.js > kakao 관련 설정 작성
- kakao 관련 설정 js 파일 생성

### auth 파일에 라우터 처리 코드 작성

## 게시글 작업

### 게시글 업로드

- router 관련 모듈화하기
- 요청에 따른 게시글 출력

## 팔로우 처리

- 로그인 시 정보 가져오게 하기 위해 passport.deserialized 수정
- page 파일에 유저 정보 초기화 미들웨어 수정
- hashtag 요청 처리
- user 라우팅 파일 추가

# API server

## 1. API(Application Programming Interface)

`프로그램 과 프로그램을 연결시켜주는 매개체`

### 다른 애플리케이션을 개발할 수 있게 도와주는 프로그램(SDK)

### 또는 데이터

- JDK  
  java software development kit
- Sony SDK  
  sony 디바이스 애플리케이션을 만들 수 있게 도와주는 프로그램
- Win API  
  윈도우 애플리케션 을 만들기 위한 함수(C) 의 집합 등...

#### 프로그램 개발에 도움, 여러 프로그램에서 공통으로 사용되는 데이터

- 이럴 경우 데이터 제공
- **Open API**  
  누구나 등록하면 하면 사용할 수 있는 API

`데이터 제공 시 데이터베이스 직접 접근이 아닌`  
`애플리케이션 서버를 통해 제공`

## 2. API Server 가 제공하는 데이터 포맷

### 1) txt / csv

일반 텍스트로 **구분 기호** 포함

- `변하지 않는 데이터 제공`에 주로 사용
- excel 이나 hwp, pdf 등으로 제공되는 경우도 있음

### 2) xml

- eXtensible Markup Language

태그의 **해석을 브라우저가 아닌 개발자**, 개발자 라이브러리 가 하는 형태

- HTML 보다는 엄격한 문법
- `설정 파일 이나 데이터 제공 용도`로 사용

### 3) json

**자바스크립트 객체 형태**로 표현하는 방식

- XML 보다 가벼워 `데이터 전송에 유리`
- `JS 객체 표현법`으로 데이터 표현하여 JS 나 Python 파싱이 쉬움

설정 보다는 `데이터 제공 용도`로 주로 사용

- apple, google, twitter 등은 데이터 전송에만 json

### 4) yaml

**email 표기 형식**으로 표현하는 방식

- `계층 구조를 가진 데이터 표현`에 유리
- 확장자는 yml

구글 `프로그램 설정`에 많이 사용

## 3. API Server 생성 기본 설정

### 프로젝트 생성

### 패키지 설치

### package.json 수정

### 파일 추가 및 .env 수정

### 화면 출력 디렉토리 와 App 설정 파일 생성

- 고정된 양식으로 활용

### 등록 도메인에서만 API 요청이 이루어지도록 도메인, 키 저장

- models 디렉토리에 저장할 모델 생성
  - host  
    클라이언트 URL
  - clientSecret  
    키
  - type  
    free || premium

### 도매인 모델 설정 및 index 파일 수정

### 쳐라한 디음 router 디렉토리의 파일 수정

### 메인 실행 파일에 routes 디렉토리의 파일 등록

## 4. JWT(JSON Web Token)

document : https://jwt.io

- JSON 데이터 구조로 표현한 토큰

API Server 나 로그인 이용 시스템에서 매번 인증하지 않고  
`서버 와 클라이언트가 정보를 주고 받을 때`  
**HttpRequest Header** 에 **JSON 토큰**을 넣어 인증하는 방식

- **HMAC 알고리즘**을 사용하여  
  `비밀키 나 RSA 기법`을 이용해 Public / Private key 로 서명

  - `암호화 키 와 해독키를 다르게 생성`
    - **암호화 키**는 누구나 알 수 있는 형태로 공개  
       **해독키**는 비밀로 하는 방식
      구성
    - **HEADER**  
      토큰 `종류 와 해시 알고리즘` 정보
    - **PAYLOAD**  
      토큰의 `내용물`이 인코딩 된 부분
    - **SIGNATURE**  
      토큰이 `변조`되었는지 여부 확인하는 부분

클라이언트가 서버에게 `데이터를 요청`할 때 **키** 와 **Domain**을 `JSON token에 포함해 전송`

- 서버는 이를 확인해서 유효한 요청인지 판단하고 데이터 전송

**쿠키**는 `동일한 도메인(포트번호까지) 내에서만` 읽을 수 있음  
서버 와 클라이언트 애플리케이션의 `도메인이 다르면 쿠키 사용 불가`

- 설정 시 서로 다른 도메인 간에도 공유 가능하나 `보안 매우 취약`

서버와 클라이언트 애플리케이션의 `도메인이 다른 경우 세션으로 인증 불가`

- 서버에서 클라이언트에게 키를 발급, 클라이언트가 서버 요청 시 키 전송
  - `평문으로 키 전송 시` 중간에 가로채서 사용 가능
  - **키** 와 **클라이언트 URL** 을 합쳐서 **하나의 암호**를 생성해 전송하게 되면  
    서버는 이를 해독하여 `키 와 인가된 클라이언트 URL` 확인 가능
  - 다른 곳에서 탈취시 `URL 이 다르기 때문에 인가된 사용자가 아님`

### 노드에서 JWT 모듈 설치

### JWT 생성에 필요한 문자 코드 .env 파일에 작성

- 암호화 시 필요

### 미들웨어로 JWT 인증 미들웨어 추가

### routers 디렉토리에 토큰 발급 처리 수행하는 파일 작성
