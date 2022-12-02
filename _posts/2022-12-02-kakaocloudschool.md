---
title: "API Server와 CORS 설정, 기타 모듈"
excerpt: "API 서버에 대해 학습하고
CORS 설정에 대해 학습하고
Node 의 나머지 모듈에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, node, mariadb]

toc: true
toc_sticky: true

date: 2022-12-01
last_modified_at: 2022-12-01
---

## API server VS Server Rendering

기본적인 요청과 응답의 과정

### Client -요청-> Web Server -> Application Server -> DB

DB에서 응답을 하는데 그 결과값이  
Data이면 api server  
View이면 서버 렌더링이라고 함  
`논리적인 개념의 차이일뿐, 물리적으로 같음`

### Authentication

화면 출력 과 처리를 하나의 애플리케이션으로 수행

- 서버 렌더링
- 클라이언트의 종류가 다양하고 확장 가능성이 있음

### 서버 렌더링의 형태 활용

웹 브라우저에서 접속을 처리하는 부분 생성

- 안드로이드 기기에서의 화면 랜더링 불가능
  - 안드로이드 브라우저 , 안드로이드 어플리케이션 중
- 안드로이드 기기 처리를 위한 URL 추가 생성
  - 동일 요청에 대한 URL 2개

`서버는 데이터만 전달, 클라이언트를 별개로 만들어 처리`

- 클라이언트 확장성이 높음

## JWT 이어서...

### 도메인 확인 해 토큰 발급 코드 작성

### 라우터에 JWT 도메인 등록

### 데이터 리턴하는 post 요청 등록

---

## Client Server 생성

### 초기 패키지 와 설정 저장

### 출력 폴더 views 생성

### 해당 도메인에 따른 키 값 받아와 .env 등록

### routing 관련 내용 작성

### 토큰 발급 여부 테스트 확인

### 실질적인 API 요청 작성

### clientServer 에서 요청 전송 해 결과 확인

---

## 서버 수정

### 사용량 제한

API Server 생성 후 데이터 무제한 제공시 트래픽 많이 발생하여 속도 제한

- DDos 공격의 대상

> 일정한 주기로 제한하거나  
> 사이즈 나 횟수 제한

- 이러한 제한은 클라이언트가 아닌 서버 쪽 미들웨어로 처리

### 기존 서버 코드 수정

`기존 코드를 무조건 바꾸는 것은 위험`

- deprecated
- 서비스 중지 메시지 전송
  - 후 새로운 내용 작성

### Node의 미들웨어 / Java 의 Filter / Spring 의 Interceptor, AOP

`실제 처리를 하기 전이나 후에 동작하는 로직을 작성하는 용도`

- Business Logic 과 Common Concern 의 분리를 위해 공통 처리 진행

### 사용량 제한 프로그램 작성

- 사용량 제한을 위한 패키지 추가
  - express-rate-limit
- 사용량 제한 미들웨어 작성

- routes 디렉토리에 새로운 버전의 요청 처리 프로그램 작성
  - 이후 이전 토큰 파일 변경
    - 미들웨어로 새로운 버전 파일 가져오고 전체 적용
- 실제 서버 구동 파일(App.js)에서 라우터 등록

---

## CORS(Cross-Origin Resource Sharing)

### SOP(Same Origin Policy)

동일 출처 정책

- 어떤 출처에서 불러온 문서, 스크립트가  
  다른 출처의 리소스와 **상호작용 제한**하는 보안 방식
- XMLHttpRequest 와 Fetch API 가 `다른 출처 리소스 요청 시` 적용
  - img, link, script, video, audio, object, embed, applet 태그 제외

### CORS

교차 출처 정책

- 추가 HTTP 헤더를 사용해 한 출처에서 실행 중인 웹 애플리케이션이  
  **다른 출처의 자원에 접근 권한**을 부여해 `브라우저에 알려주는 것`

- Ajax 나 Fetch API 가 다른 출처 리소스를 사용하려면  
  **CORS 헤더를 포함한 응답** 반환해야 함

`서버 생성 시 우선 고려하여 코드를 작성해야 함`

- 이미 생성되었을 경우 **Proxy 이용** 고려

---

- CORS 구현 패키지 설치
  - npm i cors
- 서버에 해당 설정 적용
  - 클라이언트 서버 X

## 기타

### Request 모듈

node 에서 다른 서버의 데이터 가져오기

### WebSocket

클라이언트 와 서버 연결을 유지한 상태로 데이터 주고받는 HTML5 API

- http/s 는 `연결 유지하지 않고`, `Header 의 오버헤드가 큼`
  - 짧은 메시지 자주 전송 시 적합하지 않은 프로토콜

### Push (Server Sent Events)

클라이언트의 요청이 없어도 서버가 메시지 전송
