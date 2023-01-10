---
title: "Java Spring 테스트 코드 와 JPA, MyBatis"
excerpt: "Java Spring을 통해 테스트 코드를 구현하고,
JPA 를 활용해서 DB를 연결하는 방법에 대해 학습하고,
MyBatis 활용법에 대해서도 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot, JUnit, JPA, MyBatis]

toc: true
toc_sticky: true

date: 2023-01-10
last_modified_at: 2023-01-10

---

### 링크 생성

- 기본 형식  
  \<a href="url"> 이미지 나 텍스트\</a>
- Thymeleaf 에서는 **@{ }** 로 href 설정
- 파라미터 설정은 **(파라미터이름 = ${데이터})**

### Controller 클래스에 요청 처리 메서드 생성

### 해당 template 작성

### `출력 포맷` 설정

- `숫자의 경우`는 #numbers 를 이용해 설정
- `날짜의 경우`는 #temporals 를 이용해 설정

### GetMapping 의 정도에 따라 추가적인 파일 작성

- {"링크1", "링크2"}

### 레이아웃 설정

- include 방식  
  외부에서 내용을 가져와서 삽입
  - th:insert  
    삽입
  - th:replace  
    교체
- controller 클래스 요청 처리 메서드 작성
- 레이아웃에 사용될 파일을 저장할 fragments 디렉토리 templates에 생성
- fragments 디렉토리 파일 추가
- template에 파일을 작성하고 fragments 사용

## 5. bootstrap 의 siderbar 템플릿

### bootstrap

- `반응형 웹`을 만들기 쉽게 해주는 자바스크립트 라이브러리
- 이벤트 처리에 jquery 이용 가능

### bootstrap 라이브러리

- https://startbootstrap.com/template/simple-sidebar
- 해당 템플릿 다운 받은 후 static 파일이 이동
  - index.html 의 내용을 templates/layout으로 이동

# Spring Boot 와 DB 연동

## 1. **Test**

### 테스트 코드 작성

- 개발 과정에서 문제를 미리 발견하여 `리팩토링 리스크 감소`
- `하나의 명세 문서`가 됨
- `불필요한 내용 추가 방지`

### 테스트 종류

- **단위 테스트**(Unit Test)  
  `메서드 나 클래스` 테스트
  - Repository, Service, Controller 테스트
- **통합 테스트**(Integration Test)  
  `하나의 서비스`(모듈 결합)을 테스트
  - 3개 모듈을 합쳐서 테스트
- 시스템 테스트(System Test)
- 인수 테스트(Acceptance Test)  
  최근, 클라우드 환경에서 개발시 이 과정 생략

### 코드 작성 방법 - Given When Then 패턴

- 단계 설정해서 코드 작성
- 단계
  - **Given**  
    테스트를 하기 전에 필요한 `환경을 설정`
    - 변수 설정이나 Mock 객체(가짜 객체) 생성
  - **When**  
    테스트 목적을 보여주는 단계로 `코드 작성해 결과값` 가져오기
  - **Then**  
    결과 `검증`

### 좋은 테스트를 위한 5가지 속성

- **Fast**
- **Elated**(고립, 독립)
  - 외부 요인으로 부터 독립적
- **Repeatable**(반복)
- **Self Validationg**(자가 검증)
- **Timely**(적시에)
  - 실제 구현되기 전에 테스트

### Spring Boot 에서 Test

- spring-boot-starter-test 의 의존성 설정
  - JUnit5, Mock 포함

### JUnit5 의 어노테이션

- @**Test**  
  테스트 메서드
- @**BeforeAll**  
  테스트 시작 전 한 번 호출
- @**BeforeEach**  
  모든 테스트 시작 전 호출
- @**AfterAll**  
  테스트 수행 후 한 번 호출
- @**AfterEach**  
  모든 테스트 수행 후 호출

### 가짜 객체

- `외부로 주입받아야 하는 경우` 실제 객체 만들거나, 있다 가정 후 테스트
  ```java
  @MockBean 클래스 변수 이름;
  ```

## 2. 연동 방법

- Java JDBC 코드 이용
- Framework 이용
  - Spring JDBC 이용
  - JPA(ORM) 이용
  - SQL Mapper Framework(MyBatis) 이용

## 3. ORM(Object Relational Mapping)

### ORM

- `객체 지향 패러다임을 관계형 데이터베이스에 보존`하는 기술
- `객체 와 관계형 데이터베이스의 테이블 매핑`해 사용
- 관계형 데이터베이스에서 테이블을 설계하는 것 과,  
  Class 만드는 것은 템플릿을 만든다는 점에서 유사
  ```java
  member 테이블   ---- Member Class
  uid varchar(50) ---- String uid
  upw varchar(50) ---- String upw
  ```
