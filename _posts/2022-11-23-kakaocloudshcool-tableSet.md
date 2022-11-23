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

## 3. 테이블 삭제

### 기본 형식

```sql
DROP TABLE 테이블이름;
```

### 테이블 삭제 불가능

**외래키로 참조되는 테이블**은 `외래키를 소유하고 있는 테이블 삭제`가 우선

## 4. 테이블 데이터 삭제

```sql
TRUNCATE TABLE 테이블이름
```

## 5. 테이블 압축

```sql
CREATE TABLE ROW_FORMAT = COMPRESSED
```

저장 공간 줄어드나 작업 속도 느려짐

## 6. 주석 설정

```sql
COMMENT ON TABLE 테이블이름 IS "주석";
```

## 7. 제약 조건(Constraint)

### 1. 무결성 제약 조건

- Entity Integrity(개체 무결성)  
  기본키는 NULL 이거나 중복될 수 없다
- Referential Integrity(참조 무결성)  
  외래키는 참조할 수 있는 값이나 NULL

- Domain Integrity(도메인 무결성)  
  속성의 값은 정해진 도메인의 값을 가져야 한다

### 2. NOT NULL

NULL 일 수 없다 라는 제약 조건

- 필수 입력
- 컬럼 크기와 관련 있어 컬럼 만들 때 제약조건 설정

`테이블 제약 조건으로 생성 불가`

```sql
컬럼이름 자료형 NOT NULL
```

`기본은 NULL 허용`

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

### 4. CHECK

값의 종류 나 범위를 제한하기 위한 제약조건

```sql
CHECK(컬럼이름 조건);
--
GENDER CHAR(3) CHECK(GENDER IN ("남", "여"))
SCORE INTEGER CHECK(SCORE BETWEEN 0 AND 100)
--
```

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

## 9. Sequence(일련번호)

컬럼 이름 뒤에 AUTO_INCREMENT 를 설정하면 일련번호 자동생성

- 생성되면 따로 값 대입 불필요

테이블 생성 시 초기 값 설정 가능

```sql
ALTER TABLE 테이블이름 AUTO_INCREMENT = 값;
```

- PK 나 UK 반드시 설정
- 테이블에서 한 번만 설정 가능

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
