---
title: "Java DB 연동"
excerpt: "Java 와 DB를 연동하는 방법에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, DB]

toc: true
toc_sticky: true

date: 2022-12-28
last_modified_at: 2022-12-28
---

## 6. HTML Parsing

### Jsoup 라이브러리 사용

- mvnrepository.com 에서 검색 후  
  pom.xml 파일의 dependencies 태그 안에 추가

### main 메서드에 **parsing 문구** 작성

- org.jsoup 의 클래스 이용

### URL을 이용한 html 다운로드의 한계

- `동적인 데이터`(ajax 등) 나 `iframe 의 데이터`를 가져올 수는 없음
  - 브라우저를 직접 조작할 수 있는 **selenium** 이용  
    selenium을 통해 브라우저 조작 가능

## 7. **selenium**

- 웹 앱을 테스트하기 위한 도구

### 준비

- 브라우저
- 브라우저 버전에 맞는 드라이버

### 브라우저 버전에 맞는 드라이버 다운

- pom.xml에 의존성 설치
- 자바스크립트 코드 실행 가능

## 8. 암호화

### **암호화**

- `데이터 원본을 알아볼 수 없도록` 다른 형태로 만드는 것이 **암호화**  
  `암호화된 문장을 가지고 원래 문장(평문)을 복원`하는 작업이 **복호화**
- 암호화 기법
  - 암호화 문장을 비교할 수 있지만 `복호화는 못하는` 형식
    - **해싱**  
      원문을 모두 같은 길이의 문자열로 만드는 것
  - 암호화 와 `복호화가 모두 가능`한 방식

### BCrypt 라이브러리

- `복호화가 안되는 암호화`를 위한 라이브러리
  - Spring Security 에는 해당 기능 내장
- 암호화
  ```java
  BCrypt.hashpw(원문, BCrypt.gensalt());
  ```
- 비교
  ```java
  BCrypt.checkpw(원문, 암호화문)
  ```

### 실습

- pom.xml 에 의존성 설정
- 64 자리나 128자리 방식을 생각하고, 순서 명시

# Database 연동

## 1. 연동 방법

### JDBC 코드 만으로 연동

- 사용하고자 하는 `데이터베이스 드라이버`만 있으면 됌
- 드라이버가 `데이터베이스 와 자바 사이의 인터페이스`

### Framework 이용

- 드라이버 와 프레임워크 라이브러리 모두 준비
- 관계형 데이터베이스의 경우는 2가지로 사용
  - **SQL Mapper**  
    `SQL을 Mapper File 이나 Mapper Interface 에 작성`해  
    SQL과 Java 코드 분리
    - 사용하기 쉬우나 성능이 떨어짐
    - MyBatis 가 대표적인 프레임워크
  - **ORM**  
    `테이블의 하나의 행 과 개발 언어의 인스턴스를 일 대 일로 매핑`
    - 학습이 어려우나 성능 우수
    - JPA(구현체는 Hibernate)
    - NoSQL 도 이 방식으로 사용 가능
- 프레임워크 방식은 JDBC 코드로 변환되서 실행 되나  
  ORM은 코드 변환이 되지만, 데이터를 가져와서 사용하는 방식이 다름

## 2. JDBC 프로그래밍 절차

- `드라이버 클래스` 로드  
  한 번만 수행
- `데이터베이스 연결`  
  Connection 클래스
- `SQL문 실행`  
  **Statement**

  - 완성된 SQL을 작성하나 보안 문제로 비추천

  **PreparedStataement**

  - 파라미터를 바인딩 가능한 실행 클래스로 대부분 이용

  **CallableStatement**

  - 프로시저를 이용

- `결과 사용`  
  **int**

  - select를 제외한 구문의 결과  
    `영향받은 행의 개수` 리턴

  **ResultSet**

  - select 구문의 결과  
    `데이터를 순회할 수 있는 커서` 리턴

- 데이터베이스 연결 해제

## 3. 드라이버 로드 및 접속 과 해제

- 드라이버를 build path 에 추가하고 데이터베이스 접속 정보 필요
  ```java
  Class.forName(드라이버이름); // 드라이버 클래스 로드
  // 데이터베이스 종류마다 드라이버 이름 다름
  ```
