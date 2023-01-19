---
title: "Java Spring Boot의 Spring Security"
excerpt: "Spring Boot의 Spring Security에 대해 학습한다."

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot, spring security]

toc: true
toc_sticky: true

date: 2023-01-19
last_modified_at: 2023-01-19

---

# AOP

## 1. **AOP**(Aspect Oriented Programming)

### 관점 지향 프로그래밍

- `객체 지향 프로그래밍을 보완`하는 개념으로,  
  `메서드나 클래스를 관점에 따라 분리`시켜 구현하는 프로그래밍 방법

- MVC Pattern 의 경우,
  - 데이터 다루는 Repository 와 Service, View, Controller 로 구분
    - Repository 와 Service 를 **Model** 로 설정
  - **Service** 부분은,  
     `업무 로직`을 처리하기 위한 부분 / 처리하기 위해 필요한 부분 +  
     또는 `업무와 상관없지만` 필요한 코드 로 분리해 프로그래밍 하는 것
    - Business Logic  
      실제 업무에 관련된 로직
    - Cross Coutting Concern  
      나머지의 내용, 공통 관심 사항

### Java 에서의 AOP

- 나눠서 치리하되, Java Web Programming에서는  
  `직접 인스턴스를 생성 메서드를 호출하지 않고`,
  - 코드 작성 후 설정시 `컴파일 / 빌드 / 실행 시` 코드를 조합  
    이는 **미들웨어** 작업과 같음

## 2. AOP 구현 방법

### `Filter`

- `Java EE 의 Spec`
- Spring과는 상관이 없어서 `Filter를 사용`하면 `Spring의 Bean 조작 안됨`
- 인코딩 이나 XSS(Cross-Site Scripting) 방어에 주로 이용

### `Spring Interceptor`

- `URI 요청 및 응답 시점`을 가로채 전/후 처리를 수행
- `Controller 가 처리`하기 전이나 후의 작업 작성

### `Spring AOP`

- `메서드 호출 전 후` 처리를 수행
- `Business Logic` 수행 전, 후 처리  
  `Service` 에서 사용

## 3. **HandlerInterceptor**

- `인터셉터를 구현`하기 위한 인터페이스

### 메서드

- **preHandle**  
  Controller 가 처리하기 전에 호출되는 메서드
  - true 리턴 시, Controller로 이동  
    false 리턴 시, Controller 로 이동하지 않음
- **afterCompletion**  
  Controller 가 처리한 후에 무조건 호출되는 메서드

### 설정

- WebMvcConfigure 인터페이스를 구현한 클래스의 **addInterceptors** 재정의

## 4. 실습

### 인터셉터 클래스 생성

### 프로젝트에 Web Project 설정을 위한 클래스 추가

- 인터셉터 적용

### Controller 클래스 생성 후 요청 처리 메서드 작성

- 설정한 url에서 요청 생성 후 콘솔 확인시 확인 가능

## 5. **AOP**

### AOP 필요성

- 웹 애플리케이션 개발시, 여러 계층으로 나누어서 구현
- 여러 계층에 비즈니스 로직과 관련이 없는 `공통 로직` 필요,  
  이를 `각 계층에 구현할 시 코드의 유지보수가 어려워짐`

### AOP 장점

- 비즈니스 모듈에는 `주요 관심사에 대한 로직`만 존재
- 비즈니스 모듈을 `수정하지 않고도 추가 동작 작성` 가능

### 구현 방식

- `Proxy 패턴을 이용`해서 구현
- **외부**에서 `비즈니스 로직을 가진 객체`(Target)을 호출하면,  
  이 객체를 `감싸는 외부 객체`(Proxy)를 호출해 `Target에 전달`
- `Proxy 는 구현될 때 Target을 상속`받아 생성되어,  
  `Target 과 동일한 방식으로 호출` 가능

  ```java
  class Target{

  }
  class Proxy extends Target{

  }
  Target target = new Proxy();
  ```

### AOP 용어

- **Advice**  
  `공통 기능의 코드`로 `로그 출력이나 트랜잭션 관리` 등
  - `언제 적용할 것` 인지를 설정해 사용  
    Before, After, Around, After Returning, AfterThrowind, Introduction
- **JoinPoints**  
  `advice가 적용 가능한 지점`
  - 메서드 호출 전/후 와 속성 호출 전/후 가능
  - 스프링은 `메서드 쪽만 가능`
- **PointCut**  
  `advice 와 joinpoints 를 결합`하기 위한 설정
  - `정규 표현식이나 패턴` 이용,  
    스프링에서는 apspectj pointcut 표현 언어 사용
