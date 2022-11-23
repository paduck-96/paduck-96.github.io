---
title: "MariaDB"
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop]

toc: true
toc_sticky: true

date: 2022-11-23
last_modified_at: 2022-11-23
---

### 테이블 생성

컬럼

- num 정수
- name 문자열, 영문 20자, 자주 변경
- address 문자열, 영문 100자, 자주 변경 x
- tel 문자열, 영문 20자, 자주 변경
- email 문자열, 영문 100자, 자주 변경
- birthday 날짜

`비밀번호 저장 시 해시 수행해 코드 저장하기 때문에 해시 방법에 따라 암호화`

```sql
create table contact(
    num integer,
    name char(20),
    address varchar(100),
    tel char(20),
    email char(100),
    birthday date
)ENGINE=MyISAM
```

---

## 2. 테이블 구조 변경

### 기본 형식

```sql
ALTER TABLE 테이블이름 작업 매개변수 나열
```

### 테이블 구조 확인

```sql
DESC 테이블이름;
```

### 컬럼 추가

```sql
ALTER TABLE 테이블이름 ADD 컬럼이름 자료형 제약조건
```

### 컬럼 삭제

```sql
ALTER TABLE 테이블이름 DROP 컬럼이름;
```

### 컬럼 변경

```sql
ALTER TABLE 테이블이름 CHANGE 기존컬럼이름 새로운컬럼이름 자료형 제약조건
-- 자료형만 변경
-- NOT NULL 에 대한 설정 포함(한 칸 추가 확보)
ALTER TABLE 테이블이름 MODIFY 기존컬럼 자료형
-- 크기가 커지는건 괜찮으나, 작으면 손실이 발생할 수 있음
```

`컬럼 추가, 삭제는 RDB 거의 비슷하나 컬럼 변경은 다름`

### 컬럼 순서 조정

> 새로운 컬럼은 맨 뒤에 추가

```sql
-- 컬럼 맨 앞으로
ALTER TABLE 테이블이름 MODIFY COLUMN 컬럼이름 자료형 FIRST;
-- 컬럼 순서 지정
ALTER TABLE 테이블이름 MODIFY COLUMN 컬럼이름 자료형 AFTER 앞컬럼;
```

### 테이블 이름 조정

```sql
ALTER TABLE 원래테이블이름 RENAME 새로운테이블
```

---

## 3. 테이블 삭제

### 기본 형식

```sql
DROP TABLE 테이블이름;
```

### 테이블 삭제 불가능

## **외래키로 참조되는 테이블**은 `외래키를 소유하고 있는 테이블 삭제`가 우선

## 4. 테이블 데이터 삭제

```sql
TRUNCATE TABLE 테이블이름
```

---

## 5. 테이블 압축

```sql
CREATE TABLE ROW_FORMAT = COMPRESSED
```

저장 공간 줄어드나 작업 속도 느려짐

---

## 6. 주석 설정

```sql
COMMENT ON TABLE 테이블이름 IS "주석";
```

---

## 7. 제약 조건(Constraint)

### 1. 무결성 제약 조건

- Entity Integrity(개체 무결성)  
  기본키는 NULL 이거나 중복될 수 없다
- Referential Integrity(참조 무결성)  
  외래키는 참조할 수 있는 값이나 NULL

- Domain Integrity(도메인 무결성)  
  속성의 값은 정해진 도메인의 값을 가져야 한다

---

### 2. NOT NULL

NULL 일 수 없다 라는 제약 조건

- 필수 입력
- 컬럼 크기와 관련 있어 컬럼 만들 때 제약조건 설정

`테이블 제약 조건으로 생성 불가`

```sql
컬럼이름 자료형 NOT NULL
```

`기본은 NULL 허용`

---

### 3. DEFAULT

> 데이터베이스 이론에서 DEFAULT 는 제약 조건이 아님

> 입력하지 않았을 때 기본적으로 삽입되는 데이터

```sql
DEFAULT 값
```

- 숫자  
  0
- 문자열  
  " "N/A"
- 날짜  
  현재 시간(CURRENT_TIMESTAMP 나 NOW 등)

---

### 4. CHECK

값의 종류 나 범위를 제한하기 위한 제약조건

