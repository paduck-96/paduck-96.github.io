---
title: "Java Spring DB"
excerpt: "Java Spring DB에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, Database]

toc: true
toc_sticky: true

date: 2023-01-04
last_modified_at: 2023-01-04
---

### bean 자동 생성

```java
<context : component-scan base-package="패키지이름"> // 설정 파일

// 패키지 안 클래스에 특정 어노테이션 추가시 bean 자동 생성
```

- @Component, @Service, @Repository  
  인스턴스 생성
- @Controller  
  Servlet
- @RestController  
  뷰 대신에 데이터 전송에 사용  
  `뷰를 만들 수는 없음`
- Boot는 기본 패키지 이름을 설정하면 자동으로 component-scan 추가

### RepositoryImpl, ServiceImpl, Controller 클래스에 어노테이션 추가

### applicationContext.xml 파일 설정 추가

## 12. 수명 주기(Life Cycle)

### 어노테이션을 이용한 설정

- 메서드 위에 @PostConstruct를 추가하면 `생성자 다음`에 호출
- 메서드 위에 @PreDestroy를 추가하면 `인스턴스 소멸 직전`에 호출

### InitializingBean 과 DisposableBean 인터페이스 구현하면 유사 역할

### bean 태그의 init-method, destroy-method 속성 이름 등록하면 유사

## 13. **Test**

- 테스트 클래스 위에 @RunWith(SpringJUnit4ClassRunner.class) 추가,
  @ContextConfiguration(설정파일경로) 이용하면 설종 파일의 bean으로  
  테스트 가능

# Spring Database 연동

## 1. 데이터베이스 연동

- Java JDBC 코드 이용
- Spring JDBC 코드 이용
- SQL Mapper 이용(MyBatis)
- ORM 이용(Hibernate)

## 2. Spring JDBC ----

### 스프링의 데이터베이스 지원

- `템플릿 클래스를 이용`해서 데이터를 접근
- 의미있는 예외 클래스로 예외를 던짐
  - `JDBC 는 DB 예외를 모두 SQLException`으로 던져  
    구체적인 파악 어려움
- `트랜잭션 처리가 간단`
- 다른 데이터베이스 프리임워크와 연동을 지원

### pom.xml 기본 설정 수정

- test scope 제거하면 오류 덜 발생
- 데이터베이스 dependency 추가

### DB 접속 테스트

- test 폴더에 테스트를 위한 클래스 생성

### DAO 패턴

- 데이터베이스 연관 로직은 별도의 클래스를 만들어서 작성
- **POJO 형태**로 작성하는 걸 권장
  - `외부 프레임워크의 클래스를 상속하지 않는`
- 직접 인스턴스 생성을 하지 않고 `IoC 와 DI를 이용해 사용` 권장
- 예외를 외부에 던지는 것을 권장하지 않음

### DataSource

- Spring 에서는 DB 사용 시 `DataSource 사용 강제`
  - DB 연결은 `별도의 Bean`을 활용
  - http://commons.apache.org/proper/common-dbcp/configuration.html
- pom.xml 에 스프링 jdbc 의존성 설정
- applicationContext.xml 에 DataSource 설정

### Spring JDBC Class

- JdbcTemplate  
  기본 클래스
- NamedParameterJdbcTemplate  
  파라미터 설정시 인덱스 대신에 이름을 사용
- SimpleJdbcTemplate  
  가변 인자 사용 가능
- SimpleJdbcInsert  
  삽입에만 이용
- SimpleJdbcCall  
  프로시저 호출

## 3. MyBatis

- `SQL Mapper Framework`
  - SQL을 자바 코드와 분리시켜 사용하는 Framework

### 장점

- 사용 편리
- 파라미터 매핑이나 Select 구문의 결과 매핑이 자동

### 단점

- 성능이 떨어짐

### 의존성

- 사용하고자 하는 관계형 DB 라이브러리
  - spring-jdbc
  - mybatis
  - mybatis-spring

### 구현 방법

- xml 이용
- 인터페이스 이용

### 설정

- pom.xml 파일에 의존성 추가
- MyBatis 환경 설정 파일 작성

### xml을 이용한 mybatis 사용

- 연동할 DB 테이블 생성

- mybatis-config에 VO 클래스 경로 등록해 이름만으로 사용
- SQL 작성 파일을 저장할 디렉토리 생성
- mappers 디렉토리 안에 데이터 삽입 sql 작성
- MyBatis 사용을 위한 설정 xml 파일에 추가
- SQL 사용 Repository 생성 후 데이터 삽입 메서드 작성
- Good.xml 파일에 SQL 구문 추가
- GoodRepository 클래스에서 구문을 불러오는 메서드 생성

## 4. 데이터베이스 작업 로그 출력

- log4jdbc-log4j2 라이브러리 이용
- 로그를 출력하기 때문에 `잘못된 SQL 이나 속성의 이름 파악에 도움`  
  SQL 실행 속도는 느려짐

### pom.xml 파일에 의존성 설정

### resources 디렉토리에 log4jdbc.log4j2.properties 작성

### applicationCOntext.xml에 DatoaSource 빈 수정

## 5. 트랜잭션 처리

- 스프링은 트랜잭션을 어노테이션을 이용해 처리
- TransactionManager 클래스의 bean을 만들고  
  \<tx:annotation-driven/>을 설정 파일에 추가해주면  
  메서드 위에 @Transactional만 추가하면 트랜잭션 처리
  - `예외 발생시 rollback 수행`, `미발생 시 commit`

### 트랜잭션 처리를 위해 Repository 클래스와 메서드 생성

### xml 파일에 SimpleJdbcInsert 클래스의 bean 생성 코드

### 트랜잭션을 위해 xml에 tx 네임스페이스 추가

### 트랜잭션을 적용할 메서드 위에 @Transactional 추가

- 오류 발생해 삽입되지 않음

## 6. **Hibernate**

### 개요

- Java 기반의 `ORM`
- Hibernate 가 만들어진 이후에 Java에서도 `ORM 대한 표준` Spect 발표,  
  이것이 **JPA**(Java Persistence API)
- `JPA는 인터페이스`이고, `Hibernate는 구현체`

### 장점

- 복잡하고 중복되는 JDBC 코드 제거
- 성능 우수

### 단점

- 배우기 어려움
- 여러 명의 팀 작업에는 MyBatis  
  적은 규모의 솔루션 개발에는 JPA 권장

### 필요 의존성

- 데이터베이스 라이브러리, Hibernate, Spring-JDBC, Spring-ORM

### Hibernate 사용 설정

- pom.xml에 관련 dependency 추가

### 테이블 과 연동할 모델 클래스 생성

- 어노테이션을 활용한 Id 와 Column 설정
- .xml 파일에 Hibernate 사용 빈 생성
- 메모리 DB 개념을 이용하기 때문에 성능 우수
