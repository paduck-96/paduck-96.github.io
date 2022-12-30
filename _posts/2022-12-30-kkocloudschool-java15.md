---
title: "Java 쿠키 와 세션, 이를 활용한 로그인"
excerpt: "Java에서의 쿠키와 세션에 대해 학습하고,
로그인 과정에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, Cookie, Session, Login]

toc: true
toc_sticky: true

date: 2022-12-30
last_modified_at: 2022-12-30
---

# Cookie & Session & Filter

## 1. Cookie & Session

- http 나 https 는 `상태가 없음`
- 클라이언트가 서버에 requset 를 전송하고 response 를 받으면 연결 종료
  - 이전에 무슨 작업을 했는지 알 수 없음
- 응답 전송 이후 데이터 저장해 `상태 유지하기 위해 저장소의 개념` 필요
- **Cookie**  
  `클라이언트의 웹 브라우저에 저장` 후 `동일한 도메인에서 요청 전송` 시  
  매번 서버에게 전송되는 데이터
- **Session**  
  `Cookie 에 발급되는 키`를 가지고 `서버에 저장`해서 사용하는 데이터

## 2. Cookie

- 웹 서버 와 클라이언트가 정보를 주고받는 방법 중 하나
- `클라이언트에 파일의 형태로 존재`하기 때문에 임의 조작 가능

### 동작 방식

- Cookie 생성 >  
  웹 서버에서 생성하는 것이 일반적, JS로도 생성 가능
- Cookie 저장 >
- Cookie 전송

### 구성

- **name**  
  구별하기 위해 사용하는 이름
  - `중복되면 쿠키의 내용 변경`
- **value**  
  `저장되는 값으로 문자열`만 가능
- **유효시간**
- **도메인**  
  `사용 가능한 도메인`으로 기본은 자신의 도메인
- **경로**  
  `도메인 전체`가 되는데 일부분으로 한정 가능

### java에서 쿠키 생성

```java
// 생성
new Cookie(name, value)
// 저장
response.addCookie(Cookie 객체)
//value는 반드시 인코딩 된 상태로 저장
```

### Cookie 클래스 메서드

- 구성을 get 하고 set 하는 메서드 제공
- 쿠키 삭제 메서드는 별도 없고 `유효시간을 과거로 되돌리는 방식` 활용

### 자바에서 모든 쿠키 가져오기

```java
Cookie [] request.getCookie()
```

## 3. 실습

### Maven 프로젝트 생성

- 이클립스는 Dynamic Web Project 생성 후 변환 필요

### Servlet 과 jsp 에 대한 의존성 설

- 이클립스 환경에서만 진행
- maven 의존성은 repositories(저장소 위치) 와  
  dependencies(실제 다운받을 라이브러리)에 진행

## 4. 쿠키 생성 및 읽기

### 쿠키 생성 파일 추가

### 쿠키 삭제

## 5. Web Storage

### **web storage**

- 브라우저에 저장 가능한 HTML5 API

### 쿠키 와 차이

- `크키 제한 없음`
- `데이터를 서버로 보내지 않음`
- `자바스크립트 객체를 저장`할 수 있음

### 종류

- **LocalStorage**  
  삭제하지 않는 이상 소멸되지 않음
- **SessionStorage**  
  세션과 수명을 같이하는 스토리지

### 속성 과 메서드

- length
- key(index)
- getItem(key)
- setItem(key, data)
- removeItem(key)
- clear( )

### 아이디를 로컬에 저장했다가 다음 접속시 출력

- 쿠키 미사용으로 html이나 jsp 상관 없음

## 6. **Session**

- 접속한 브라우저에 대한 정보를 `웹 컨테이너에 저장`하는 객체  
  웹 컨테이너는 `하나의 웹 브라우저에 하나의 Session 할당`

### 세션 사용

```java
//jsp
<%@ page session="true" %> //작성, 생략 시 session 생성
//false로 설정시 만들어지지 않음
```

- **Spring Legacy Project**로 MVC Project 생성시 index.jsp에  
  `세션 미사용 옵션이 자동 설정`