```sql
CHECK(컬럼이름 조건);
--
GENDER CHAR(3) CHECK(GENDER IN ("남", "여"))
SCORE INTEGER CHECK(SCORE BETWEEN 0 AND 100)
--
```

---

## 5. PRIMARY KEY(기본키)

테이블에서 `PRIMARY KEY는 한 번 만 설정 가능`

- 2개 이상의 컬럼으로 복합키 설정하는 경우 테이블 제약 조건으로 진행
  - 학습시보다는 실무 사용

```sql
-- 컬럼 제약
CREATE TABLE MEMBER(
    ID VARCHAR(50) PRIMARY KEY
);
--테이블 제약
CREATE TABLE MEMBER(
    ID VARCHAR(50),

    PRIMARY KEY(ID)
)
-- 복합키
CREATE TABLE MEMBER(
    ID VARCHAR(50),
    NAME VARCHAR(50),
    ...
    PRIMARY KEY(ID, NAME)
)
```

PRIMARY KEY 는 자동으로 **클러스터 인덱스** 를 생성

- 저장 순서대로 하나만 만들어지는 인덱스

- 조회시 가장 빠른 성능

### ` PRIMARY KEY 는 NOT NULL 이고 UNIQUE`

---

## 6. UNIQUE

중복 값을 가질 수 없도록 하는 제약조건

- NULL 허용

인덱스 생성시  
`PRIMARY KEY 가 없으면` UNIQUE 가 클러스터 인덱스  
`있으면` 보조 인덱스

- PRIMARY KEY 와 더불어 다른 테이블의 FOREIGN KEY 가 될 수 있음

## 제약조건 이름 설정

```sql
CONSTRAINT 제약조건이름
```

- 테이블 이름 과 제약조건의 약자를 조합해서 만드는 경우가 대다수
  - PRIMARY KEY  
    pk
  - NOT NULL  
    nn
  - UNIQUE  
    uk
  - CHECK  
    ck
  - FOREIGN KEY  
    fk

---

## 8. 제약조건 수정

```sql
-- 수정
ALTER TABLE 테이블이름 MODIFY 컬럼이름 자료형 [CONSTRAINT 이름] 제약조건;
-- 추가
ALTER TABLE 테이블이름 ADD [CONSTRAINT 이름] 제약조건(컬럼이름);
--삭제
ALTER TABLE 테이블이름 DROP CONSTRAINT 제약조건이름;

-- NOT NULL 설정일 경우
-- 컬럼 자료형 수정
```

---

## 9. Sequence(일련번호)

컬럼 이름 뒤에 AUTO_INCREMENT 를 설정하면 일련번호 자동생성

- 생성되면 따로 값 대입 불필요

테이블 생성 시 초기 값 설정 가능

```sql
ALTER TABLE 테이블이름 AUTO_INCREMENT = 값;
```

- PK 나 UK 반드시 설정
- 테이블에서 한 번만 설정 가능

---

## 10. 참조 무결성

    tEmployee 직원 테이블
    tProject 직원 수행 프로젝트 테이블

    //foreign key 미설정
    CREATE TABLE tEmployee(
        name VARCHAR(20) PRIMARY KEY,
        salary INT NOT NULL,
        addr CHAR(100) NOT NULL);
    )

- 외래키 설정
  외래키는 상대방 테이블에서 PRIMARY KEY 나 UNIQUE 제약 조건이 설정되어 있어야함

```sql
-- 컬럼 제약 조건으로 설정
컬럼이름 자료형 [CONSTRAINT 제약조건이름] REFERENCES 참조하는테이블이름(컬럼이름) 옵션
-- 테이블 제약조건 설정
[CONSTRAINT 제약조건이름] FOREIGN KEY(컬럼이름) REFERENCES 참조하는테이블이름(컬럼이름) 옵션
```

---

## 11. 외래키 옵션

옵션없이 FOREIGN KEY 를 설정하면 외래키로 참조되는 데이터 삭제 불가

- 참조되지 않는 데이터 삭제 가능

외래키에 의해 참조되는 테이블은 먼저 삭제할 수 없고 `외래키 소유 테이블 삭제 우선`

