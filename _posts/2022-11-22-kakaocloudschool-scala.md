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