- Servlet  
  `request 객체`를 가지고 **getSession**( )를 호출하면  
  없을 경우 생성하고 있으면 있는 세션을 리턴

### 세션에 데이터 활용

- void setAttribute(String name, Object value)
- Object getAttribute(String name)
- void removeAttribute(String name)

### 메서드

- String getId( )  
  id 리턴
- long getCreationTime( )  
  세션이 만들어지는 시간 리턴
- long getLastAccessedTime( )  
  마지막 사용 시간
- void setMaxInactiveInterval(int seconds)  
  세션을 사용하지 않았을 때 유지하는 시간으로 초 단위
- long setMaxInactiveInterval( )  
  자동 로그아웃 등
- invalidate( )  
  세션 초기화

- 유효 시간 설정은 web.xml 파일에서도 가능하나 분 단위
  ```java
  <session-config>
    <session-timeout>시간</session-timeout>
  </session-config>
  ```

### 세션 사용

- `redirect 하더라도 정보를 유지`하기 위해서 사용하는데  
  대표적인 경우가 `로그인 정보`를 한 번 로그인으르 하면  
  브라우저 종류할 때까지, 일정시간 동안 조작하지 않으면 로그아웃
- 접속자가 아주 많을 때 세션 사용할 경우 서버 속도 감소
  - `세션 저장 정보를 DB 나 파일에 처리하기도 함`

## 7. Filter

- `Controller 가 요청 처리 전이나 후`에 동작하는 Java EE 의 객체
- spring 에서는 `Interceptor 또는 AOP`라고 부름
- Filter 인터페이스의 메서드를 구현한 클래스를 생성, URL 패턴 등록>  
  패턴에 해당하는 요청이 왔을 때 동작

### Filter 인터페이스

- **doFilter**  
  요청이 왔을 때 호출되는 메서드이고 3개의 매개변수 포함
  - request, response, chain

```java
chain.doFilter(request, response) // 원래 해야하는 동작 수행
```

- 해당 메서드 앞에서 Controller 가 처리하기 전에 수행 내용 작성  
  해당 메서드 뒤에서 Controller 가 처리 후에 수행할 동작 설정

  - `요청 처리 전`  
    유효성 검사, 로그 기록
  - `요청 처리 후`  
    로그 기록, 데이터 변환 작업

- init
- destroy

### 사용

- 전체 요청에 대해서 인코딩 설정 할 때 많이 사용

## 로컬 로그인 구현하기

### 로그인 정보 생성

### Project 생성 후 설정

## 3. Database Layer 작업

### `tbl_member 테이블에 대한 VO 클래스` 생성

### `DB 연동 코드를 가진 DAO 클래스` 생성

## 4. `ServiceLayer` 작업

### Service 와 Controller 그리고 View 사이의 데이터 전달

- `DTO 클래스 생성`

### 회원 요청을 처리할 메서드를 소유하는 `Service` 인터페이스

- 이와 더불어 메서드 구현하는 Impl 클래스 생성

## 5. `Controller Layer`

- 아이디 와 비밀번호를 입력 받아서 로그인을 처리

### login 요청 처리할 servlet 생성

### logout 요청 처리할 servlet 생성

### webapp 디렉토리에 member 디렉토리 생성해 login 페이지 구현

### 톰캣 버전으로 오류가 발생할 수 있음

- servers 탭에서 server 삭제 후 다른 버전 가용

## 6. 자동 로그인

- 로그인 할 때 자동 로그인 체크 여부를 확인해서 자동 로그인을 체크  
  `쿠키에 유일 무일한 값을 배정`하고 이 값을 `DB에도 기록` 한 후  
  다음 로그인 시도시 쿠키 값을 확인해 `쿠키의 값으로 DB에서 로그인`

### DB 회원 테이블 수정

- 유일 무이한 값을 저장할 수 있는 컬럼 추가

### MemberVO 와 MemberDTO 클래스에도 변경된 속성 추가

### 자동로그인 여부 확인

### MemberDAO 메서드에 관련 내용 추가

- uuid로 로그인 / uuid를 업데이트

### MemberService 인터페이스와 클래스 수정

### IndexCOntroller 생성

### 모든 요청에 반응하는 filter 생성