외래키 설정할 때 옵션

```sql
ON DELETE [NO ACTION | CASCADE | SET NULL | SET DEFAULT]

ON UPDATE [NO ACTION | CASCADE | SET NULL | SET DEFAULT]
```

- no action  
  아무것도 하지 않음
- cascade  
  같이 삭제되거나 수정
- **set null**  
  null로 변경
- set default  
   default 값으로 변경
  > ON UPDATE 는 잘 사용하지 않는데, PRIMARY KEY 는 불변의 성격

---

# DML 과 Transaction

## 1.DML(data manipulation language)

데이터를 테이블에 삽입, 삭제, 갱신하는 SQL

- 개발자가 사용하는 언어

---

## 2. 데이터 삽입

```sql
INSERT INTO 테이블이름(컬럼 이름 나열)
VALUES(값 나열)
```

- 컬럼 이름을 생략하면 모든 컬럼의 값을 테이블을 만들 때 사용했던 순으로 대입

### NULL 삽입

- 기본 값이 없는 경우에는 컬럼 이름 생략하고 삽입
- 명시적으로 값을 NULL 이라고 설정
  - 문자열의 경우는 "" 형태로 입력해도 NULL로 간주하는 경우 있음

### 여러 개의 데이터 한꺼번에 삽입

- 기본 구조에서 값 나열 계속 붙이기

```sql
...VALUES(값 나열), (값 나열), ...
```

### 다른 테이블로부터 조회해서 삽입

```sql
INSERT INTO 테이블이름(컬럼 이름 나열)
SELECT 구문
```

### 조회한 결과로 테이블 생성

```sql
CREATE TABLE 테이블이름 AS
SELECT 구문
```

### 에러 무시하고 삽입

스크립트를 이용할 때 에러 무시하고 삽입하는 방법

```sql
INSERT IGNORE INTO 테이블이름(컬럼 이름 나열)
SELECT 구문
```

---

## 3. 데이터 삭제

```sql
DELETE FROM 테이블이름 [WHERE 조건];
```

- where 절 생략하면 모든 데이터 삭제
- TRUNCATE 와 유사하나, DELETE 는 `트랜잭션 설정`하면 **복구 가능**

`INSERT 는 성공하면 반드시 1개 이상 행이 영향`을 받으나  
`DELETE / UPDATE 는 where 절로 0개 이상의 행이 영향 받음`

- 조건에 맞는 데이터 없으면 영향 받는 행 수 0

외래키 옵션 없이 생성되면 삭제가 되지 않을 수도 있음

---

## 4. 데이터 수정

```sql
UPDATE 테이블이름
SET 수정할칼럼 = 값, ...
[WHERE 조건];
```

- where 절 생략시 테이블 모든 데이터 수정

---

## 5. Transaction

`한 번에 수행`되어야 하는 `논리적인 작업의 단위`

- 1개 이상의 DML 문장으로 구성

### 트랜잭션이 가져야 하는 성질

- Atomicity(원자성)  
  All Or Nothing > 전부 아니면 전무
- Consistency(일관성)  
  트랜잭션 수행 전 과 수행 후의 결과가 일관성 있어야 함

- Isolation(격리성, 독립성)  
  하나의 트랜잭션은 다른 트랜잭션의 영향을 받으면 안 되고 독립적 수행
- Durability(영속성, 지속성)  
  한 번 완료된 트랜잭션은 영원히 반영되어 수정할 수 없어야 한다

### 트랜잭션 구현 원리

DML 작업을 수행할 때는 원본 데이터가 아닌 `임시 작업 영역에 데이터 복사 후 진행`

- 작업 전부 완료하면 원본에 변경 내역을 반영; **COMMIT**
- 작업 도중 실패 시 변경 내역 미반영; **ROLLBACK**

### 트랜잭션 명령어

#### COMMIT

- 원본 반영

#### ROLLBACK

- 원본 미반영

#### SAVEPOINT

- ROLLBACK 할 위치 선정

### `트랜잭션 모드`

#### Manual

- 사용자가 직접 COMMIT 과 ROLLBACK 을 하도록 하는 모드

#### Auto

- 하나의 명령어가 성공적으로 수행되면 자동으로 COMMIT

