---
title: "Java Spring Boot JPA의 쿼리 처리"
excerpt: "Spring Boot를 활용해 JPA 상 쿼리를 처리하는 방법에 대해 학습하고,
"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot, JPA]

toc: true
toc_sticky: true

date: 2023-01-11
last_modified_at: 2023-01-11

---

## 1. JPA 의 Query 처리

### JPARepository 가 기본적으로 제공하는 메서드를 사용

- save, delete, findAll, findById

### QueryMethod 이용

- `이름이 Query` 인 메서드
- 인터페이스에 메서드 만들 때 규칙을 지켜주면 메서드 구현을 자동 수행

### @Query를 이용해서 JPQL 이나 SQL을 작성해서 실행

- 쿼리를 문자열로 작성하므로 `컴파일 시점에 에러를 확인할 수 없음`
- 동적인 쿼리를 만드는 것이 어려움
  - 상황에 따라서 변하는 쿼리

### query dsl

- JPQL을 코드로 작성할 수 있도록 도와주는 빌더 API
- 쿼리를 자바 코드로 작성
- 장점
  - 조건에 맞게 `동적으로 쿼리를 생성`할 수 있음
  - `쿼리를 재사용`할 수 있고 `제약 조건 조립 및 가독성을 향상`
  - `컴파일 시점에 오류`를 잡아낼 수 있음
  - `IDE의 자동 완성 기능`을 사용할 수 있음
- 단점

  - spring boot 버전에 따라서 설정 방법이 다름
    - 2.5 <-> 2.6, 2.7 <-> 3.0

## 2. MappedSuperclass

- `테이블로 생성되지 않는` Entity 클래스
- 추상 클래스 와 유사,  
  여러 `Entity 가 공통으로 가져야 하는 속성을 정의`하는 클래스
- 등록 시간이나 수정 시간처럼 여러 Entity 가 공통으로 갖는 속성을 정의

## 3. JPA Auditing

- Entity 객체에 어떤 `변화가 생길 때 감지하는 리스너`가 존재
- @**EntityListeners**(value={클래스이름.class})을 추가하면,  
  Entity에 `변화가 생기면 클래스의 메서드`가 동작
  - 보통은 클래스를 직접 만들지 않고,  
    Spring JPA가 제공하는 **AuditingEntityListener**.class를 설정
- 이 기능을 사용하기 위해서는 SpringBootApplication 클래스 상단,  
  @**EnableJpaAuditing**을 추가해야 함

## 4. Entity 와 DTO

- 두가지 모두 `속성들을 합쳐서 하나로 묶기 위해` 만드는 클래스
- **Repository**에서는 Entity 객체를 이용하고,  
  그 이외의 영역에서는 DTO를 사용하는 권장
- 사용자의 요청이나 응답 과 Entity가 `일치하지 않는` 경우가 많고  
  Entity는 `JPA 가 관리하는 Context` 에 속하기 때문에  
  직접 관리하는 것은 `일관성에 문제가 발생`할 가능성이 있기 때문
- DTO는 `용도 별로 생성`하는 것을 기본

## 5. 애플리케이션 개요

### 요청

- 목록 보기  
  list - GET 방식
- 등록  
  register - GET, POST(목록 보기로 리다이렉트)
- 조회  
  read - GET
- 수정  
  modify - GET, POST(조회로 리다이렉트)
- 삭제  
  remove - POST(목록 보기로 리다이렉트)

### 구조

```
Controller(Rest Controller 도 생성) <->
Service(ServiceImpl) <->
Repository
DTO Entity
```

## 6. Spring Boot Project 생성

- 의존성
  - lombok, spring web, thymeleaf, spring data jpa, 사용 DB

## 7. 환경 설정

- application.properties 파일을 application.yml 파일로 변경하고 작성
- yml 파일을 작성시 들여쓰기 와 값을 설정할 때 공백하나 추가
  - 애플리케이션을 실행해서 오류가 있는지 확인하고  
    데이터베이스 연결 pool  
    (미리 만들어두고 빌려서 사용할 수 있도록 해주는 것) 생성 확인

## 8. 화면 출력 준비

- sidebar의 압축을 해제해서 모든 파일을 static 디렉토리에 복사
- templates 디렉토리에 layout 디렉토리를 만들고  
  기본 레이아웃 파일인 basic.html을 복사
- templates 디렉토리에 view를 저장할 guestbook 디렉토리를 생성

