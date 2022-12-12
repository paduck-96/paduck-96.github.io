---
title: "React redux와 middleware, 가상 서버 제작"
excerpt: "React에서 redux를 활용한 프로젝트와 컨테이너를 학습하고,
middleware를 활용해 적용하고,
가상 서버로 서버를 대신해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, react, redux, container, middleware, virtual server]

toc: true
toc_sticky: true

date: 2022-12-12
last_modified_at: 2022-12-12
---

## 8. redux

### 프로젝트 생성 및 필요한 라이브러리 설치

- yarn add redux react-redux

### ui 작업

- 저장 후 실행의 유용성

### todos 작업

### 화면 배치 및 props 전달

### custom redux module 설정

- redux 모듈에는 **패턴**이 있음

  1. 액션(행동 이름) 타입 정의
  2. 액션 생성 함수(발생하는 일) 정의
  3. 초기값 설정
  4. state 와 action 으로 구성된 redux 생성

- 각 모듈에 따라 생성하고 하나의 파일에서 이를 통합
  - combineReducers( ) 함수 사용

### redux 적용

- `store 변수를 생성하고 이를 Provider를 통해 전달`해주는 동일한 패턴
  - 최상위 index 폴더에 적용
  - createStore 함수 import 시 이름 변경 필요

### 컨테이너 컴포넌트 작업

**`컨테이너 컴포넌트`**

- redux를 사용하는 컴포넌트

컨테이너 컴포넌트 생성

- react-redux 의 **connect( ) 사용**

```javascript
connect(mapStateToProps, mapDispatchToProps)(연동할 컴포넌트)
//mapStateToProps 는 스토어 안의 상태를 컴포넌트 props로 전달
//mapDispatchToProps 는 액션 생성 함수를 컴포넌트 props로 전달
```

## MiddleWare

### 개요

액션의 `디스패치 후` 리듀서에서 해당 `액션(실 동작) 수행 전 / 후에 작업` 진행

- `작업을 수행하기 전`에는 **유효성 검사** 같은 작업 주로 진행
  - 정규 표현식을 통한 패턴 검증이 주
- `작업 수행된 이후`에는 **로그 기록**을 많이 진행
  - 대부분 프로그램에서 진행(라이브러리 존재 확률)
- Filter, Interceptor, AOP 등의 이름으로도 불림

### 미들웨어 적용

- 최상위 파일에서 **applyMiddleware** 불러오기

### 외부 라이브러리를 이용한 로그 기록

- yarn add redux-logger
- 결국은 **로그 처리**를 어떻게 할 것이냐가 중요

## 가상의 API Server 만들기

- yarn add json-server
  - 가상의 더미 데이터를 만들고, 그걸 기반으로 서버 실행
- json-server ./data.json --port 포트번호
