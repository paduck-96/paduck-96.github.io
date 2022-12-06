---
title: "React Component, props, state, Event handler 와 ref"
excerpt: "React component에 대해 학습하고
props 와 state에 대해 학습하고
Event handler 와 ref에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, react]

toc: true
toc_sticky: true

date: 2022-12-05
last_modified_at: 2022-12-06
---

## 3. Component

화면을 구성하는 단위

- View(하나의 화면)  
  \> Component  
  (Segment - Control을 1개 이상 모아서 만든 논리적인 화면 구성 단위)  
  \> Control(작은 부품 하나)

### 1)만드는 방법

- 클래스로 구성  
  Componet 클래스로 상속 받아 render 함수에 출력 내용 리턴
- 멤버 변수 사용이나 수명 주기 메서드를 사용하는 것이 편리

  ```javascript
  class 이름 extends Component{
  render(){ return 출력할 내용}
  }
  ```

- 함수로 구성  
  출력할 내용을 리턴, 클래스로 만드는 것보다는 가볍고 속도가 빠름

  > 최근에는 함수로 구성하는 경우가 많음

  ```javascript
  function 이름(){
  return (출력할 내용)
  }

  const 이름 = () => {
  return (출력할 내용)
  }
  ```

### 2) 확장자

- js
- jsx  
  HTML에 사용되는 자바스크립트 와 구분
- tsx  
  타입스크립트 문법을 사용한다것을 명시

### 3) 만들 때 주의사항

- 컴포넌트는 Root 가 1개  
  div 나 span 또는 <> 태그로 묶어서 표현

- react 에서는 데이터 원본을 직접 수정 X
  - 다른 곳에서 넘겨준 데이터는 복제해 수정 후 다시 대입

## 4. props

properties 의 약자

- **상위 컴포넌트**(포함하고 있는 컴퍼넌트, 부모 컴포넌트)에서  
  **하위 컴포넌트**에게 `데이터를 넘겨줄 때` 사용하는 객체

### 넘겨 줄 때

```javascript
// 상위 컴포넌트에다가
<컴포넌트이름 props이름 = 값..>
// 개수 제한 없음
```

### 사용

- 클래스형 컴포넌트  
  **this.props.이름** 으로 사용
- 함수형 컴포넌트  
  모든 props를 하나로 묶어 함수의 매개변수로 받아, **함수의 매개변수**로 사용

  ```javascript
  // 함수형 컴포넌트
  // App.js 파일에서
  ...<컴포넌트 이름 name="">
  // name에 작성한 값이 props 변수로 삽입
  ```

  - 기본 값 설정

  ```javascript
  컴포넌트이름.defaultProps = {props이름:값...}
  // 기본 값 설정
  ```

  - 태그 안에 내용 사용

  ```javascript
  // 컴포넌트 파일에서
  {
    props.children;
  }
  ```

### 비구조화 할당

자바스크립트는 객체를 분해해서 할당하는 것이 가능

- 분해 할당 시 인덱스가 아닌 이름으로 데이터 할당

### props 의 유효성 검사 기능

필수 나 자료형 설정해 검사 가능

- `화면 출력에는 영향이 없고`, 스크립트 오류 발생해 검사 창에서만 확인
- `문자열이 아니면` **{ }** 로 감싸야 함
  - 기본적으로 XML에서 태그 안 속성은 문자열만 가능

## 5. State

props 는 상위 컴포넌트에서 하위 컴포넌트에게 데이터 넘겨주는 것

- 하위 컴포넌트의 props 는 **읽기 전용**으로 `수정 불가`

### 개요

Component 내부에서 `읽고 쓸 수 있는 값을 사용`하고자 할 때 사용

- 기본값 설정 가능
  - 클래스형 컴포넌트의 경우 초기값은 **constructor(props)** 메서드
  ```javascript
  this.state = {state 이름 : 값, ...}
  ```
- setter 메서드를 이용해 수정 가능
  - setState 메서드 이용
  ```javascript
  this.setState({state이름:값, ...})
  ```

