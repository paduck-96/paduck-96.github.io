---
title: "React의 특징과 생성 및 실행"
excerpt: "React의 특징에 대해 학습하고
어떻게 프로젝트를 생성하고 react를 실행하는지 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, react]

toc: true
toc_sticky: true

date: 2022-12-02
last_modified_at: 2022-12-05
---

# React

## react

유저 인터페이스를 만드는데 사용할 수 있는 자바스크립트 라이브러리

`SPA(Single Page Application) 구현을 위해 주로 사용`

- angular.js, vue.js 등

**Component**

- **특정 부분의 모양**을 결정하는 선언체
  - HTML을 재생성해서 보여주는 템플릿 엔진과 다르게 많은 기능 내장
- **Virtual DOM**을 이용해 `출력 속도가 빠름`

  - DOM 은 자바스크립트 내장 객체보다 처리 속도가 느림
  - `Virtual DOM`으로 출력 내용 메모리에 만든 후 `변경 시 적용`  
     이후, 실제 DOM 과 `비교 후 변경되는 부분만 수정`해서 출력
    > 게임 화면 출력 방식과 유사

- 오로지 **View**를 위한 **라이브러리**
  - MVC(Model View Controller)  
    MVVM(Model View ViewModel)  
    MVW(Model View Whatever)  
    MVP(Model View Presentation) 등의 구조 **프레임워크**와 차이
  - ajax 처리를 위한 axios  
    데이터 가져오기 위한 Fetch API 라이브러리  
    Redux 라이브러리 등의 나머지 기능 구현을 위한 학습 필요

## 개발 환경 설정

- 1. node
- 2. npm
- 3. yarn

  - 페이스북이 만든 npm을 개선한 패키지 관리자

  ```javascript
  npm install yarn
  npm install -g yarn || npm install --location=global yarn
  ```

- 4. webpack  
     프로젝트에 사용된 파일 분석해서 `웹 문서 파일로 변환`해주는 도구

  - js, css, html 확장자 `이외의 파일을 브라우저가 해석하게 변환`

- 5. babel  
     ES6(ECMA 2015) 버전의 JS 문법까지 적용  
     그 이상 버전의 `문법을 ES6 이하로 바꾸어` 이해 가능하게 도와줌
     - Trans Compiler

- 6. IDE

  - VSC 와 같은 IDE 필요
  - 부가적인 확장 프로그램

- 7. 형상 관리 도구
  - Git 과 같은
- 8. 디버깅 위한 도구

  - React Developer Tools 등

- 9. react 프로젝트 앱 설치
  ```javascript
  yarn global add create-react-app
  ```

## React Application 생성 및 실행

### 1. 생성

```javascript
// powershell
Set-ExecutionPolicy RemoteSigned // 권한 후
yarn create react-app 애플리케이션 이름
npx create react-app 애플리케이션 이름
```

- 기본 구조 자동 생성
- entry point 는 App.js

### 2. 실행

```javascript
yarn start
npm start
```

- App.js를 읽어 실행하고 브라우저 자동 오픈
- 기본 포트 3000

- 실행시 모든 파일을 읽어 번들러로 JS 파일을 만듦

  - webpack 과 babel 이때 실행

- 파일 수정 시 바로 적용

## JSX

JavaScript XML 의 약자로 `JS 에 XML을 추가한 확장형 문법`

- 브라우저 실행 시 Babel 이 JS 코드로 변환

### 장점

- 보기 쉽고 익숙
- 코드 작성 오류 발생시 Babel 이 변환 과정에서 이를 감지
- `HTML 태그 와 Component 혼용`하여 개발 가능

### 규칙

- 주석

```javascript
{/* 주석 */}
'//' 이나 '/*' '*/'도 가능하지만
/> 는 다음 줄에 나와야 함
```

- 반드시 `하나의 부모 요소`로 시작

  - `root 요소 하나`

- 태그는 반드시 닫아야 함
- `JS 내용 출력시 { } 안에 표현`

- if 는 출력이 불가능하지만 **삼항 연산자**는 가능

- **스타일**을 적용할 때는 `객체 형식`으로 설정

  - 문자열로 설정하지 않음
  - camel case로 작성

- class 속성 대신에 **className** 이라는 속성 이용

## 개발 플로그인

### 1. ESlint

코드 작성 도중 에러 확인

### 2. Prettier

들여쓰기  
문자열 상수의 작은 따옴표 나 큰 따옴표  
; 삽입 등 자동

## **Component**

### 개념

`화면을 구성하는 재사용 가능한 모듈`

- View 는 전체 화면을 대부분 의미
  - 재사용성 떨어짐
- `component 는 전체 화면의 일부분`
  - js 확장자에서 jsx, tsx 확장자 사용

### 생성 방식

클래스형 컴포넌트 >> 함수형 컴포넌트

### 클래스 형 컴포넌트

- 생성
  ```javascript
  class 컴포넌트이름 extends Component{
    render(){
        출력 내용 리턴
    }
  }
  ```
- `Life Cycle 이용 가능`  
  함수는 호출하면 안의 내용을 수항하고 호출한 곳으로 리턴

  - 인스턴스는 한 번 만들어지면 **소멸시키기 전**까지 존재
  - 생성 시 생성자와 같은 메서드 호출되어  
    **수명주기에 따른 작업** 수월

- `내부 매서드 구현` 가능
  - CBD 개발은 별도 파일에 컴포넌트 구성 후 통합 파일로 import
