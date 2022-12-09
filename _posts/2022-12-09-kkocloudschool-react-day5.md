---
title: "React 불변성 유지 와 데이터 공유"
excerpt: "React 불변성을 유지하는 라이브러리에 대해 학습하고,
데이터를 공유할 수 있는 라이브러리에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, react, Immer, Data]

toc: true
toc_sticky: true

date: 2022-12-09
last_modified_at: 2022-12-09
---

# 데이터 불변성

## 불변성

React 에서는 `props 와 useState 로 만든 데이터는 원본 수정 불가`

- **Virtual Dom**의 개념을 통해 랜더링을 구현
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

- 다만, React에서 주로 이용되는 Spread 연산자는 **얕은 복사 방식**
  - **얕은 복사**  
    가장 바깥 쪽만 복제
  ```javascript
  let a = { a: 10, b: [1, 2, 3] };
  let b = a;
  b.b[0] = 3;
  // 객체 속 객체 속성이나 배열 내부를 변경하면 같이 변경됌
  ```
  - **깊은 복제**  
    재귀적으로 복제해야함
  ```javascript
  let b = JSON.parse(JSON.stringify(a));
  // 데이터만 복제할 수 있는 야매
  // JSON은 함수가 없어 함수 포함될 시 불가능
  //함수도 포함할 시 immer 나 lodash 필요
  ```

## 불변성 라이브러리 immer

불변성을 신경쓰지 않으면서 업데이트 할 수 있는 라이브러리

- **procude** 라는 함수를 이용해 업데이트 시 객체 복제해 작업 후 원본에 적용
  ```javascript
  produce(state, (draft) => {
    draft.number += 1;
  });
  // produce 함수의 state를 draft를 통해 적용하면
  // 깊은 복사로 자동 처리
  ```

# `운영 모드`

## 배포

```javascript
yarn build
yarn global add serve
serve -s build
```

# React Router

## Routing

`요청 URL 에 따라 분기`를 해서 출력하는 것

## SPA

### Server Rendering

웹 브라우저가 서버에게 요청을 전송하면 서버가 HTML을 전송해 전체 재출력

- 사용자와 인터렉션이 많은 웹 앱에서는 `속도 측면의 문제` 발생

### SPA

첫번째 요청을 전송했을 때만 HTML이 전송  
이후의 요청에는 서버가 JSON 형태(|| XML) 데이터 전송하고 `브라우저가 데이터 파싱 후 랜더링`

- 앱 규모가 커지면 js 파일의 사이즈가 커져 트래픽 과 로딩 속도에 문자 발생
  - 적절한 Code Splitting을 이용해 해결
- 브라우저에서 js 코드 관리시 크롤러가 페이지 정보 제대로 받아가지 못함
  - 검색 엔진에서 페이지의 정보를 검색 결과에 미포함
- js 실행될 때까지 `페이지가 비어 있어 빈 페이지 보여줄 수 있음`
  - 첫 번째 페이지는 서버에서 랜더링하고 다음부터 클라이언트 랜더링
- `적절한 라우팅으로 분산`
  - react-router-dom

```javascript
<Router>
  <Route path="경로" element={<추가 컴포넌트 />} />
</Router>
```

## 링크

`Link 라는 컴포넌트` 이용

- 페이지를 새로 불러오지 않고 `그대로 유지한 상태`에서 HTML5 History API를 사용해서 페이지의 URL 갱신
  - a 태그로 만들어져 있는 `페이지 전환 기능 방지`

```javascript
<Link to=URL>내용</Link>
```

## URL Parameter & Query String

### URL Parameter

URL 의 마지막이나 중간에 데이터를 전송하는 것

- /:id 같은 부분

```javascript
/URL명/:변수명
```

### Query String

URL 뒤에 ? 를 추가하고 이름 과 값을 전달하는 것

- Client 가 Server 에게 전달하는 데이터

  - JS 문법을 활용해 만들고
  - 반드시 `인코딩 필수`

- **useLocation** 이라는 Hook 을 이용해서 **Location 객체** 리턴
  - pathname  
    query string 을 제외한 경로
  - search  
    ? 를 포함한 query string
  - hash  
    #문자열 형태의 값(segment)
    - 하나의 페이지 내에서 이동
    - 구버전 브라우저에서 클라이언트 라우팅 할 때 사용
  - state  
    페이지 이동 시 임의로 넣을 수 있는 상태 값
  ```javascript
  react-router-dom 의 useSearchParams 라는 Hook
  qs 라이브러리 의 query string 객체화
  ```

## `Sub Routing`

Router 내부에 다시 Router 를 만드는 것

- 공통된 URL을 작성해주어야 함

## 공통 레이아웃

### 1) 공통된 레이아웃을 위한 컴포넌트를 만들고 각 페이지 컴포넌트에서 출력

### 2) 중첩된 라우트와 Outlet 을 이용해 구현