### state 함수 전달

state 도 하나의 `속성`이고, JS 함수도 하나의 `데이터`로  
`state에 함수 전달 가능`

setState에 2번째 매개변수로 함수를 전달하면  
state 설정 변경 후 호출되는 함수 설정

> 한 곳에서만 사용하는 데이터가 있는 경우 로컬 데이터를 업데이트하고  
> 업데이트가 종료되면 서버에 업로드하는 형태일 때 주로 이용

### useState

함수형 컴포넌트에서 state 를 사용하기 위한 Hook

- `배열을 리턴`
  - 첫 번째 데이터는 **현재 상태**
  - 두 번째 데이터는 **setter**  
    `상태를 변경해주는 함수`
    - set상태이름의 형태
    - 상태이름의 첫 글자는 대문자
- 상태의 초기값을 매개변수로 대입

### state 사용시 주의 사항

`클래스형 컴포넌트`에서는 setState  
`함수형 컴포넌트`에서는 usetState 함수 이용 필수

`객체 나 배열 수정 시 `**복사본**`을 만들고 수정 필요`

```javascript
const copyObject = { ...object, name: 군계 };
//object.name="" 지양

let copyAr = ar.concat(4); // ar.push 지양

배열에서 데이터 삭, 변형시 filter 나 map 함수 이용
> 작업 수행 후 새로운 배열 생성
```

## 6. Event Handling

### 주의 사항

이벤트 이름은 camelCase

- 시작은 소문자, 두 번째 단어부터 시작이 대문자

자바 스크립트 코드를 단순하게 작성하는 것이 아닌 `함수 형태로 전달`

`DOM 요소에만 이벤트를 설정`

- Component 요소에는 **설정 불가**

이벤트 종류

- https://reactjs.org/docs/events.html

---

### onChange

입력 내용이 변경될 때 호출되는 이벤트

---

### 이벤트 처리 코드를 별도 함수화

이벤트 처리 코드를 `별도의 함수`로 만들거나

- **모듈화**

`상위 컴포넌트에서 받아오기`

- **여러 컴포넌트에서 공통으로 사용**하는 데이터 조작

---

### 바벨 transform-class-properties 문법 사용

`클래스 안`에 일반 메서드 생성 시, `인스턴스의 메서드로 자동 변형`

- 클래스 안에 함수 만든 경우 this에 다시 바인딩 불필요

---

### EventRouting

하나의 함수로 여러 DOM 의 이벤트 처리

이벤트 처리 함수의 매개변수로 Event 객체 전달  
이벤트 처리 함수 `매개변수의 target 속성`을 호출 시  
`이벤트 발생한 객체`의 참조 리턴

- 분기문을 만들어 이벤트 처리 코드 작성하면 하나의 함수로 여러 DOM 이벤트 처리 가능
  - name 이나 id 속성 등을 적절히 이용

---

### onKeyPress

키보드를 누를 때 발생하는 이벤트

- 키보드 이벤트에서 이벤트 객체의 key 라는 속성에 누른 키 값 전달
  - `문자열 형태`

### 함수형 컴포넌트로 수정

## ref

HTML 에서는 DOM 요소에 어떤 작업을 수행할 때 id 부여 후  
document.getElementBy.. 로 DOM 객체 찾은 후 수행

대신 **ref** 라는 개념을 가지고 DOM 을 찾아 핸들링

### ref 설정 방법

- 콜백 함수

  ```javascript
  <태그 ref={(ref)=>{this.? = ref}} />
  ```

- createRef() 함수

  ```javascript
  이름 = React.createRef();

  <태그 ref={this.이름}>
  ```

### Component 에 Ref 설정

`컴포넌트 내`의 `변수 나 메서드에 접근`이 가능

`다른 컴포넌트`에서 `Ref 속성을 이용해서 조작`이 가능

- 컴포넌트를 다른 곳에서 조작하고자 할 때 활용
