---
title: "Java Spring을 활용한 파일 출력과 유효성 검사"
excerpt: "Java Spring을 활용해 파일로 데이터를 출력하는 방법에 대해 학습하고,
Spring에서 제공하는 유효성 검증을 살펴본다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, csv, json, pdf, validation]

toc: true
toc_sticky: true

date: 2023-01-06
last_modified_at: 2023-01-06
---

### 상세보기

- 메인 목록에 링크를 만들어 상세보기 수행
- 링크에서 GET 방식 요청시, 기본키 값을 전달
  - 링크에서 `파라미터 형태`로 전달 / `URL에 전달`
- Repository 에 상세보기를 위한 DB 메서드 작성
- Controller 클래스에 상세보기 요청 처리
- detail.jsp 출력

### 경로에 파라미터 넘기기

- 파라미터 값 하나 받는 형식으로 변경
- controller 클래스의 상세보기 처리 메서드 수정
- `PathVariable 사용시 경로가 하나 더 붙어` 수정 필요

## 13. jsp 이외의 출력

### Java Web Programming에서 데이터 출력

- **html**  
  서버의 데이터를 직접 받아서 출력하는 것은 어렵고,  
  `통신을 이용해서 데이터`를 받아서 출력
  - ajax, fetch api, websocket / web push
- **jsp**  
  `el 과 jstl을 이용`해서 서버에서 전달한 데이터 출력
- **template engine**  
  `별도의 라이브러리를 추가`해서 서버의 데이터 출력
- 데이터 출력  
  csv, xml, json 등
- `별도의 View` 를 만들어서 출력  
  파일 다운로드, pdf, excel 등

### Excel 출력

- `apache poi` 라는 라이브러리 이용  
  pom.xml 파일에 의존성 추가
- jsp 파일에 출력 경로 설정하는데, 확장자 설정 권장
  ```java
  <a href="item.xls"></a>
  ```
- View 클래스 생성
  - **AbstractXlsView** 클래스로부터 상속을 받아서  
    buildExcelDocument 라는 메서드를 overriding해서 출력
- Controller에서 뷰 이름 리턴시 View 클래스로 출력할 수 있도록  
  servlet-context.xml 파일에 수정이 필요
- Controller 클래스 요청 처리 메서드 추가

### pdf 출력

- 한글 출력 시 `한글 폰트가 존재`해야 함
- iText API 라이브러리를 pom.xml 추가
- AbstractPdfView 클래스 상속

### json 출력

- Controller 클래스의 요청 처리 메서드 리턴 타입을 ResponseEntity,  
  Content-type을 json 으로 설정해서 값을 리턴
- jackson-databind 라는 라이브러리를 이용해,  
  Controller 클래스의 리턴 타입을 일반 자바 객체나 List로 설정하고,  
  앞에 @ResponseBody를 추가
- JSONObject 나 JSONArray를 데이터로 넘겨준 후 MappingJacksonJsonView로 출력
- `RestController 생성해서 리턴`

### 출력 방법

- pom.xml 파일에 json 출력 위한 라이브러리 의존성 설정
- controller에 처리 메서드 추가
- servlet-context.xml 파일에 뷰 결정 코드 추가

### 출력 방법2

- @**JSONController** 사용
- POJO 클래스 위에 @JSONController 추가하면  
  문자열이나 JSON 출력 Controller 생성
- 요청 처리 메서드에서 문자열 리턴시 문자열 출력  
  DTO나 List 를 리턴하면 JSON 형태의 문자열 생성

## 14. 스프링의 예외처리

### Controller가 처리하는 도중 예외가 발생하는 경우

- 예외의 내용을 브라우저에 출력
- 사용자는 예외의 내용을 읽어보아도 의미 파악이 어려움
  - 사용자에게 예외 내용 직접 출력은 무의미
  - `사용자가 이해하기 쉬운 예외 페이지 전송`

### 예외 발생

- exception 요청을 GET으로 전송했을 때 출력할 파일 생성
- 정상적 요청이 아닐 경우 예외 발생해 tomcat이 소유하고 있는 예외 출력

### 예외 처리

- 일반 자바 웹 프로젝트에서는 web.xml 파일에 \<error-page> 이용,  
  `예외 발생하면 보여질 페이지` 설정
- 스프링에서는 @**ExceptionHandler** 와 @**ControllerAdvice** 이용해 처리
  - @ExceptionHandler  
    Controller 클래스 안에 작성
  ```
  @ExceptionHandler(예외 종류)
  public String 메서드이름(){
    return "예외 출력 뷰의 이름";
  }
  ```
  예외 종류에 해당하는 `예외 발생시 리턴하는 문자열`을 가지고  
  `ViewResolver 와 결합`해 예외 발생시 페이지 출력