- Instance 와 Row 가 유사

### 장점

- 특정 관계형 데이터베이스에 종속되지 않음
- 객체 지향 프로그래밍에 집중 가능
- 생산성 향상

### 단점

- 쿼리 처리가 복잡
- 성능 저하 위험
- 학습 시간 소요

### JPA(Java persistence API)

- `Java ORM 기술에 대한 표준 API`
- JPA 는 인터페이스이고 구현체는 Hibernate, EclipseLink 등
  - 대부분 Hibernate 사용
  - 데이터베이스 (<-> JDBC <-> Hibernate <->) JPA
- **Persistence Context**  
  애플리케이션 과 DB `사이의 중재자` 역할
  - Entity <-> Persistence Context <-> Dataase
  - 직접 연결이 아닌 중간 매개체를 통하면 `버퍼링, 캐싱` 등 가능
  - `쓰기 지연`을 수행  
    트랜잭션 처리로 `commit 전까지 DB 반영 안됨`

### Entity 클래스 생성

- VO 클래스 상단에 @Entity 추가

## 4. JPA 활용 애플리케이션 생성

### 프로젝트 생성

- 의존성  
  Lombok, Spring Data JPA, Spring Web, 사용 데이터베이스

### 초기 실행 오류

- JPA 사용시 DB 바로 접속하려는데 접속 정보 없어서 에러

### application.yml 에 DB 접속 정보 저장

- 이후 실행시 HikariDataSource 라이브러리로 DB 접속 여부 확인 가능

## 5. Entity Class 와 JpaRepository

### 개발에 필요한 코드

- **Entity** Class 와 **Entity** Object
- Entity 를 처리하는 **Repository**
  - `인터페이스`로 설계
  - JPA `제공 인터페이스 상속`시,  
    Spring Boot 내부에서 `자동 객체 생성 및 실행`
  - 일부 메서드는 이미 구현, 메서드 선언만 해도 구현되는 경우 있음  
    단순 CRUD 나 페이지 처리 개발 시 직접 코드 미작성

### Entity 관련 Annotation

- @Entity  
  Entity 클래스 생성
- @Table  
  `Entity 와 매핑할 테이블` 설정,  
  생략시 `클래스 이름 과 동일한 이름 테이블` 과 매핑되는데,  
  mysql/maria db는 첫글자 이외 대문자 있으면 \_ 추가 후 소문자 변경
  - name 속성  
    테이블 이름 설정
  - uniqueConstraints 속성  
    여러 개의 컬럼이 합쳐져서 유일성을 갖는 경우 사용
    ```java
    uniqueConstraints={@UniqueConstraint(
      columnNames={컬럼이름 나열}
    )}
    ```
- @Id  
  `컬럼 위에 기재해서 Primary Key를 설정`, 필수
- @GeneratedValue  
  `키 생성 전략을 기재`하는데 @Id 와 같이 사용
- @Column  
  `컬럼 위에 기재`해서 `테이블의 열 과 매핑`
  - name 속성  
    컬럼 이름 설정, 생략시 속성 이름과 동일한 컬럼으로 매핑
  - unique 속성
  - nullable 속성
  - insertable, updateable 속성
  - precision 속성  
    숫자의 전체 자릿수
  - scale 속성  
    소수점 자릿수
  - length 속성  
    문자열 길이
- @Lob  
  BLOB 나 CLOB 타입
  - **lob**는 바이트 배열로, 보통 `파일의 내용 저장`할 목적
    - 파일 저장시 대부분은 `파일 경로를 문자열 저장`,  
      파일은 스토리지(Amazon-S3 등) 에 따로 저장
- @Enumerated  
  enum 타입으로, check 제약조건에 해당
- @Transient  
  `테이블 생성시에 제외`되어, 파생 속성
  - 다른 속성을 가지고 만들어내는 속성  
    나이 등
- @Temporal  
  날짜 타입 매핑
- @CreatedDate  
  생성 날짜 자동 삽입
- @LastModifiedDate  
  수정 날짜 자동 삽입
- @Access  
  `사용자가 값을 사용할 때 바인딩` 하는 방식
  - @Access(AccessType.FIELD)  
    속성 직접 사용
  - @Access(AccessType.PROPERTY)  
    getter 와 setter 사용

### 기본키 생성 전략

- AUTO  
  default로, `JPA 가 생성 전략 선택`
  - MariaDB 의 AutoIncrement
- IDENTITY  
  AutoIncrement 로,  
  oracle 에서는 이 전략 사용시 테이블 생성 불가
- SEQUENCE  
  oracle 의 sequence 이용
- TABLE  
  직접 생성 전략 결정

### 기본키를 복합키로 생성

