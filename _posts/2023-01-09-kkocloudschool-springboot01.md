---
title: "Spring Boot 을 활용한 Http 요청 과 View"
excerpt: "Spring Boot를 활용해 Http 요청과 Rest API 처리에 대해 학습하고,
View를 출력할 수 있는 방법들에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, spring boot, web programming, http request, rest api, view, template engine]

toc: true
toc_sticky: true

date: 2023-01-09
last_modified_at: 2023-01-09
---
# Spring Boot
- 사용할 IDE: Intelli J Ultimate - https://www.jetbrains.com/ko-kr/idea/download/#section=windows  
STS 를 이용해도 되고 Intelli J 다른 버전도 상관없음

## 1.Spring Boot
- `단독 실행`되는 `상용화 가능한 수준의 스프링 기반 애플리케이션`을 최소한의 설정으로 만들어 사용하기 위해서 등장

### 장점
- 환경 설정 최소화
- `WAS를 내장`해서 독립 실행이 가능한 스프링 애플리케이션 개발이 가능
- Spring Boot Starter 라는 의존성을 제공해서 기존의 Maven 이나 Gradle `설정을 간소화`
- XML 설정 없이 자바 수준의 설정 방식 제공
- JAR을 사용하여 자바 옵션만으로 배포 가능
- 애플리케이션의 모니터링 과 관리를 위한 Spring Actuator 제공

### 단점
- 버전 변경이 너무 자주 일어남
- 특정 설정을 별도로 하거나 설정 자체를 변경하고자 하는 경우 내부의 설정 코드를 확인 필요

## 2.프로젝트 생성
- STS  
Spring Starter Project로 생성

- Intelli J Ultimate Edition  
Spring Initializer 프로젝트 생성

- Intelli J Community Edition
   - `start.spring.io` 사이트에 접속해서 옵션을 설정한 후 Generate를 클릭  
   압축된 파일이 다운로드 됨
  - 압축을 해제한 후 Intelli J에서 그 디렉토리를 열면 됩니다.

## 3.프로젝트에 Controller 클래스를 만들어서 확인
### 클래스 만들기
- @RestController  
View 대신에 문자열(CSV) 나 JSON을 리턴하는 컨트롤러를 만들고자 할 때 사용하는 어노테이션
### 실행
### 브라우저에서 localhost:8080/hello 입력


## 4.포트 번호 변경
- Oracle 과 같이 사용하는 경우나 다른 프로젝트를 같이 실행해야 하는 경우 포트 충돌이 발생
- application.properties 파일에,  
`server.port=포트번호` 를 설정하면 포트 번호를 사용해서 실행

- 애플리케이션을 다시 실행하고 브라우저에 localhost/hello 를 입력해서 확인

- 설정 파일을 YAML로 변경 
  - 기존 설정 파일의 이름을 application.yml로 수정
- 서버 포트 설정 코드를 추가
  ```java
  server:
    port: 80
  ```

## 5.REST API
### REST(Representational State Transfer)
- `분산 하이퍼미디어 시스템 아키텍쳐`의 한 형식
- `자원에 이름`을 정하고(URL) URL에 명시된 `HTTP Method(GET, POST, PUT, DELETE)`를 통해서 `해당 자원의 상태`를 주고 받는 것
	- 동일한 자원에 대한 요청은 동일한 URL로 처리  
    Seamless 가능, 뷰를 만들지 말고 데이터를 전송
	- URL은 소문자로만 작성

### `REST API`
- REST 아키텍쳐를 따르는 시스템/애플리케이션 인터페이스
- REST 아키텍쳐를 따르는 서비스를 RESTful 하다라고 표현

### 특징
- 유니폼 인터페이스 
  - 일관된 인터페이스
- **무상태성**
   - 서버에 상태 정보를 따로 보관하거나 관리하지 않는다는 의미  
`세션이나 쿠키 사용을 하지 않음` 
  - 서버에 불필요한 정보를 저장하지 않음 
    - Web Token 이나 로컬 스토리지 사용으로 대체
- 캐시 가능성
- Layerd System  
**서버**는 네트워크 상의 여러 계층으로 구성될 수 있지만,  
**클라이언트**는 서버의 복잡도와 상관없이 End Point 만 알면 됨
- 클라이언트 
  - 서버 아키텍쳐  
  클라이언트 애플리케이션 과 서버 애플리케이션을 별도로  
  설계하고 구현해서 `서로에 대한 의존성을 낮추는 것`

