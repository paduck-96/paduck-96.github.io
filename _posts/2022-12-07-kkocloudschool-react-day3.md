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

date: 2022-12-07
last_modified_at: 2022-12-07
---

## useCallback

### `특정 함수`를 새로 만들지 않고 재사용하고자 할 때 사용

컴포넌트에 구현한 함수들은 컴포넌트가 랜더링 될 때마다 다시 생성  
양이 많아질수록 비효율성 증가

- `데이터가 변경된 경우에만 함수 재생성 가능`

```javascript
useCallback(함수, 데이터의 배열)
```

- 컴포넌트 내부 함수에만 적용 가능
- **useMemo** 는 '부르는 개념' **useCallback** 은 '함수 생성 개념'

## React.memo

컴포넌트의 props 가 변경되지 않았다면 리랜더링을 방지

- 리랜더링의 성능 최적화 함수
- 함수로 컴포넌트를 감싸기만 하면 리랜더링이 필요할 때만 적용

# React Styling

## Styling 방식

- **일반 CSS**
- **Sass**  
  CSS 전처리기(pre-processor) 를 이용해 확장된 CSS 문법 사용
- **CSS Module**  
  스타일 작성시 CSS 클래스 이름들이 충돌되지 않도록 파일마다 고유 이름 자동 생성해주는 옵션
- **styled-components**  
  컴포넌트 안에 스타일을 내장시켜 동일한 스타일의 컴포넌트 가능
  - 실 제작시 주료 이용

## 2. 일반 CSS

webpack 의 css-loader를 이용해 일반 CSS 불러오는 방식

- App.js 가 App.css 를 붙여와서 적용

### Naming

컴포넌트-클래스이름 의 형태로 네이밍

- App-header

BEM(Block Element Modifier) 방법

- CSS 에서 가장 많이 사용
- 각각의 요소는 - 나 \_ 로 구분
- 블럭이름\_요소이름\_수정지이름 형태로 네이밍

SMACSS(Scalable and Modular Architecture for CSS)

- 확장형 모듈식 구조
- Base 에는 접두어 X, Layout 관련되면 l- 등으로 이름으로 용도 파악 가능

OOCSS 등이 있음

## 3. CSS Module 사용

CSS 를 불러와서 사용시 클래스 이름을 고유한 값으로 적용

[파일이름]\_[클래스이름]\_[해시값] 을 추가해 클래스 이름 부여

- css 파일의 `확장자를 .module.css` 로 작성
- css 파일의 `클래스 이름은 일반적`이게 작성
- :global 를 클래스 이름 앞에 작성시 별도의 클래스 이름 부여 불가

## 4. classnames 라이브러리

CSS 클래스를 `조건부로 설정`할 때 유용하여 여러 클래스 설정이 편리

```javascript
yarn add classnames
//
classNames("one","two") // 2개 설정
classNames("one", ["two", "three"]) // 3개 설정
classNames("one", {"two":true}) // true 조건으로 two 적용
```

- true 자리에 변수를 설정하면 조건부 설정 구현도 가능

## 5. Sass

### CSS Preprocessor(전처리기)

CSS 가 동작하기 전에 사용하는 기능

- CSS의 불편함을 해결하기 위한 확장 기능
- 문법 자체는 유사하나 `선택자의 중첩, 조건문, 반복문, 단위 연산` 등 가능
- Sass, Less, Stylus 등이 있음

### SASS

- Syntactically Awesome Style Sheets

중복되는 코드를 줄여서 가독성 좋게 작성 가능

Sass 가이드 라인 :: https://sass-guidelin.es/ko/

**SCSS**  
CSS 구문과 호환되게 새 구문을 도입해 만든 SASS 기능 지원하는 CSS super set

```javascript
yarn add node-scss scss-loader sass
```

- scss 확장자 사용

### 변수 와 믹스-인 사용

```javascript
$변수명: 값;
// 변수를 만들어 다른 곳에서 @import 해서 사용

@mixin 이름( ){
    속성:값,
    ...
} // 여러 속성을 모아놓은 것

@include 이름( ); // 다른 파일에서 적용 시 해당 속성 모두 사용 가능
// mixin import 느낌
```

`자주 사용하는 속성`이 있을 때 유용

> 여러 컴포넌트 만들다보면 공통적으로 사용하는 scss 라이브러리가 있는데  
> 한 곳에서 import 할 경우 다른 곳에서는 별도 import 없이 사용 가능

### SCSS 라이브러리

SCSS 가 이미 적용된 라이브러리

- 반응형 웹 디자인
  - include-media
- 색상 모음집
  - open-color
- 아이콘
  - react-icons

### Material Design

웹 과 앱을 통틀어 `모든 개발 플랫폼에서 사용자 경험을 하나로` 묶기 위해

- Progressive Web Design

구글에 제시한 디자인 방식

#### CDN 방식

Content Delivery(Distribution) Newtwork  
서버 와 사용자 사이의 물리적 거리를 줄여 `콘텐츠 로딩 속도 최소화`

- `동일한 콘텐츠를 여러 네트워크에 분산` 저장
- 사용자 요청 시 `가장 가까운 네트워크`에서 다운로드

이는, 스타일시트의 **외부 링크** 이용하는 것

- **네트워크 미연결** 시 적용되지 않음

#### scss / css 파일

다운받아 적용시키면 네트워크 사용 여부와는 무관해지지만 앱 크기가 커짐

## 6. styled-components

### CSS in JS

`자바스크립트 파일 안에 스타일을 선언`하는 방식

- 컴포넌트 와 디자인을 분리하지 않고 `하나의 파일`로 만들어서 사용  
  컴포넌트 배치하면 자동으로 디자인 적용
- react-icons 의 각 icon 컴포넌트를 삽입하면 이미 설정된 style 적용
  - https://github.com/MicheleBertoli/css-in-js

### 패키지 설치

## 7. 서버에서 데이터 받아오기

### 1) ajax

load 이벤트 와 error 이벤트 처리

### 2) **`Fetch API`**

Promise를 이용하는 API

### 3) axios 라이브러리

### **`서버 설정`**

#### node server 에서 **CORS** 설정

- cors 라이브러리 설치 후 서버 실행 파일에 추가

#### **Proxy** 설정

- package.json 파일에 설정 추가

  ```javascript
  "proxy":"서버의 도메인"
  ```

  요청은... /api/도메인 뒤의 url

- 라이브러리 이용

  ```javascript
  yarn add http-proxy-middleware

  // 설치 후 src 디렉토리에 setupProxy.js 작성
  const {createProxyMiddleware} = require("http-proxy-middleware");
  module.exports = (app)=>{
    app.use(createProxyMiddleware("/클라이언트공통URL",{
        target:"서버URL",
        changeOrigin:true
    }))
  }
  ```