### 사용자의 요청을 받아서 뷰로 출력하도록 해주는 ViewController를 만들어서 기본 요청을 처리하는 메서드를 작성

- 실행 클래스에 JPA 감시를 위한 어노테이션을 추가
  ```java
    @SpringBootApplication
    //JPA의 변화를 감시하겠다는 어노테이션
    @EnableJpaAuditing
    public class GuestbookApplication {
  ```

## 9.Entity 작업

### 생성 날짜 와 수정 날짜를 모든 Entity 가 소유

- 하나의 상위 Entity를 만들어서 상속,  
  이 상위 Entity는 테이블로 만들어지면 안되니

  - @**MappedSuperclass** 를 추가해주면 됩니다.

- 생성 날짜는 속성 위에 @CreateDate 를 추가

- 수정 날짜는 속성 위에 @LastModifiedDate 를 추가

### 공통된 속성을 가진 Entity 생성

```java
  @MappedSuperclass
  //Entity 변화 감시
  @EntityListeners(value = {AuditingEntityListener.class})
  @Getter
  public class BaseEntity {
  @CreatedDate
  @Column(name="regdate", updatable=false)
  private LocalDateTime regDate;

      @LastModifiedDate
      @Column(name="moddate")
      private LocalDateTime modDate;

  }
```

### application 에서 사용할 Entity 생성

- 컬럼

  - 기본키로 사용할 번호, 자동생성
  - 제목 문자열, 100자에 필수 입력
  - 내용 문자열, 1500자에 필수 입력
  - 작성자 문자열, 50자에 필수 입력
  - 작성 날짜
  - 수정 날짜

- 콘솔 창에서 SQL 확인 유무 체크
  - SQL이 출력안되면 .yml 파일 설정 확인
  - 테이블이 생성 안되면 .yml의 ddl-auto 확인

## 10. GuestBook DB 를 위한 Repository

### GuestBook Entity 인터페이스 생성

- extends JpaRepository\<Entity 클래스 이름, 기본키 자료 형>

### query dsl 사용 설정

- build.gradle 에 작업

### Query DSL 사용을 위한 Repository 인터페이스

- extends JpaRepository\<..>, QuerydslPredicateExecutor\<Entity 이름>

### Repository test 클래스 생성 후 샘플 데이터 삽입
- save( )
  - 기본키의 값을 설정하지 않거나 존재하지 않는 값이면 삽입,  
존재하는 값이면 수정으로 처리

### query dsl 테스트
- 사용법
  - **BooleanBuilder를** 생성
  - 구문은 Predicate 타입의 함수를 생성
  - BooleanBuilder에 작성된 Predicate을 추가하고 실행

### title에 1이라는 글자가 포함된 Entity 조회

## 11. Service Layer
### Service 와 Controller 그리고 View 가 사용할 GuestBook 관련 DTO 클래스 생성

### Service 인터페이스 생성
- Service 에서 가장 많이 하는 것 중 하나가 DTO 와 Entity 사이의 변환
  - 인터페이스에 default method로 추가해주는 것이 좋음}

### ServiceImpl 클래스 생성

### 데이터 삽입
- Service 인터페이스에 데이터 삽입을 위한 메서드를 선언
  
- ServiceImpl 클래스에 데이터 삽입을 위한 메서드를 구현
- ServiceTest 클래스를 만들어서 메서드 테스트

### 목록 보기
- 게시판 형태에서 목록 보기 요청
  - 페이지 번호, 페이지 당 데이터 개수, 검색 항목, 검색 값  
  (이런 형태의 요청 DTO 가 필요)

- 목록 요청을 위한 DTO 클래스 생성

- 응답을 위한 DTO

- Service 인터페이스에 목록 보기를 위한 메서드를 선언
    
- ServiceImpl 클래스에 목록 보기를 위한 메서드를 구현

- ServiceTest 클래스에 테스트 메서드를 작성하고 확인
  
- 페이지 번호 목록을 출력할 수 있도록 PageResponseDTO 수정

- ServiceTest 클래스에 테스트 메서드를 만들어서 확인

## 12. Controller 와 View
### 목록 보기
- ViewController를 수정

- list.html 수정

- 목록 보기를 JSON 데이터 형식으로 보내기 위한 Controller를 생성하고 요청 처리 메서드 작성


## 13. 데이터 삽입 구현
### ViewController에 데이터 삽입을 위한 메서드 추가

### 데이터 삽입 화면 생성 - register.html

### list.html 파일에 작성 링크를 만들고 메시지 출력 영역을 생성