### URL 설계 규칙
- URI의 마지막에 /를 포함하지 않음
- 언더바 대신에 -(하이픈) 를 사용
- URL에는 행위가 아닌 `결과를 포함` 
  - 행위는 HTTP 메서드로 표현
  ```java
  123 번 product를 삭제
  https://localhost.com/product/123 - DELETE 방식으로 요청
  https://localhost.com/delete_product/123 - 잘못된 설계
  ```
- URL은 소문자로 작성
- URL에 파일의 확장자를 표현하지 않음 
  - 파일의 확장자는 accept 헤더를 이용

## 6. GET API
### GET 방식
- 웹 애플리케이션 `서버에서 값을 가져올 때` 주로 이용
- URL 매핑  
  @RequestMapping(method=RequestMethod.GET, value="URL")
  @GetMappping("URL")

### 테스트
- 브라우저에서 테스트 가능
- API 요청 툴(POSTMAN 등)에서도 가능

### GET 요청을 처리하는 클래스를 만들어서 요청을 처리하는 메서드를 생성
### 실행 한 후 요청을 확인 
- 브라우저에 localhost/api/v1/rest-api/hello

### 요청을 처리하는 클래스를 만들어서 요청을 처리하는 메서드를 생성
 - 브라우저에 localhost/api/v1/rest-api/newhello

### URL에 포함된 파라미터 처리
- 파라미터가 1개 일 때는 파라미터를 URL에 포함시켜 전송 가능
- GET 이나 DELETE 인 경우
  - 요청 처리 메서드의 URL을 설정할 때 `파라미터로 사용된 부분`을 **{변수이름}** 으로 설정,  
   요청 처리 메서드의 `매개변수`로 **@PathVariable(변수이름) 자료형 이름** 을 추가
  - { } 안의 변수 이름 과 @PathVariable( ) 안의 변수 이름은 `일치`
  - 데이터의 형 변환은 자동으로 수행하는데 `자료형이 맞지 않으면 예외가 발생`
  - 파라미터를 대입하지 않으면 404에러

### Controller 클래스에 요청 처리 메서드를 추가
- 브라우저에 localhost/api/v1/rest-api/product/숫자 를 입력해서 확인

### 일반 파라미터 처리
- **HttpServletRequest** 이용해서 처리  
  ```java
  String getParameter(String 파라미터이름)
  String [] getParameterValues(String 파라미터이름)
  ```

- @RequestParam 어노테이션 이용
  ```java
  // 요청 처리 메서드의 매개변수
  @RequestParam(String 파라미터이름) 자료형 변수이름
  ```
- Command 객체 이용  
`파라미터 이름을 속성으로 갖는 클래스`를 만들고,  
 `클래스의 참조형 변수`를 요청 처리 메서드의 `매개변수로 대입`해서 처리

### GET 방식의 파라미터 설정
- URL 뒤에 ?를 추가하고 이름=값(&이름=값...)
- form을 만들고 method를 생략하거나 method에 get이라고 설정
- ajax 나 fetch api 에서 전송 방식을 GET으로 설정

### name, email, organization을 GET 방식으로 전송했을 때 처리
- Controller 클래스에 HttpServletRequest를 이용해서 처리하기 위한 메서드를 구현
- @RequestParam 으로 처리하는 메서드를 Controller 클래스에 생성
- Command 객체를 이용해 파라미터 처리시 파라미터 이름을 속성으로 갖는 DTO 클래스를 생성
  - .../param2?name=이름&email=이메일&organization=조직

### 파라미터 이름을 모르는 경우
- HttpServletRequest 나 @RequestParam을 이용해서 처리 가능
  - HttpServletRequest 의 경우  
  getParameterMap 메서드나 getParameterNames 메서드를 활용해서 처리가 가능
  - @RequestParam Map<String, String>param

## 7. POST API
- `리소스를 저장할 때 사용`하는 요청 방식
  - 리소스 나 값을 **HTTP Body**에 담아서 서버에 전달
- 파라미터 처리는 HttpServletRequest 나 @RequestParam 그리고 Command 객체를 이용 
  - **Command 객체**를 이용시,  
  처리할 때는 클래스 이름 앞에 **@RequestBody**를 추가해서,  
  `Http Body의 내용을 객체에 매핑`하겠다고 명시적으로 알려주는 것을 권장

### POST 방식에서의 파라미터 전송
- form 태그의 method를 post로 설정해서 폼의 데이터 전송
- ajax 나 fetch api에서 method 속성의 값을 POST 로 설정해서 전송

