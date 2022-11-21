---
title: "MariaDB"
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop]

toc: true
toc_sticky: true

date: 2022-11-21
last_modified_at: 2022-11-21
---

# MariaDB

## 개요

### 1. 정의

SQL에 기반을 둔 RDBMS(관계형 데이터 베이스)로 Open Source 형태로 제공
MySQL 개발자가 만들어서 거의 유사

- SQL도 거의 차이가 없음

### 2. 작업 단위

데이터베이스 > 테이블

- 하나의 데이터베이스를 여러 유저가 공유

---

## 데이터베이스 서버 설치

### 1. OS에 직접 설치

window

- https://mariadb.org/download

`InnoDB vs MyIsam`

- Linked List 와 Array List 의 차이에 기반

### 2. 가상화 컨테이너에 설치

Docker 와 같은 가상화 컨테이너에 설치

- mariaDB image 다운

  - 터미널 에서 docker pull mariadb  
    버전 생략 시 자동 최신 버전
    - `운영체제 설치한 것에 따라 명령어 사용 위치 변경`

- mariaDB 컨테이너 실행

  ```Docker
  docker run --name mariadb -d -p 외부에서접속할포트번호:MariaDB포트번호 -e MYSQL_ROOT_PASSWORD=루트비밀번호 컨테이너이름
  ```

  ###### - docker run --name mariadb -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=rootPassword mariadb

- docker ps 로 컨테이너 실행 여부 확인 가능

  - -a로 존재 컨테이너 모두 확인 가능

- 컨테이너 중지 / (재)시작

  - docker stop 컨테이너이름 이나 아이디
  - docker (re)start 컨테이너이름 이나 아이디

- 컨테이너 삭제
  - docker rm 컨테이너이름 이나 아이디
  - docker rm -f 컨테이너이름 이나 아이디

---

## 데이터베이스 IDE 설치

데이터베이스 접속 도구

- 데이터베이스 콘솔 에서 작업할 경우 `복제 없이 원본에 적용`
  - 관리자 의 경우 콘솔에서 가능할 수 있으나  
    사용자 같은 경우에는 복제 본에 작업 필수
- IDE 없이 `외부 데이터베이스에 접속` 어려움

#### DBeaver

오픈 소스 이고 여러 데이터베이스 접속 가능

- 금융 분야 는 토드 나 오렌지 사용

## 데이터베이스 서버 실행 및 접속 확인

### 데이터베이스 접속 도구에서

```
HOST : localhost(로컬 접속 시)
PORT : 3306(도커 설치 시 변경했으면 수정)
DATABASE:mysql(기본 제공)
USERNAME:root(기본 제공)
PASSWORD:설치 시 사용한 비밀번호
```

---

## 데이터베이스 외부 접속 허용

1. `권한 설정`

```javascript
GRANT all privileges on 사용할DB이름 TO '계정'@'접속할IP;`
 // 모든 데이터베이스 사용 은 *.*
 // 모든 곳에서 접속 시 % 설정
 // 로컬 접속 시 localhost

 //root 계정
 //모든 곳 접속 시
 //모든 데이터베이스 접속 시
 //접속 설정
 GRANT all privileges on *.* TO 'root'@'%'
```

`권한 설정 명령은 설정 후 적용 명령을 수행`

```javascript
FLUSH privileges;
```

2. 서버 설정  
   윈도우 직접 설치 시 해당 과정 생략

- 도커의 경우 **직접 파일 수정 불가**로 컨테이너의 bash 로 접속

```javascript
docker exec -it 컨테이너이름 bash

// 최초 1번 작성
// vim으로 수정
apt update
apt upgrade
apt install vim

vim /etc/mysql/mariadb.conf.d/50-server.cnf
// i > esc > :wq
```

```javascript
/etc/mysql/mariadb.conf.d/50-server.cnf 파일
-> bind-address 부분을 허용할 IP로 변경
// 0.0.0.0 이면 모든 곳에서 접속
```

`실제 서버 설정이라면 Application Server IP 만 허용`

## 이후 컨테이너 재시작

## SQL 작성 규칙

### 1. SQL의 예약어는 대소문자 구분을 하지 않음

2
. **`테이블 이름 이나 컬럼`** 은 대소문자를 구분하는 경우도 있으니 구분`

- MariaDB 는 테이블 이름 대소문자 구분

3 .**값**을 작성할 때는 대소문자 구분 하나, MariaDB 는 하지 않는 경우도 있음

4
. **숫자 데이터**는 따옴표를 하지 않고 문자는 작은 따옴표로 표현

5
. 명령문의 끝은 ; 인데 접속도구에서는 불필요 하나 절처자거 프로그래밍에는 필수  
 프로그래밍 언어에서 SQL을 사용할 때는 ; 하면 안 됌

---

## 데이터 베이스 관련 명령어

### 1. 데이터베이스 생성

```sql
create database 데이터베이스이름;
```

- 주석은 # 으로

이미 존재하는 이름이면 에러

`일반적으로 프로젝트 마다 데이터베이스 생성`

### 2. 데이터베이스 확인

