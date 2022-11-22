---
title: "Scala Function"
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop]

toc: true
toc_sticky: true

date: 2022-11-22
last_modified_at: 2022-11-22
---

# Scala Function

## 하나의 데이터를 받아서 하나의 데이터를 리턴하는 함수

컬럼을 데이터로 제공하면 각 컬럼 데이터 단위로 작업 후 결과를 하나의 컬럼으로 리턴

### 1. 수치 함수

숫자 연산 과 관련된 함수로 올림, 버림, 반올림 등 제공

- 데이터는 숫자 데이터
  FROM 절을 제외한 곳에서 사용 가능

### 2. 문자열 함수

- CONCAT  
  문자열 결합
- UPPER, LOWER
- LTRIM, RTRIM, TRIM
- SUBSTRING
- LENGTH

### 3. 날짜 관련 함수

#### 현재 날짜 및 시간

- CURRENT_DATE, CURDATE()  
  현재 날짜
- CURRENT_TIME(), CURTIME()  
  현재 날짜
- NOW(), LOCALTIME(), LOCALTIMESTAMP(), CURRENT_TIMESTAMP()  
  현재 날짜 및 시간

#### 날짜 및 시간 연산 함수

- ADDDATE, SUBDATE, ADDTIME, SUBTIME

#### 특정 날짜 생성

- STR_TO_DATE(문자열, 서식)  
  서식에 맞춰서 문자열을 날짜 형태로 변환

  - 1986-05-5 11:00:00 %Y-%M-%d %H:%i:%S

  > MySQL 이나 MariaDB 는 일반적인 날짜 포맷 문자열도 날짜로 간주

### 4. 시스템 정보 함수

- ROW_COUNT()
- USER()
- DATABASE()

### 5. 타입 변환 함수

- CAST(데이터 AS 자료형)
  - 자료형  
    DATETIME, DATE, TIME, CHAR(VARCHAR, TEXT), INT, DOUBLE, BINARY

### 6. NULL 관련 함수

- IFNULL(데이터1, 데이터2)  
  데이터1이 NULL 이 아니면 데이터1을 리턴하고,  
  데이터1이 NULL이면 데이터2 리턴
- NULLIF(데이터1, 데이터2)  
  두 개의 데이터가 같으면 NULL을 리턴하고,  
  그렇지 않으면 데이터1을 리턴

- COALESCE(데이터 나열)  
  나열된 데이터 중 NULL 이 아닌 첫 번째 데이터 리턴

  `데이터베이스에서는 NULL 과 연산 하면 결과는 NULL`

### 7. 분기 관련 함수

- IF
- CASE 데이터 WHEN 값 THEN 결과  
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ELSE 결과

# GROUPING

### 그룹화 관련된 기능

## 1. 그룹 함수

- COUNT  
  개수

- SUM  
  합계

- AVG  
  평균

- MAX  
  최대값

- MIN  
  최소값

- STDDEV  
  표준편차

- VARIANCE  
  분산

## 2. 특징

`NULL 은 제외하고 연산`  
COUNT를 제외한 모든 함수는 컬럼 이름 이나 연산식을 대입해야 함

- COUNT는 \* 이 가능하여,  
  모든 컬럼이 NULL 인 경우 제외하고 데이터 개수 계산 가능

`SUM 과 AVG, STDDEV, VARIANCE 는 문자열에 사용 불가`

**GROUP BY 이후**부터 사용 가능

- HAVING, SELECT, ORDER(실제 사용 필요성 낮음) 사용 가능

SELECT 절에 사용 시 별명과 함께 사용

## 3. 그룹 함수에서의 NULL 제외하고 연산

일반적으로 평균을 구할 때  
**AVG(컬럼)** 과 **SUM(컬럼)/COUNT(컬럼 | \*)** 로 할 수 있는데  
데이터가 NULL 인 경우 0으로 간주하는 COUNT 로 인해 분모 차이 발생

## 4. GROUPING

SELECT 구문에서 데이터를 그룹화 하고자 할 때 사용

WHERE 절 다음에 수행

- WHERE 절 전에는 사용 불가
- 그룹화 항목을 제외하고 출력하면 `데이터 가시성이 떨어져 항목과 같이 조회`
- 그룹화는 여러개 가능
- 그룹화 후 SELECT 절에서 **해당 내용이 아닌 일반 데이터** 조회
  - ORCALE 은 에러, MySQL 은 그룹화 한 항목 중 첫 번째 데이터 조회