- 연결

  ```java
  Connection 변수 =
    DriverManager.getConnection(DB경로/이름, id, pw);
  // 아이디와 비번은 생략이 가능한데
  //MongoDB 처럼 아이디, 비밀번호 없어 사용 가능한 경우 이거나
  //운영체제 인증(OS Authentication)을 사용한 경우
  ```

  옵션 설정

  - 트랜잭션 사용 여부  
    자바는 기본이 Auto Commit

- 해제  
  Connection 인스턴스로 close( )를 호출

## 4. MariaDB

### 의존성 설정

### 데이터베이스 접속 정보를 별도의 파일에 작성

- 특별한 경우를 제외하고는 DB 접속 정보는 하드 코딩하지 않음
  - 보안 문제 와 운영 환경으로의 이전 문제
- **properties 파일**이나 **xml 파일**에 많이 진행
  - **yaml(yml)**에도 작성

### db.properties 파일을 생성

### 트랜잭션 사용

- Connection 인스턴스를 가지고 **setAutoCommit**(boolean b)로 설정
  - AutoCommit 해제시 **commit**( )과 **rollback**( ) 로 사용

## 5. SQL 구문

### 실행 클래스

- **Statement**
  - Connection 인스턴스의 **createStatement**( )로 생성  
    `데이터 바인딩이 되지 않음`
    - `보안성이 떨어져 거의 미사용`
- **PreparedStatement**
  - Connection 인스턴스의 **prepareStatement**(String sql)로 생성  
    sql을 만들 때 `정해지지 않은 값은 ?` 로 설정
    - **set자료형**(? 인덱스, 실제 데이터)  
      `? 에 데이터 바인딩`
    - SQL 실행은 int executeUpdate( ), ResultSet executeQuery( )
    - **날짜**의 경우 java.sql.Date, **시간**은 java.sql.time  
      **날짜 시간**을 모두 설정할 때는 java.sql.Timestamp
    - 파일 내용은 blob
- **CallableStatement**
  - Connection 인스턴스의 **getCall**(String procedureName)로 생성  
    `프로시저를 수행`
    - ResultSet executeQuery( ) 로 실행

### 결과 사용(ResultSet)

- **next**( )  
  다음 데이터가 있으면 true, 없으면 false 리턴
- **get자료형**(컬럼이름 이나 컬럼인덱스)  
  컬럼 이름이나 인덱스에 해당하는 컬럼의 값을 리턴
  - 자료형을 String으로 설정하면 모든 데이터 전부 받아올 수 있음

## 6. DTO & DAO 패턴

### **DTO**(Data Transfer Object) Pattern

`여러 개의 데이터를 하나로 묶기 위해서` 사용하는 패턴

### **DAO**(Data Access Object) Pattern

`데이터를 연동하는 로직을 별도의 클래스`로 만들어서 처리

### Singleton Design Pattern

`클래스의 인스턴스를 1개만 생성`할 수 있도록 하는 Design Pattern

- `Server에서 작업을 처리`하는 클래스  
  동시에 처리하는 것이 아닌 Multi Thread를 이용하기 때문
- `공유 자원을 소유`하는 클래스

### Template Method Pattern

`인터페이스에 메서드의 모양`을 만들고 이를 `implements 한 클래스`를 만들어  
`실제 내용을 구현`하는 패턴

- 최근의 프레임워크에서는 `Service Layer에만 적용`  
  DAO Layer는 인터페이스 구현으로 종료

## 7. 실습

### 프로젝트에 DTO 클래스 생성

### DAO 인터페이스 생성

### DAOimpl 클래스 생성

### 클래스로 생성

### 초기화 작업

- 데이터베이스 연동에 필요한 변수 선언

### 전체 데이터 가져오기

- GoodDAO 클래스에 goods 테이블의 전체 데이터 가져올 메서드

### 코드를 이용해 조회

- GoodDAO 인터페이스에 메서드 선언
- GoodDAOImpl 클래스에 구현

### 데이터 삽입

- GoodDAO 인터페이스에 메서드 선언