프로그래밍 언어에서 데이터베이스 연걸하거나 접속 도구 등에서  
데이터베이스 서버 접속해 작업 하는 경우 **Auto 설정 경우 있음**

- 트랜잭션 이용하려면 manual 로 설정해야 하는 필요성 있음

**` 실제 서비스에서는 Service 단위로 트랜잭션을 구현해야 함`**

### 트랜잭션 생성 과 종료

- 생성  
  DML 문장이 성공적으로 완료되면 생성
- 종료  
  COMMIT 이나 ROLLBACK 을 수행한 경우

### AUTO COMMIT

DDL 이나 DCL 문장

- CREATE, ALTER, DROP, TRUNCATE  
  DCL 문장을 수행
- GRANT, REVOKE
  접속 프로그램을 정상적으로 종료한 경우

자동으로 COMMIT

### AUTO ROLLBACK

접속이 비정상적으로 종료된 경우

자동으로 ROLLBACK

---

## 6. LOCK

### Shared LOCK

공유 가능한 LOCK

- 읽기 작업을 할 때 설정

### Exclusive LOCK

공유 불가능한 LOCK

- 이외에 작업에서 설정

> 트랜잭션이 종료되어야만 LOCK이 해제

- Manual transaction mode 일 때  
  DML 작업 수행 후 COMMIT, ROLLBACK 없이 다른 컴퓨터에서 SELECT은 가능  
  DML 작업이나 DDL 작업 수행하게 되면 무한 루프 발생
- LOCK 의 기본 단위는 테이블

`Back Log 문제가 발생할 수 있는데, 정상적으로 요청이 완료되지 않으면 토큰 미반납`

---

# 테이블 이외의 객체

VIEW 나 PROCEDURE, TRIGGER, INDEX 가 데이터베이스 사용 성능을 향상 시키기 위한 개체인데  
최근의 프로그래밍에서는 **IN MEMORY DB** 개념의 형태를 사용하기 때문에 이 개체 사용 이점이 별로 없음

- 예전  
  애플리케이션 서버의 요청에 대해 데이터 서버의 결과 응답
- 현재(IN MEMORY DB)  
  애플리케이션 서버가 부팅될 때 요청을 바로 던져 데이터 서버 값 가져오기
  - 스프링 Bean 같은 느낌

## 1. VIEW

자주 사용하는 SELECT 구문을 하나의 테이블의 형태로 사용하기 위한 개체

### - 장점

- SELECT 구문을 메모리에 적재하기 때문에 속도 향상
- 필요한 부분만 노출하면 되어 보안 향상

### - Inline VIew

- FROM 절에 사용한 SELECT 구문
- SELECT 구문의 리턴값은 하나의 테이블처럼 사용 가능
- `SELECT 구문 결과는 이름이 없어` 반드시 이름 생성
  - 오라클에서는 TOP-N 을 구현해 중요

### - VIEW

```sql
CREATE [OR REPLACE] VIEW 뷰이름
AS
SELECT 구문
[WITH CHECK OPTION]
[WITH READ ONLY]
```

- VIEW 는 ALTER 로 수정이 불가능해 수정 시 OR REPLACE 사용
- VIEW 는 테이블처럼 사용할 수 있기 때문에 읽기 쓰기 모두 가능
- WITH CHECK OPTION  
  뷰를 만들 떄 사용한 조건 과 일치한 데이터만 수정, 삭제, 삽입
- WITH READ ONLY  
  쓰기를 못하게 하는 옵션

### - VIEW 삭제

```sql
DROP VIEW 뷰이름;
```

    // DEPT에서 DEPTNO 30인 데이터 자주 사용

    CREATE VIEW DEPTVIEW
    AS
    SELECT *
    FROM DEPT
    WHERE DEPTNO = 30; ======>
    SELECT * FROM DEPT WHERE DEPTNO = 30; 과 같음

`한 번 컴파일 되면 SQL 이 메모리에 상주하여 빠르게 사용 가능`

- `view 는 SQL을 가지고 있는 것일 뿐, 실제 데이터는 없음`

> 포트폴리오 제작 시 SQL Mapper를 이용한다면 VIEW 사용 권장