- `요청 처리 메서드에 Exception 타입의 매개변수 설정` 가능
- `Controller 클래스 1개에서만 발생하는 예외처리`
- @ControllerAdvice("패키지이름")을 추가하면 패키지 내에서 예외 발생시  
  클래스 안에 있는 @ExceptionHandler이 붙은 메서드 호출

## 15. Spring MVC 설정

### 설정을 이용한 뷰 결정

- 요청이 왔는데 처리할 내용이 없고, `단순히 뷰 이름만 리턴`하는 경우
  ```java
  <view-controller path="요청경로" view-name="뷰이름" />
  // 설정 파일에 설정
  ```

### 뷰의 디폴트 설정

- DispatcherServlet 만들 때 url을 /\* or / 을 사용하면  
  거의 모든 확장자에 대한 처리를 Controller에 해야 함
- html, css, js 등의 파일을 `생성자에서 처리`해야 함
  - **default-servlet-handler** 태그를 추가해  
    생성자에서 처리 못하는 요소는 `Tomcat 에서 처리`

## 16. Spring 의 Custom

- Spring에서 독자적으로 제공하는 태그  
  `유효성 검사`에 편리
  ```java
  <%@ taglib prifix="시작할태그요소 - spgin", uri="태그 위치" &/>
  ```

## 17. MessageSource

### 지역화 개념

- 파일명\_언어코드.properties 로 선언된 파일을 읽어서 출력

### <spring_message>

- 문자열을 가지고 와서 출력하는 태그

### 메시지 출력

- label.properties 파일 생성 후 작성
- servlet-context.xml 에 메시지 출력 디렉토리 설정
- COntroller 클래스에 message 요청 처리 메서드 작성

### spring form 태그

- spring form을 만들어서 `유효성 검사를 쉽게 하기 위해 태그 지원`
- 태그 라이브러리 설정
  ```java
  <%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
  ```
- 폼 생성  
  \<form:form>
  - method 를 `설정하지 않으면 post`  
    id 속성에 command 객체의 기본값이 설정  
    spring 의 form 태그로 form 을 만들면 `CSRF 공격 방어`
- **CSRF**(Cross Site Request Forgery) 공격
  - 사용자가 자신의 의지와는 무관하게 `공격자가 의도한  
삽입, 수정, 삭제 등의 행위를 요청`하게 만드는 것
  - form을 전송할 때 form의 URL 을 가로채 사용자 데이터 입력시  
    form에 대신 입력해서 처리하는 공격
  - 방어방법
    - referrer 확인  
      `어디에서 요청`이 이루어졌는지 확인
    - Security Token 사용  
      사용자 `세션에 임의 난수 저장`후 요청마다 난수 같이 전송
      - spring form 태그는 해당 부분 자동 설정
- form:input, form:password, form:hidden: path에 아이디 설정
  ```java
  <form:input path="userid" /> //입력시
  <input id="userid" name="userid" value="${?.userid}" type="text">
  ```
- form:select
  - items 속성에 List 타입 속성을 대입하면 자동으로 옵션으로 변경

### 폼

- DTO 생성 후 컨트롤 메서드 수정
- Controller 클래스에 뷰에 무조건 데이터 넘겨주는 메서드 생성

### 유효성 검사

- `데이터를 검증`
- 유효성 검사 위치
  - **클라이언트**  
    JavaScript 나 HTML5 속성 이용  
    검사 결과가 빠르게 전성되나 `보안이 취약`
    - 소스 코드 노출
  - **서버**  
    서버 언어로 검사를 수행하는데 권한 있는 사용자의 데이터인지
    - CSRF 공격 방어
      데이터의 유효성을 검사해 통과 못하면 클라이언트로 돌려보냄  
       응답 속도 느리나 코드 노출되지 않아 `보안 우수`

`실제로는 양쪽 모두 수행이 좋음`

### 스프링의 유효성 검사

- `서버 측 유효성 검사`
- 데이터 검증에 사용되는 **Validator** 인터페이스 와  
  유효성 검사 결과를 저장할 **Errors** 와  
  **BindingResult** 인터페이스를 제공
- Validator 인터페이스
  - supports(Class <?> clazz)  
    유효성 검사를 수행할 DTO 클래스 지정
  - validate(Object target, Errors errors)  
    target이 유효성 검사 수행할 객체, errors는 실패시 에러 여부 저장

### 유효성 검사

- Validator 인터페이스 가져오기
- 오버라이딩 후 Controller 에 추가
- 유효성 검사 실패시 출력될 메시지를 작성하기 위해 .properties 추가

### 메시지 국제화

- 기존 .properties 파일 이름 변경

### 고정된 문자열

- 되도록이면 properties 파일에 작성하고 불러들이는 것이 효율적