```sql
show databases;
```

### 3. 데이터베이스 사용

```sql
use 데이터베이스이름;
```

MySQL 이나 Maria DB 에서 `SQL을 사용하기 전에 데이터베이스 사용 설정 우선`

- 백업 단위가 **프로젝트** 이기 때문
- 하루 작업 후 **git 에 `올리는 것`** 과 **db `덤프`** 필수

### 4. 데이터베이스 삭제

```sql
drop 데이터베이스이름;
```

### 5. 데이터베이스에 존재하는 테이블 확인

```sql
show tables;
```

---

## SQL 분류

### 1. DDL

구조에 관련된 명령어로 일반적으로 DBA 의 명령어  
`취소 불가능`

- CREATE  
  구조 생성
- ALTER  
  구조 변경
- DROP  
  구조 삭제

- TRUNCATE  
  테이블 내의 데이터 삭제

- RENAME  
  구조 이름 변경

### 2. DQL

검색에 관련된 명령어

- SELECT

### 3. DML

데이터 관련 명령어  
`취소 가능`

- INSERT
- UPDATE
- DELETE

### 4. TCL

트랜잭션 관련 명령어  
`취소 불가능`

- COMMIT  
  현재 작업까지의 내용 원본 반영
- ROLLBACK  
  작업 내용 취소
- SAVEPOINT  
  취소할 지점 생성

### 5. DCL

제어 명령어  
`취소 불가능 하고 운영자의 언어`

- GRANT  
  권한 부여
- REVOKE  
  권한 회수

> DQL > DML > TCL > DDL > DCL 순으로 개발자에게 중요

---

## SELECT

데이터 조회 명령어로 원본에 아무런 영향을 주지 않음  
`원본에서 데이터를 복제해 리턴`

#### (샘플 데이터의 구조)

<pre>
  EMP 테이블
    EMPNO 사원번호, 정수 4자리, 기본키
    ENAME 사원이름, 문자
    JOB 직무, 문자
    MGR 사원번호(관리자)
    HIREDATE 입사일, 날짜
    SAL 급여, 실수 7자리+소수 2자리
    COMM 상여금, 실수 7자리+소수 2자리
    DEPT 부서 번호, 정수 2자리, DEPT 테이블 DEPTNO 참조
  
  DEPT 테이블
    DEPTNO 부서번호, 정수 2자리, 기본키
    DNAME 부서이름, 문자
    LOC 위치, 문자

  SALGRADE 테이블
    GRADE 호봉, 숫자, 기본키
    LOSAL 호봉 최저, 숫자
    HISAL 호봉 최고, 숫자

  Tcity 테이블
    NAME 도시 이름, 문자열, 기본키
    AREA 면적, 정수
    POPU 인구수, 정수
    METRO 대도시 여부, 문자
    REGION 지역, 문자

  TSTAFF 테이블
    NAME 이름, 문자열, 기본키
    DEPART 부서 이름, 문자열
    GENDER 성별, 문자열
    JOINDATE 입사일, 날짜
    GRADE 직무, 문자열
    SALARY 급여, 정수
    SCORE 고과 점수, 실수, 소수 2자리

</pre>

### SELECT

1. Selection  
   테이블의 행을 선택할 때 사용
2. Projection  
   테이블의 열을 선택할 때 사용
3. Join  
   공유 테이블 양쪽의 열에 대해 링크 생성하여 다른 테이블의 데이터를 가져와 합치는 것

--

### Maria DB 에서의 Select 구조

#### 5. SELECT

- 데이터 열 단위 조회를 위한 열 이름이나 계산식 나열

#### 1. FROM

- 데이터를 조회할 테이블 나열

#### 2. [ WHERE

- 데이터 행 단위 분할 조건
  - GROUP BY 에 영향을 받지 않음

#### 3. [ GROUP BY

- 데이터 그룹화시키기 위한 열 이름이나 계산식 나열

#### 4. [ HAVING

- 데이터 행 단위 분할 조건

#### 6. [ ORDER BY

- 데이터 정렬 위한 열 이름이나 계산식, SELECT 절의 번호 와 정렬 방법
  - SELECT 시 테이블 순서 정해져 있지 않기 때문에  
    복수 가져오기에는 이용

#### 7. [ LIMIT

- 데이터 위치 와 개수를 지정해서 가져오기 위한 것으로 표준 아님

`FROM 을 명령하게 되면 원본 DB 에서 테이블 단위로 복제 해 작업 수행`

- 여기`(FROM)` 서 다른 이름을 명명하는 것은 별명이 아닌 이름 변경
  - 동일 데이터 테이블을 여러 번 가져올 때

--

### SELECT 의 기본적인 구조

#### 1. 테이블 모든 데이터 조회

```sql
SELECT *
FROM 테이블이름;
```

- 컬럼의 순서는 테이블 만들 때 작성한 순서
  - 직접 테이블을 생성한 경우가 아니라면 \* 사용은 자제

#### 2. 특정 컬럼만 추출

```sql
SELECT 컬럼 이름 나열
FROM 테이블이름;
```