### POST 방식의 테스트
- 별도의 프로그램을 이용해서 테스트

### POST 방식의 요청 처리
- Controller 클래스에 요청 처리 메서드를 생성
- POSTMAN을 이용해서 테스트

## 8. PUT API
### 데이터를 수정할 때 사용하는 방식
- 유사한 역할을 하는 것으로 FETCH 도 존재
  - 사용 방법은 POST 와 유사

### 요청 방법
- ajax 나 fetch api에서는 method를 PUT으로 설정
- form 에서 method 속성은 GET 과 POST 만 설정 가능 
  - PUT으로 설정하면 GET으로 처리    
  form에서 처리하고자 하는 경우에는   
  form 안에  \<input type="hidden" name="_method" value="PUT" />  로 전송

### PUT 요청을 처리하는 메서드를 Controller 클래스에 추가
### 실행하고 이전 POST 요청에서 요청 방식만 PUT으로 변경해서 확인

### Controller 클래스에 요청 처리 메서드를 추가
  ```java
    @PutMapping("/param1")
    public ParamDTO getPutParam1(@RequestBody ParamDTO paramDTO){
        return paramDTO;
    }
    @PutMapping("/param2")
    public ResponseEntity<ParamDTO> getPutParam2(@RequestBody ParamDTO paramDTO){
        return ResponseEntity.status(HttpStatus.ACCEPTED)
                .body(paramDTO);
    }
  ```
- json 문자열로 만들어서 출력

## 9. DELETE API
### 데이터를 삭제할 때 사용
- 삭제를 할 때는 기본키값 하나만으로 삭제하는 경우가 많기 때문에 GET 방식과 동일한 방식으로 처리

### Controller 클래스에 DELETE 요청 처리 메서드를 생성

## 10. 로깅 라이브러리
### Logging
- 애플리케이션이 동작하는 동안 시스템의 상태 정보 나 동작 정보를 시간 순으로 기록하는 것
- Logging 은 개발 영역 중 **비기능 요구사항**(Common Concern) 에 속하지만  
 디버깅하거나 개발 이후 발생한 문제를 해결할 때 원인 분석에 꼭 필요한 요소
- 자바 진영에서 가장 많이 사용되는 로깅 라이브러리는 Logback

### Logback
- log4j 이후에 출시된 로깅 프레임워크로서 slf4j 를 기반으로 구현되었으면 log4j에 비해 성능 향상
- spring-boot-starter-web 에 내장
  - https://logback.qos.ch/manual/introduction.html
- 5개의 로그 레벨 설정 가능  
	- **ERROR**  
    `심각한 문제`가 발생해서 애플리케이션의 동작이 불가능
	- WARN  
    `시스템 에러의 원인`이 될 수 있는 경고 레벨
	- INFO  
    `상태 변경`과 같은 정보 전달
	- DEBUG  
    `디버깅할 때 메시지`를 출력
	- TRACE  
    DEBUG 보다 `자세한 메시지를 출력`하고자 할 때 사용

### Controller 에서 로깅을 수행
  ```java
    //로깅 가능한 객체를 생성
    private final Logger LOGGER =
            LoggerFactory.getLogger(JSONController.class);

    @RequestMapping(value="/hello", method= RequestMethod.GET)
    public String getHello(){
        LOGGER.info("Hello 요청이 왔습니다.");
        return "Get Hello";
    }
```

# Spring View
## 1. Java Web Application 에서 화면을 출력
### JSP 이용 
- 서버의 데이터를 출력하고자 하면 EL(내장) 과 JSTL(외부 라이브러리)을 학습
- **EL** 은 `표현식`으로 자바에서 `전달한 데이터를 출력`하는 문법  
\${데이터}
-  **JSTL**은 Apache 에서 제공하는 Custom Tag로,  
웹 프로그래밍에서 많이 사용하는 `자바 기능을 태그 형태`로 만들어 준 것

### Template Engine 이용
- 서버의 `데이터를 View로 출력`하기 위해서 만든 문법
- 일반적으로 확장자를 html을 사용
  - Thymeleaf, Velocity, FreeMaker, Mustache, Groovy 등

### 별도의 Front End Application 생성
- Web Application 이나 Mobile Application을 별도로 제작해서 Server의 데이터를 출력
- javascript 나 java, kotlin, swift 같은 언어로 제작
- javascript를 이용할 것이라면  
ajax, fetch api 또는 axios 라이브러리 필수
  - jquery는 별도의 ajax 관련 함수를 제공하지만  
  react, vue, angular는 `화면에 관련된 기능`만 제공
