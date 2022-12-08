---
title: "React의 성능 최적화, CSS 그리고 데이터 전송 방법에 대해"
excerpt: "React의 성능 최적화를 위한 useMemo, useCallback에 대해 학습하고,
다양한 CSS 처리 방법에 대해 학습하고,
SOA를 해결하기 위한 클라이언트-서버 데이터 전송 방법에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, react, CSS, CORS, Proxy, Fetch API]

toc: true
toc_sticky: true

date: 2022-12-08
last_modified_at: 2022-12-08
---

# React ToDo

## 화면 구성

### 패키지 설치

### 투두 메인 페이지

### 데이터 삽입 화면

### 데이터 목록 화면

### 각자 데이터 화면

### 통합

---

## 기능 구현

### 데이터 배열 출력

### 데이터 추가

- App.js 가 아닌 생산성 고려해서 해당 위치에 작성

### 데이터 삭제

- 기본 키로 삭제

### 데이터 수정

---

## 최적화

### 많은 양의 데이터 랜더링

- 함수 호출에 있어서 호출 구문을 작성할 경우 데이터 생성 시마다 리랜더링
  - 함수 이름을 대입해야 전부 수행하고 1번만 리랜더링

### 컴포넌트가 리랜더링 되는 경우

- 전달받은 props 가 변경되는 경우
- 자신의 state 가 변경되는 경우
- 상위 컴포넌트가 리랜더링 되는 경우
- forceUpdate 함수를 호출하는 경우

### 하나의 데이터가 수정되면 전체가 리랜더링 문제를 해결

- 과데이터일 경우 데이터 수정 발생 시 todos에 변경이 발생하고  
  todos 는 App 컴포넌트의 state 이기 때문에 App이 리랜더링이 되고  
  화면 전체가 리랜더링 되는 문제 발생

`자신의 props 가 변경될 때만 리랜더링`

- Class Component  
  shouldComponentUpdate 수명주기 메서드를 활용
- Function Component  
  Reac.memo 를 이용해 컴포넌트 감싸기

### 함수 업데이트 자체 방지

- useCallback으로 선언한 함수는 두 번째 매개변수 deps 변경에 따라 재생성
  - 해당 함수에서 deps 작성은 필수
  - 비즈니스 로직 상 todos 변경에 따른 함수 재생성 불필요

#### 1) useState 의 setter 에 함수형 업데이트 사용

- state를 수정하는 함수의 매개변수를 함수화

#### 2) useReducer 를 이용해 함수를 컴포넌트 외부에 생성

- state 를 수정하는 함수를 컴포넌트 외부로 내보내기
  - 변경할 state 와 action 을 매개변수로 받아서  
    action 의 type을 가지고 분기를 만들어 state에 작업
  - 컴포넌트 내부에서는 useReducer(함수이름, 초기값, 초기화하는 함수)
- 컴포넌트 내부에 state 수정 함수를 직접 생성하지 않기 때문에  
  state 가 변경되더라도 함수 재생성 X
  - 함수 만드는 작업이 그렇게 많은 자원,메모리를 소모하지 않음

### 화면에 보여질 내용만 랜더링

스마트폰 애플리케이션에서 CollectionView(Table View, Map View, Web View 등)은  
메모리 효율을 높이기 위해서 Deque 라는 자료구조로 화면에 보여지는 만큼만 메모리 할당해 출력  
스크롤을 하면 Deque의 메모리를 재사용하는 메커니즘으로 메모리 효율 증가

React도 외부 라이브러리를 통해 동일한 효과를 보여줄 수 있고  
이는 SPA 에서 굉장히 중요한 기술

- 이 방식이 어려울 경우 서버 랜더링으로 처리

- react-virtualized 라이브러리를 통해 지연 로딩 구현
  - 하나의 항목 너비 와 높이를 알아야 하고, 목록의 높이도 알아야 함
  - react 에서 목록은 스타일에서 width 와 height 설정

#### 패키지 설치

# 데이터 불변성

## 불변성

React 에서는 props 와 useState 로 만든 데이터는 원본 수정 불가

- Virtual Dom의 개념을 통해 랜더링을 구현
- 실제 DOM 과 Memory 상의 Virtual Dom 을 비교해 수정된 부분만 랜더링
  - 속도 향상 추구
  - 게임 물리 엔진과 같은 원리
- `비교 과정을 수행하기 때문에 원본에 대한 수정이 일어나선 안 됌`

  - js에서는 스칼라 데이터와 백터 데이터 사용의 원리가 다르다
    - 스칼라 데이터를 가진 변수끼리는 서로 다른 데이터
    - 백터 데이터일 경우 같은 데이터  
      `백터 데이터는 해당 정보가 참조하는 곳(Hashcode)를 가지고 있기 때문`

  ```javascript
  let a = 10;
  let b = a;
  // 서로 다른 데이터로 b를 변경해도 a가 변경되지 않음

  let a = { a: 10 };
  let b = a;
  // 같은 참조를 가르키기에 b를 변경해도 a도 변경됌
  ```

- 다만, React에서 주로 이용되는 Spread 연산자는 얕은 복사 방식
  - 얕은 복사  
    가장 바깥 쪽만 복제
  ```javascript
  let a = { a: 10, b: [1, 2, 3] };
  let b = a;
  b.b[0] = 3;
  // 객체 속 객체 속성이나 배열 내부를 변경하면 같이 변경됌
  ```
  - 깊은 복제  
    재귀적으로 복제해야함
  ```javascript
  let b = JSON.parse(JSON.stringify(a));
  // 데이터만 복제할 수 있는 야매
  // JSON은 함수가 없어 함수 포함될 시 불가능
  //함수도 포함할 시 immer 나 lodash 필요
  ```