## 5. HAVING

GROUP BY 이후의 조건을 설정해서 행 단위로 추출

`그룹 함수는 WHERE 절에서 사용 불가능하기 때문에 따로 조건절 필요`

`데이터를 필터링 할 때는 할 수 있으면 최대한 빨리 하는 것이 좋음`

- 불필요한 데이터도 필터링 될 수 있기 때문
- **그룹 함수를 이용한 조건**이 아니라면 WHERE 에 작성

## 6. SELECT 구문

### 5 - SELECT

### 1 - FROM

### 2 - WHERE

### 3 - GROUP BY

### 4 - HAVING

### 6 - ORDER BY

### 7 - LIMIT

---

# Window 함수

행 과 행 사이의 관계를 표현하기 위한 함수

### 1. 순위나 누적합 등의 연산을 위한 함수

- 순위 함수  
  동등한 값일 때 어떤 식으로 처리하느냐에 따라 여러 함수를 제공하고  
  N 등분한 그룹도 제공해주는 함수
  - RANK, DENSE_RANK, ROW_NUMBER, NTILE 제공
  - 함수 뒤에 OVER 를 이용해 순위 구하는 방법을 ORDER BY로 제공

### 2. JSON 출력

데이터를 조회할 때 JSON_OBJECT 로 감싸면 JSON 문자열 리턴

# SET Operator

## 1. 개요

### 동일한 테이블 구조를 가진 2개의 테이블을 가지고 수행하는 연산

- 컬럼의 **개수** 같아야 하고, 컬럼의 **자료형** 일치
- 컬럼의 이름이나 테이블의 이름은 아무런 상관이 없음

### UNION, UNION ALL, INTERSECT, EXCEPT(MINUS 인 데이터베이스도 있음)

    - UNION 은 중복 제거, UNION ALL 은 중복 미제거

- 컬럼의 이름은 첫번째 테이블의 컬럼 이름을 사용
- **ORDER BY** 는 마지막에 한 번 작성
- `데이터의 자료형이 BLOB, CLOB, BFILE, LONG 이면 안 됌`
  - 사이즈가 너무 커서 일치 여부 판단에 시간 많이 소요
  - INDEX 생성하지 않는 자료형

`데이터가 분산되어 있는 경우 연산 결과 합칠 때도 사용`

## 2. 형식

```sql
SELECT
FROM
...
SET 연산자

SELECT
FROM
```

# SubQuery

## 1. 개요

다른 SQL 구문 안에 포함된 쿼리

- Sub Query 는 SELECT 구문
- Sub Query 는 반드시 괄호 안에 작성
- Sub Query 는 포함하는 Query 가 실행되기 전에 **한 번만** 실행

## 2. 분류

위치에 따른 분류

- FROM 절이 아닌 경우  
  Sub query
- FROM 절에 쓰인 경우  
  Inline View

리턴 되는 데이터에 따른 분류

- 단일행 sub query  
  리턴되는 결과가 하나의 행
- 다중행 sub query  
  리턴되는 결과가 2개 이상의 행

> 하나씩 처리 하는 방법과 sub query 사용이 모두 가능한 경우 있음

## 3. 다중 열 Sub Query

sub query 의 결과가 1개의 컬럼이 아니고 여러 개의 컬럼인 경우

## 4. 다중 행 sub query

sub query 의 결과가 2개 이상의 행인 경우

- `=, <> 는 사용이 안되고 >, >=, <, <= 도 단독 사용 불가`
  - 단일행 연산자  
    =, <> 는 하나의 데이터와만 비교 가능
  - IN 이나 NOT IN 그리고 ANY와 ALL 같은 다중 행 연산자 사용
    - ANY 와 ALL 은 MAX 와 MIN 으로 대체 가능

# JOIN

## 0. 개요

2개 이상의 테이블을 합쳐서 하나의 테이블을 만드는 것

- 2개의 테이블이 동일한 테이블일 수도 있음

조회하고자 하는 데이터가 2개 이상의 테이블에 나누어져 있거나 하나의 테이블에서 2번 이상 찾아야 하는 경우에 사용

## 0. 종류

- Cartesian Product  
  Cross join, 단순하게 2개 테이블의 모든 조합 생성

- Equi join  
  Inner join, 양쪽 테이블에 동일한 의미를 갖는 칼럼이 존재할 때 2개의 컬럼 값이 일치하는 경우에만 결합