- 한 번만 설정하면 됌

## 라우팅 에서의 index 속성

index 라는 props 가 존재하는데 이 props 는 "/"

## Router 의 부가 기능

### useNavigate

Link 컴포넌트 대신 `다른 페이지로 이동`하고자 할 때 사용하는 Hook

- `Redirect 구현`할 때 주로 사용

```javascript
// useNaviagte 은 리턴 값을 가지고 있음
useNavigate(정수 || 문자열, 옵션);
// 첫 번째 매개변수가 숫자면 숫자만큼 이동
//문자열이면 이동할 URL

// 두 번째 매개변수로 객체 생성해 replace:true 를 설정하면
// 현재 페이지에 대한 기록을 남기지 않음
```

### NavLink

Link와 거의 유사한데 `현재 경로와 Link의 경로가 일치하면 특정 스타일 적용`

### Navigate

화면에 보여지는 순간 `다른 페이지로 이동하고자 할 때 사용`하는 컴포넌트

- 로그인 안되면 로그인 페이지로 이동

### 404 에러(URL 오류)

Route 를 만들 때 path "\*" 로 설정하면 모든 경우에 반응  
이를 `Route의 맨 아래 추가하면 앞에 일치하는 path 없을 시 동작`

# `Context`

## react 프로젝트에서의 데이터 공유

Compoenent 의 props 를 이용해서 하위 컴포넌트에게 전달

- 구조가 간단할 경우 어려움이 별로 없음
- 복잡해지면 번거로운 작업이 많아짐
- 여러 곳에서 하나의 데이터 사용하는 경우도 유사 작업 반복

`Context API 를 활용해 공유 데이터 작성`

- .Consumer  
  저장한 context 가져오기
- .Provider  
  수정 기능

# Redux

## redux

`상태 관리 라이브러리`

- node 관련된 프로젝트 어디서든 사용 가능
- Context API 나 useReducer 보다 이전에 등장

`상태 관련 로직을 별도의 파일로 분리`해서 관리 가능

- 데이터 공유가 수월해짐

`프로젝트의 규모가 크거나 비동기 작업을 주로 할 때 사용`

## 키워드

### 1. **Action**

`상태에 어떠한 변화가 필요하게 될 때 발생`시키는 객체

- type이 필수이고 나머지는 옵션

```javascript
{
    type:"타입이름",
    data:{id:1, name:"adam"} // 단일 데이터 전달도 가능
},{}...
```

### 2. Action Creater

`Action을 생성`하는 함수

- 필수는 아님
- 직접 Action 객체 생성하거나 별도 함수로 만들어 사용

```javascript
export function addToDo(data) {
  return {
    type: "",
    data,
  };
}
```

### 3. Reducer

`상태 변화를 일으키는 함수`

```javascript
function 이름(state, action){
    return 변경된 state
}
...
function counter(state, action){
    switch(action.type){
        case "":
            return state+1
            break;
        ...
    }
}
```

### 4. Store

`redux를 사용시 하나의 store 생성`

- 애플리케이션의 **state** 와 **reducer**, 몇 개의 **내장 함수** 제공
  - dispatch  
    실제 액션을 발생시키는 함수
    - action 객체 대입시 dispatch 가 reducer 를 호출해 함수 실행 하 상태 변경
  - subscribe
    - 함수 형태의 파라미터를 받아 action이 dispatch 될 때 호출

## 규칙

### 1. `하나의 애플리케이션에는 하나의 스토어`

### 2. `state 는 읽기 전용`

### 3. `reducer 는 pure function` 이어야 함

- 외부에서 넘겨받은 매개변수 수정하지 않고 `복제해서 수정 후 return`
- `동일한 입력이면 동일한 출력`
  - 랜덤 / new Date / 네트워크 다운 등은 별도 미들웨어로 처리

### 4. 패키지 설치

yarn add redux

## Redux Module

리덕스 모듈은 액션 타입, 액션 생성 함수, 리듀서를 포함하는 JS 파일

- 리덕스 샘플에서는 액션 과 리듀서를 분리해서 정의
- 실 개발 환경에서는 `액션 과 리듀서를 하나의 파일로 통합`
  - **Ducks 패턴**
  - 예제 기준 모듈화  
    counter 변화 부분과 text 변화 부분 배열 변화 부분 각각 파일화  
    하나의 파일(index.js)에서 모두 combine 후 export 형태
- 액션을 구분하기 위해서 액션 이름을 만들 때 이름 앞에 접두어 사용

### 컴포넌트의 분류 필요

- Presentational Component  
  리덕스 스토어에 직접 접근하지 않고 필요 값이나 함수 넘겨받아 사용

  - UI 선언에 집중

- Container Component  
  리덕스 스토어에 직접 접근해서 상태를 조회하거나 액션 디스패치
  - HTML 태그 사용하지 않고 Component 가져와 사용
