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

## 4. props

properties 의 약자

- 상위 컴포넌트(포함하고 있는 컴퍼넌트, 부모 컴포넌트)에서  
  하위 컴포넌트에게 데이터를 넘겨줄 때 사용하는 객체

### 넘겨 줄 때

```javascript
<컴포넌트이름 props이름 = 값..>
// 개수 제한 없음
```

### 사용

- 클래스형 컴포넌트  
  this.props.이름 으로 사용
- 함수형 컴포넌트  
  모든 props를 하나로 묶어 함수의 매개변수로 받아, 함수의 매개변수로 사용