- **Weaving**  
  `Advice 와 Target 을 결합하는 시점`,  
  **컴파일시** /클래스 **로딩** 시 /**런타임** 시 3가지 존재
- **Target**  
  `Advice 적용되는 객체`
- **Aspect**  
  `공통 관심 사항 과 이를 적용하는 코드 상의 포인트를 모은 것`,  
  Advice 와 PointCut 의 결합체

### 적용

- build.gradle에 의존성 추가
- SpringBootApplication 에 어노테이션 추가
  - @EnableAspectAutoProxy(proxyTargetClass = true);
- Domain 클래스 생성
- Service 인터페이스 생성

# Spring Security

## 1. 인증 과 인가

### **Authentication**(인증)

- `작업을 수행할 수 있는 주체`인지 확인
  - 로그인 처리 등

### **Authorization**(인가)

- `권한을 확인`하는 작업

## 2. Spring에서의 Security

- 직접 구현도 가능하나, Security 관련 패키지 제공
  - SPring Security 와 OAuth2

## 3. 실습

- 의존성  
  Spring Boot DevTools, Lombok, Spring Web, Thymeleaf, Spring Security, OAuth2Client, Spring Data JPA, MariaDB
- .yml 파일 수정 및 Jpa 어너테이션 추가
- user 라는 계정의 비밀번호가 생성되는데,  
  `아무런 설정이 없으면 모든 경우에 로그인 필요하다 간주`
  - login으로 `리다이렉트 수행`  
    어떤 요청도 리다이렉트 되기 때문에 user 와 비밀번호 입력으로 진행

## 4. Spring Boot 설정 클래스

- spring boot 이전의 프로젝트에서는 web.xml파일에 설정,  
  boot에서는 `설정에 관련된 클래스를 제공`해서,  
  설정 관련 클래스를 `extends/implements 한 클래스 만들어` 메서드 생성

- Security 설정은 **WebSecurityConfigurerAdapter** 클래스 상속받아 설정

### Controller 와 View 생성해 확인

- 권한별 페이지 생성해 출력

### SpringBoot 설정 클래스 추가하고 filterChain 메서드 재정의

- 재실행시 로그인 과정 없이 요청을 처리
- 설정 클래스에 메서드 추가해 정적 파일에서는 시큐리티 미동작 설정

## 5. Spring Security Customizing

### Password Encoder

- 암호화해서 `복호화가 불가능한 암호화`를 수행해주는 클래스
- 복호화는 불가능하지만 `비교는 가능`
  - **BCryptPasswordEncoder** 사용  
    빈으로 설정 가능

### **UserDetailsService**

- 일번적인 로그인 처리는 ID 와 Pw로 데이터 조회해,  
  올바른 데이터가 있을 경우 `세션이나 쿠키로 처리`
- Security에서는 회원 정보를 User, 아이디를 username이라 칭함

  - 아이디로 데이터 조회 후 비밀번호를 비교하는 형식
  - 비밀번호 틀리면 `Bad Cridential` 결과  
    로그인 성공시 자원 접근 권한 확인해, `Access Denied` 결과 생성

- **loadUserByUsername** 메서드만 소유

  - username을 가지고 User 정보를 찾아오는 메서드
  - 리턴 타입은 UserDetails로,  
    Authorities, Password, Username, 계정 만료, 계정 잠김 등 알아냄
  - `DTO 클래스에 UserDetails 구현`하거나, 별도 DTO 생성해서 구현

- **formLogin**( )  
  인가 나 인증 절차에서 문제가 발생시 로그인 페이지 보여주는 메서드
  - 연달아 loginPage("/로그인 URL") 호출해 로그인 URL 설정
  - loginProcessingUrl("/로그인처리 URL") 호출해 설정 가능

### 로그인 처리

- SecurityFilterChain 에 formLogin 호출 코드 추가
- 로그인 관련 로즉 클래스 생성 후 오버라이딩

### 인가 설정

- 어노테이션으로 권한 설정시 설정 관련 클래스에 @**EnableGlobalMethodSecurity** 추가,  
  Controller에서 @**PreAuthorize** 어노테이션 이용해 설정
- CustionSecurityConfig 에 어노테이션 추가

### 인증 설정 방식

- **formLogin**  
  `인증이 필요한 경우 로그인 폼`을 출력하도록 해주는 설정
- **loginPage**  
  `로그인 페이지 URL 직접 설정`
- **defaultSuccessUrl**  
  `로그인 성공시 리다이렉트할 URL` 설정
- **usernameParameter**  
  `username에 대한 파라미터 이름` 설정
- **failureUrl**  
  `로그인 실패했을 때 리다이렉트할 URL` 설정
  - 기본은 login

### 로그인 화면 직접 생성

- CustomSecurityConfig 클래스 메서드 수정

### **CSRF**(Cross Site Request Forgery)

- `크로스 사이트 요청 위조`
- 사용자의 등급 변경 `URI를 알고 이때 필요한 파라미터`를 안다면,  
  `img 태그나 form 태그`를 이용해 `URI와 파라미터를 기록`해 둔 상태에서,  
  관리자가 이 링크 클릭시 공격자가 관리자 등급으로 변경

- 방어 방법
  - **referrer**를 체크
  - **PUT / DELETE** 방식 사용
  - **CSRF 토큰** 활용

### logout

- logout 해도 되고, JSESSIONID로 시작하는 쿠키 삭제
