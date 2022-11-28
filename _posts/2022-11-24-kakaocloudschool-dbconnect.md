---
title: "Node에서 MariaDB 연동"
excerpt: "Node에서 MariaDB를 연동하고
테이블 생성, 컬럼 생성 등의 작업을 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, node, mariadb]

toc: true
toc_sticky: true

date: 2022-11-24
last_modified_at: 2022-11-28
---

# Node + Maria DB

## 1. 프로그래밍 언어에서 관계형 데이터베이스 사용 방법

### 데이터베이스 제조업체에서 제공하는 드라이버 사용

### SQL Mapper Framework 사용

SQL을 이용, 사용하기 쉬워 SI 같이 많은 인력 진행 프로젝트에서 이용

### ORM Framework 이용

SQL을 이용하지 않고 객체 지향 언어의 메서드를 이용해 SQL 자동 변환 후 수행

- 성능이 우수하여 적은 인력을 가지고 만드는 솔루션 분야에 이용

## 2. 데이터베이스 제공업체의 드라이버를 이용해 SQL 실행

> Node 에서 Maria DB 는 MySQL 과 같은 데이터베이스로 취급

## 3. MariaDB 연동

### 필요한 모듈

MySQL

### 필요한 정보

연결할 데이터베이스를 소유하고 있는 컴퓨터의 IP 나 도메인 과 포트번호

- localhost(127.0.0.1, ::1):3306

사용할 데이터베이스 이름(sid)

계정 > 아이디 와 비밀번호

---

## 연결

### 연결 코드

```javascript
const mariadb =require("mysql");

//접속정보
let connection = mariadb.createConnection({
  host:"아이피 나 도메인",
  port:포트번호,
  user:"아이디",
  password:"비밀번호",
  database:"데이터베이스 이름"
});

//연결
connection.connect(function(error){
  if(error){
      에러 발생 시 수행 내용
  }
});
데이터베이스 연결 시 수행 내용
```

### 연결 확인

node 프로젝트 생성 후 mysql 패키지 설치

### 연결

에러 메시지 내용 확인 후 수정

---

## SQL 실행

### SELECT 가 아닌 구문

결과가 `성공` 과 `실패` 또는 `영향 받은 개수의 형태`

```javascript
연결객체.query(SQL, [파라미터배열]);
// 파라미터 배열은 SQL을 작성할 때 값의 자리에 직접 작성하지 않고
// ? 로 설정한 후 나중에 값 대입 가능
```

### SELECT 구문

조회한 결과

- `Cursor` 또는 하나의 `객체` 나 `배열`

`프로그래밍 언어에서 SQL 을 작성할 때는 ; 를 하지 않음`

- 콜백 함수를 매개변수로 추가하는데 3개의 매개변수를 가지고 있음
  - 에러 객체
  - 검색된 내용  
    javascript 객체 형태 제공
    - 화면에 그대로 출력
  - meta data  
    검색된 결과에 대한 정보
- 데이터 형태로 제공시 `JSON 문자열 변환`

## sql 연동

### 기능

- 샘플 데이터 삽입
- 패키지 모듈 설치

  ```javascript
  npm install express morgan multer mysql cookie-parser express-session expressmysql-session dotenv compression file-stream-rotator

  npm install --save-dev nodemon //소스 코드 수정 시 자동 재시작
  //--save-dev 설치
  ```

- 서버 업데이트 로그 받아오기
  테이블 데이터 전체 가져오기

테이블 데이터 일부 가져오기(페이지 단위)

기본키로 데이터 1개 가져오기

데이터 삽입, 삭제, 갱신

파일 업로드/다운로드

가장 최근 수정한 데이터 시간 기록 및 조회
