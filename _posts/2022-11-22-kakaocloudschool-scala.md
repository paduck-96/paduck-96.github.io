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