- 기본키로 사용할 속성들을 `별도의 VO 클래스`로 만들고,  
  `Serializable 인터페이스 구현` 후,  
  기본 생성자와 모든 속성 생성자를 만들고 Getter 만 추가해 생성
- Entity 클래스에 위 클래스의 속성 추가후 @**Id** 와 @**embeddable** 추가

### DB 연동할 Entity 클래스 생성

### .yml에 jpa 설정 추가

### Entity에 CRUD 작업 Repository 생성

- `extends JpaRepository`< , >
  - 2개의 템플릿 자료형 설정  
    `Entity 클래스`와 `Entity 클래스 @Id 속성의 자료형` 설정
- 자동 생성되는 메서드
  - insert, update  
    **save**(Entity 객체)
  - select  
    **findById**(기본키값), **findAll**()
  - count  
    **count**()
  - delete  
    **deleteById**(기본키값), **delete**(Entity 객체)

### 테스트 코드

- 필요 코드 @Autowired로 DI

### 페이징 & 정렬

- 관계형 DB에서 `페이지 단위로 데이터를 가져오는 것`(Top N)
  - 데이터베이스마다 다른 방식으로 구현  
    Orcale - Inline View / FETCH & OFFSET  
    MySQL/Maria DB - limit
- JPA 는 `연결한 DB 에 따라 SQL 자동 변환`
- `페이징 과 정렬`은 **findALL** 메서드 이용
  - Pageable 이라는 `인터페이스의 객체`를 대입하면,  
    Paging 처리를 해서 `Page\<T> 타입`으로 리턴
- Pageable 인터페이스 생성
  ```java
  PageRequest.of(int page, int size) // 0부터 시작하는 페이지 번호 와 페이지 당 데이터 개수 설정
  PageRequest.of(int page, int size, Sort.Direction, String..proprs) //정렬 방향과 정렬 속성을 추가
  PageRequest.of(int page, int size, Sort sort) // 정렬 관련 정보를 Sort 객체로 생성해 대입
  ```

### 정렬해서 페이징

- Sort 클래스 와 .by( ) 메서드 이용
- 2개 이상 조건 결합
  - Sort 클래스의 인스턴스끼리 .and( ) 로 합치고,  
    반복문으로 출력

### query methods

- `인터페이스에 메서드 이름만 생성`하면 **select** 와 **delete** 구문 생성
- https://docs.spring.io/spring-data/jpa/docs/current/refenrece/html/#jpa.query-methods
- 이름 자체가 query
  ```java
  find + (Entity 이름) + By + 컬럼이름
  // Item Entity 에서 name 조회
  findItemByName(String name)
  ```
- Or 나 And 사용 가능
- **매개변수**는 자신이 설정하는 Select 형태에 따라 설정,  
  `Pageable 과 Sort 추가` 가능
- Pageable이 없으면 `리턴 타입은 List` 이고, `존재시 Page`

### 조건에 해당하는 값만 조회하는 메서드 생성

- 자동 생성 구문에 맞춰 확인

### SQL 실행

- @**Query**
  - `JPQL로 JPA에서 제공`하는 쿼리 문법
  - `Native SQL로 데이터베이스에서 실제 사용`하는 SQL
- Select 가 아닌 경우는 @Modifying과 같이 사용

### SQL 및 페이징 실행 메서드

### `Object []` 리턴 메서드

- select 구문을 보낼 때 제공되는 `기본 메서드나 Query 메서드` 이용시  
  `Entity 타입으로 리턴`이 되고, `원하는 데이터만 추출 불가능`
- Join 이나 Group by 등 SQL 을 실행시,  
  이에 맞는 `Entity 가 존재하지 않을` 가능성이 매우 높음
  - **원하는 내용 추출** 시 `Object [] 리턴`하는 메서드 사용

### mno 와 memoText 와 현재 시간 조회

- 적당한 Entity가 존재하지 않음
  - Object [] 실행

### nativeSQL 실행

```java
@Query(value="sql 작성", nativeQuery=true)
```

## 6. Oracle 변경

### Docer에 오라클 설치 및 실행

```java
docker search 이름
// 오라클은 버전 주의
docker pull jaspeen/oracle-xe-11g

docker run --name 컨테이너이름 -d -p 8080:8080 -p 1521:1521
jaspeen/oracle-xe-11g

// oracle의 sid 는 xe(orcl)
//관리자 계정은 system - oraacle
```

### build.gradle 에 오라클 의존성 확인

- .yml 파일 수정

## 7. Spring Boot 에서 MyBatis 사용

### build.gradle 에 MyBatis 의존성 설정

### 테이블 과 연동할 DTO 클래스 생성

### SQL 실행할 인터페이스 생성