- Non Equi join  
  2개 컬럼의 값이 일치하지 않는 조건으로 결합
- Outer join  
  한 쪽 테이블에만 존재하는 데이터도 join 에 참여
- Self join  
  동일한 테이블끼리 join 하는 것으로 하나의 테이블에 동일한 의미를 갖는 컬럼이 2개 이상 존재할 때
- Semi Join  
  Sub Query를 이용해서 join

## 1. Cross join - Cartesian Product

### `양쪽 테이블의 모든 데이터 조합을 만들어내는 것`

컬럼의 수

- 양쪽 테이블의 컬럼 수 합

행의 수

- 양쪽 행의 수의 곱

```sql
SELECT *
FROM 테이블 이름, 테이블이름;
```

## 2. Equi Join

### `양쪽 테이블의 동일한 의미를 갖는 컬럼 값이 일치하는 경우에 join`

동일한 의미를 갖는 컬럼이 **Foregin Key**이면 join 성능 향상

```sql
-- WHERE 절에 join 조건 기재
SELECT *
FROM 테이블이름, 테이블이름
WHERE 조건
```

- 양쪽 테이블에 동일한 이름의 컬럼 있으면 중복으로 **앞에 테이블 이름 명시 필수**
  - 테이블이름.컬럼

`Join 을 한 후 조건을 가지고 데이터를 조회하는 경우 JOIN 조건 먼저 기재`

- 조건이 복잡하고, 조회할 컬럼이 하나의 테이블에 있다면 Sub Query
- 동일한 문제를 Sub Query 로 해결되면 **그대로 사용**

### **`관계형 DB의 단점은 JOIN 이 많다는 것!`**

```sql
SELECT 컬럼, 컬럼
FROM 테이블, 테이블
WHERE 테이블.컬럼 = 테이블.컬럼 AND 조건;
```

JOIN 테이블의 순서

- 선행 테이블에 조건을 설정해 데이터 추출 후 후행 테이블의 데이터를 결합

  - 조건 확인 후 `데이터 추출 개수가 적은 테이블`을 선행으로 사용
  - 한쪽에만 적용된다면 조건 적용되는 테이블 먼저 기재

## 3. Non Equi Join

### join 조건이 등호가 아닌 경우

`다른 테이블의 데이터와 비교할 때 \= 연산자가 아닌 것으로 비교하는 것`

```sql
select 컬럼, 컬럼
from 조건해당 테이블이름, 테이블이름
where 컬럼 BETWEEN  AND ;
```

## 4. SELF JOIN

### 동일한 테이블을 가지고 JOIN

- 하나의 테이블에 동일한 의미를 갖는 칼럼이 2개 이상 존재할 때 사용
  - sns 나 인력배치 와 같은 인사 관리 시스템에서 주로 이용
  - `2개의 동일한 테이블 사용하기 때문에 반드시 테이블 이름 변경`

## 5. ANSI(미국 표준 협회) JOIN

cross join 을 할 때 FROM 절에 CROSS JOIN 기재

```sql
SELECT *
FROM 테이블 CROSS JOIN 테이블;
```

Equi join 대신에 INNER JOIN 이라는 표현을 사용하고  
 WHERE 절 앞에 on 추가하고 JOIN 조건 기재

```sql
SELECT *
FROM 테이블 INNER JOIN 테이블
ON 테이블.컬럼 = 테이블.컬럼;
```

- 양쪽 테이블 컬럼 이름 동일할 경우 ON 절 대신 USING(컬러이름) 가능
- 양쪽 테이블의 동일한 컬럼은 한 번만 조회

- 양쪽 테이블의 컬럼 이름 동일 시, USING 생략, INNER JOIN 대신 NATURAL JOIN

## 6. OUTER_JOIN

한 쪽 데이블에만 존재하는 데이터도 json에 참여

- Maria Db 에서는 Left ,right join만 지원
- NULL OUter Join 은 자몬 소모 심하면 패스
- 테이블의 존재하지 않는 데이터가 10,20,30

다른 테이블에 존재하지 않아도 데이터는  
 그 테이블의 모든 칼럽 값을 NULL

0
---`
from EMP RIGHT OUTER JOIN Dept
ON EMP.DEPETO = DEOT.DEPTTO

## 7. 다중 조인

#### 3개 상의 테이블

JOIN 가능

```

- 2개 테이블 join 하고 다른 케이블가 도메인 과 JS offie
```

!!! 잠 !!!!