- 모바일의 경우는 **서버에서 데이터**를 받아오는 방법과 **스레드**를 미리 학습

## 2. 프로젝트 생성
- 의존성  
Spring Boot DevTools, Lombok, Spring Web, Thymeleaf 

## 3. JSP 출력
- spring boot 에서는 jsp 출력을 지원하지 않음

### build.gradle 파일에 의존성 추가
  ```java
  implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
  ```

### application.properties 파일을 application.yml로 변경하고 작성
  ```java
    spring:
        mvc:
            view:
            prefix: /WEB-INF/views/
            suffix: .jsp
    
    thymeleaf:
        prefix: classpath:/templates/
        suffix: .html
        cache: false
        view-names: thymeleaf/*
  ```

### Controller 클래스를 추가하고 요청을 처리하는 메서드를 생성}

### 뷰를 출력할 디렉토리를 생성
- src/main 디렉토리 안에 webapp 디렉토리 생성
- webapp 디렉토리 안에 WEB-INF 디렉토리 생성
- WEB-INF 디렉토리 안에 views 디렉토리를 생성

### views 디렉토리 안에 main.jsp 파일을 생성하고 작성
  ```java
    <%@ page language="java" 
            contentType="text/html; charset=UTF-8"
            pageEncoding="UTF-8" %>
    <!DOCTYPE html>
    <html>
    <head>
        <title>Spring Boot 에서 JSP 출력</title>
        <meta charset="UTF-8" />
    </head>
    <body>
        메시지:<%=request.getAttribute("message")%>
    </body>
    </html>
  ```

## 4. Thymeleaf
### 개요
- 화면 출력을 위한 `템플릿 엔진` 중 하나

### 장점
- 데이터 출력이 `JSP 의 EL 과 유사`하게 ${}를 이용
- Model에 담긴 데이터를 화면에서 JavaScript로 처리하기 편리
- `연산이나 포맷과 관련된 기능`을 추가적인 라이브러리 없이 지원
- html 파일로 바로 출력이 가능 
  - `서버 사이드 랜더링을 하지 않고 출력 가능`

### 도큐먼트
  -  https://www.thymeleaf.org

### thymeleaf 출력을 위해서 application.yml 파일을 수정
  ```java
    server:
    port: 80

    spring:
    thymeleaf:
        cache: false
  ```

### thymeleaf 기본 출력
- Controller 클래스에 요청 처리 메서드 작성
- thymeleaf 는 templates 디렉토리에 작성 
  - thymeleaf 디렉토리에 ex1.html 파일을 만들고 작성
  ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
    <h1 th:text="${'Hello Thymeleaf'}"></h1>
    </body>
    </html>
  ```

### 문법
- 속성이 아닌 곳에서 데이터 출력
  - [[${데이터}]]
- 반복문
  - th:each = "임시변수:${데이터 목록}"  
  반복문을 사용하면 state 객체가 생성되는데 `이 객체에 인덱스가 전달`

- 분기문
  - th:if ~ unless이용
  - th:switch 와 th:case 제공  
  - 삼항 연산자 사용가능하고 마지막 항은 생략 가능

- 데이터 출력을 위해서 Controller 의 기본 요청을 수정

- template 디렉토리에 main.html 파일을 생성하고 작성
  ```java
  ...
    <body>
        <table>
            <tr>
                <th>언어</th>
                <th>IDE</th>
                <th>빌드 도구</th>
            </tr>
            <tr>
                <td>[[${map.language}]]</td>
                <td>[[${map.ide}]]</td>
                <td>[[${map.buildtool}]]</td>
            </tr>
        </table>
        <table>
            <tr th:each="task:${list}">
                <td>[[${task}]]</td>
            </tr>
        </table>
    </body>
  ```

### DTO 의 List 출력
- DTO 클래스 생성 
- Controller 클래스에 요청 처리 메서드를 작성

- templates 디렉토리에 ex2.html 파일을 만들어서 데이터를 출력

### th:block
- 별도의 `태그 없이 출력`하고자 할 때 사용
- case 에 boolean 이 가능
  ```java
    <body>
    <ul>
        <li th:each = "vo, state:${list}">
        <th:block th:switch="${vo.sno % 2 == 0}">
            <span th:case=true th:text="${vo.sno}"></span>
            <span th:case=false th:text="${vo.last}"></span>
        </th:block>
        </li>
    </ul>
    </body>
  ```




