---
title: "React Life Cycle 과 Hooks"
excerpt: "React의 라이프 사이클에 대해 학습하고,
다양한 React Hooks에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, react, hook, life-cycle]

toc: true
toc_sticky: true

date: 2022-12-06
last_modified_at: 2022-12-06
---

# React Hooks

## 1. 컴포넌트 반복

동일한 모양의 컴포넌트를 여러 개 배치

- 배열 형태의 데이터 출력 시 유용하게 사용

### **배열.map( )**

배열의 데이터 순회하면서 매개변수로 받은 함수를 각각 실행 후 결과 배열로 리턴

- callback 함수
  - 3개까지 작성 가능
    - 순회하는 요소 > 인덱스 > 배열 그 자체
  - 반드시 `하나의 데이터 리턴`
- callback 함수 내부에서 사용할 this 참조

`props 나 state 는 직접 수정이 불가능해 흔히 말하는 반복문 사용 어려움`

### 배열.filter( )

- 매개변수 1개
  - boolean을 리턴하는 함수를 매개변수로 대입
- 함수 결과가 true인 데이터만 모아서 다시 배열로 리턴

### 배열을 이용한 컴포넌트 반복

- Component 배열을 랜더링 할 때 `변동점을 찾기 위해` **key 값** 설정 필요

  - 빠르게 배열을 순회하기 위해
  - **index**를 키로 주로 설정

  ##### props, state, ref

  > props 상위 컴포넌트가 하위 컴포넌트에게 데이터 넘겨줄 때  
  > state 컴포넌트 내에서 생성해서 수정 과 삭제가 가능한 객체로  
  >  변경 시 리랜더링  
  > ref 컴포넌트 조작 시 사용

---

# Life Cycle

`인스턴스가 생성되고 소멸될 때 까지의 과정`

생성은 Constructor 가 담당하고, 소멸은 Destructor 가 담당

## Ioc(Inversion of Control) - 제어의 역전

클래스 개발은 개발자의 몫
인스턴스의 생성/소멸은 프레임워크 나 컨테이너가 관리

- IoC가 적용된 경우 일반적으로 `생성자로 인스턴스 생성하지 않음`
  - 수명 주기 관련 메서드를 이용해 생성 과 소멸 시 작업 수행
- **React 컴포넌트**가 IoC가 적용되는 객체
  - Spring Bean, 안드로이드 Component 등

### React Component 수명 주기

- Mount  
  컴포넌트가 메모리 할당을 받아서 `출력`
- Update  
  컴포넌트 정보를 업데이트 하는 것으로 `리랜더링`
- Unmount  
  컴포넌트가 화면에서 `제거`

### Mount 메서드

- constructor  
  생성자로 가장 먼저 호출되는데 `state `**초기화**`를 수행`
- getDerivedStateFromProps  
  `props 에 있는 값을 state에 동기화` 할 때 호출
- render  
  랜더링 할 때 호출되어 **this.props** 와 **this.state**로 `접근 가능`
- componentDidMount  
  화면에 보여지고 호출되어, `비동기 작업(네트워크, 타이머 작업) 수행`
  - `랜더링 완료 후 진행해야 하는 개념`으로 splash screen 같은 것

### Update 메서드

- getDerivedStateFromProps  
  props 에 있는 값을 state에 동기화 할 때 호출
- shoudComponentUpdate  
  리랜더링 결정 메서드로 `false 리턴 시 리랜더링 하지 않음`
- render
- getSnapshotBeforeUpdate  
  변경된 내용을 `DOM에 적용하기 직전`에 호출되어, `화면에는 미반영`
  - **Snapshot**으로 git의 브랜치와 같이 변경내역만
- componentDidUpdate  
  업데이트 되고 나면 호출

### Unmount 메서드

- componentWillUnmount  
  `메모리에서 사라지기 전`에 호출되어 **memory leak** 발생할 객체 제거
  - 타이머 등 무한 반복 객체

### 라이프 사이클 이용 시 주의할 점

- React 개발 모드가 **React.StrictMode** 면 개발 환경시 Mount 2번씩 진행
  - 첫 번째는 에러 여부 찾고, 두 번째는 실 랜더링

## 에러 출력

### 없는 프로퍼티 출력

### 에러 출력 시 호출되는 메서드 재정의

- 에러 메서드 작성된 컴포넌트로 감싸주면 `문법 외 오류 확인 가능`
- `서버로부터 데이터 받아올 때 주로 사용`

---

## Hooks

### Hook

React 16.8 버전에 추가

Class Component 의 기능을 Function Component에서 사용하게 해주는 기능

## useState

`state를 함수 컴포넌트 안에서 사용`할 수 있도록 해주는 hook

- 초기값  
  useState의 매개변수
- 리턴 데이터  
  state 와 state 의 값을 변경할 수 있는 setter 함수의 배열

---

## useRef

### ref

`컴포넌트를 조작`하기 위해 붙이는 `일종의 id 와 같은 변수`

useRef 로 만들어진 변수는 값이 변경되도 컴포넌트가 리랜더링 **X**

- useRef(초기값)으로 변수 생성  
  컴포넌트 나 DOM 설정 시 ref 속성에 대입

#### Input 포커스 설정으로 학습

---

## useEffect

`state 가 변경된 후 수행할 side effect`를 설정하는 Hook

Class 의 Life-Cycle 중에서 componentDidMount 와 componentDidUpdate, ComponentWillUnmount 가 합쳐진 형태

#### 클래스의 수명주기 메서드

- 생성자 와 마운트가 2번씩 반복되는데 개발 환경 strict mode 때문

#### useEffect 로 함수형 컴포넌트

### **useEffct**

```javascript
useEffect(()=>{수행할 내용},  deps 배열)

```

`deps 배열을 생략`하면 Mount 된 경우 +  
모든 state 가 변경될 때 마다 호출  
`deps 배열을 비우면`(...,[ ]) Mount된 후에만 호출  
`desp 배열에 state 를 설정`하면 state 값의 변경될 때도 호출

- 수행할 내용에서 `함수를 리턴`하면 **Clean-up 함수**가 됌
  - Unmount 될 때 호출되는 함수

#### 객체 배열을 함수형 컴포넌트로 출력하고 CRUD

- 각 함수형으로 선언하고 .map으로 하나씩 출력
- root js 파일에 데이터가 있기 때문에 이벤트 생성 후 props로 하위 전달
  - 삭제의 경우 대부분 기본키를 매개변수로 받아 삭제
    - 최상위 컴포넌트에서 하위로 전달, 전달하는 형식으로 생성
  -

### useEffect 활용

## useMemo

연산된 값을 재사용하는 Hook  
**성능 최적화**를 위해서 사용

#### 매개변수로 (연산 수행 함수, 배열)

- `배열에 변화가 생긴 경우`에만 연산 수행  
  그렇지 않은 경우에는 함수를 호출해도 `결과만 제공`