### SELECT 절에서의 별명

컬럼에 별명 부여 가능

- 하나의 공백을 두고 설정
- 공백 자리에 AS 작성 가능
  - 공백, 특수문자, 대문자가 있으면 **\" \"** 로 묶어야 함

`ORDER BY에서만 사용 가능` 하고 프로그래밍 언어에서도 별명을 가지고 데이터 가져옴

- Select 는 순서 상 5번째고 Order by 가 6번째라서

- 계산식 이나 그룹 함수의 결과를 조회하고자 할 때는 별명 부여

```sql
SELECT name as 이름
FROM 테이블이름;
-- 프로그래밍 언어에서는 이름 으로 밖에 쓸 수 없음
```

--

### 계산식 출력

`FROM 절 제외한 모든 곳에서 사용 가능`

> 계산식은 가상의 컬럼이고 FROM 은 실제 테이블을 가져오기 때문

- 단순 계산식은 FROM 생략 가능

--

### concat 함수

2개 이상의 문자열을 합쳐주는 함수

- 복수의 컬럼 이나 연산식을 하나로 합쳐서 출력하기 위해 사용

> MyBatis 와 같은 SQL Mapper Framework 에서 like를 사용하기 위해

--

### DISTINCT

SELECT 절 맨 앞에 한 번만 기재해 컬럼 중복 값 제거

- 컬럼 이름이 하나일 경우 중복된 값만 제거
- 컬럼 이름이 2개 이상이면 모든 값이 일치할 경우 제거

--

### ORDER BY

조회된 데이터 정렬하기 위한 절

`SELECT 절의 결과 2개 이상일 경우 정렬`

- [ASC | DESC]  
  기본이 오름차순 정렬이고, DESC 가 내림차순

  - 숫자 > 작은 것 에서 큰 것
  - 날짜 > 빠른 것 에서 늦은 것
  - 문자 > 알파벳 순서, 앞글자부터 비교
  - NULL 이 가장 마지막

- 컬럼 대신에 SELECT 절 순서로 설정 가능
- SELECT 절에서의 별명 사용 가능
- 2개 이상의 필드 나열 가능
  - `첫 번째 필드로 정렬 후 동일 값이 있으면 두 번째 필드 조건 확인`
- 계산식을 이용한 정렬 가능
- 정렬 기준 필드를 출력하지 않아도 되나 **비권장**

--

### WHERE

테이블의 데이터를 행 단위로 분할하기 위한 조건 설정 절

#### SELECT, UPDATE, DELETE 구문 과 함께 사용

- 비교 연산자
  - =
  - \>
  - \<
  - \>=, NOT 컬럼이름
  - <=, NOT 컬럼이름
  - <>, !=, ^=, NOT 컬럼이름 =

> 테이블 이름과 달리 컬럼 비교 시 대소문자 구분이 안 되는 경우도 있어 주의

- Boundary Value Analysys(경계값 분석 기법)  
  `경계값 과 경계값 양쪽의 데이터 반드시 테스트`

#### NULL 비교

> 아직 알려지지 않은 값

`NULL 은 일반 연산자로 비교가 안됨`

- IS NULL 과 IS NOT NULL 로 비교

데이터베이스에서 NULL 저장

- 공간에 NULL을 대입하는 개념이 아니고 NULL을 저장할 수 있는 컬럼에는  
  `데이터 저장 공간에 하나의 공간을 추가해서 그 공간에 NULL 여부 표기`

#### 논리 연산자

1. AND  
   두 개의 조건이 모드 일치하는 경우만 조회

   - 앞이 일치하지 않으면 뒤는 확인하지 않음

2. OR
   두 개의 조건 중 하나의 조건만 일치해도 조회

   - 앞의 조건이 일치하면 뒤는 확인하지 않음

> AND 가 OR 보다 우선 순위가 높다

3. NOT

4. **`LIKE`**  
   부분 일치하는 데이터 조회

   - 와일드 카드 문자
     - \_  
       하나의 문자와 매칭
     - %  
       글자 수 상관 없음
     - []  
       문자를 나열하면 문자 중 하나와 일치
     - [^]  
       문자를 나열하면 문자에 포함되지 않는

   `와일드 카드 문자를 검색하고자 하는 경우 ESCAPE 이용`

5. BETWEEN

```sql
BETWEEN A ANd B -- A 부터 B 까지의 데이터 조회
-- 숫자, 날짜, 문자열 모두 사용 가능
```

- 단순 AND 로도 가능

  `문자열의 크기 비교`는 맨 앞 글자부터 **순서대로 하나씩** 비교

#### IN 연산자

```sql
IN (값을 나열) -- 나열된 값에 포함되는 경우 조회
```

### LIMIT

#### 행의 개수 제한에 이용

TOP N

```sql
LIMIT [건너뛸 행의 개수], 조회할 개수
--최근
LIMIT 개수 OFFSET 건너뛸 행의 개수
```

`ORDER BY 와 같이 사용되는 경우가 많음`

### Scala Function

#### 1. Function

데이터베이스에서 함수는 **반드시 리턴**을 해야 함
