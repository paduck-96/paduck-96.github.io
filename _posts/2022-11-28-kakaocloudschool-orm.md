---
title: "Node.js에서 데이터베이스 ORM"
excerpt: "Node.js의 데이터베이스 ORM인 Sequelize에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, node, ORM, database, Sequelize]

toc: true
toc_sticky: true

date: 2022-11-28
last_modified_at: 2022-11-29
---

# SPA 구현 방법

### `각 콘텐츠에 해당하는 별도의 html 파일`을 만들고 이를 불러들이는 방식

- 하나의 HTML 파일에 스크립트를 이용해서 여러 개의 콘텐츠 출력 방식 X
- 이렇게 만들어진 콘텐츠에 해당하는 파일 => **컴포넌트**

# 프로그래밍 언어에서 관계형 데이터베이스 사용 방법

## 1. 데이터베이스 드라이버만 이용

소스 코드 안에 SQL을 삽입해서 작업하는 방식

- 유지보수가 어려움

## 2. SQL Mapper 방식

소스 코드와 SQL 분리해서 작성

- 사용성이 좋아 여러 명의 공동 작업에 사용하나 성능 떨어짐
- MyBatis 가 가장 대표적인 프레임워크

## 3. ORM

관계형 데이터베이스의 `테이블을 클래스`, 테이블의 `행을 인스턴스`와 매핑해 사용

- 성능이 좋아 솔루션 개발에 주로 이용
- JPA(Hibernate 로 구현 많음), Sequelize, Django 등
- 데이터베이스 변경할 때 설정만 변경하면 완료

# Node_ORM

## 1. ORM(Object Relational Mapping)

객체 지향 패러다임을 관계형 데이터베이스에 적용하는 기술

관계형 데이터베이스의 Table은 객체 지향 프로그래밍의 클래스와 유사

- Table 에서는 여러 개의 컬럼
- Class 에서는 속성
  - 메서드 추가 가능 유무
- `Instance 와 Row 가 유사`, Instance 를 이용해 DB 작업 하게 만든 프레임워크
  - Instance 를 가지고 작업하면 프레임워크가 SQL로 변경해 DB 작업 수행
- node 는 sequelize 모듈

## 2. 연동

### 1)패키지 설치

```javascript
sequelize, sequelize - cli, mysql2;
```

### 2)sequelize 초기화

```javascript
npx sequelize init
// config, migration, models, seeders 디렉토리 생성
```

#### **`config`**

데이터베이스 **접속 정보** 저장

#### **`models`**

각 테이블 과 **매핑되는 클래스** 설정

#### migrations

데이터베이스 스키마(구조, 테이블) 변경되는 경우 설정

#### seeders

테스트 데이터 사용 설정

## 3. 데이터베이스 접속 설정

### 1)config 디렉토리의 config.json 파일 수정

```javascript
"development": {
  "username": "root", //계정
  "password": null, //비밀번호
  "database": "database_development", //DB 이름
  "host": "127.0.0.1", //아이피
  "dialect": "mysql" //DB 종류
  ...port 항목 추가 가능
},
```

### 2)models의 index.js 수정

### 3)데이터베이스 연결 코드 작성 후 실행

테이블 연동

- `테이블 먼저 만들고 연결 가능`

모델을 만들고 처음 실행하면 테이블 없을 시 자동 생성

- 테이블이 이미 존재하면 존재하는 테이블과 연결
- 실무에서는 테이블 > 모델 순서  
  학습에서는 모델>테이블 순서

`MySQL 이나 Maria DB 는 Snake 표기법을 따르니 대소문자 유의`

### 4)연동 모델 생성

테이블 과 연결할 모델 생성

- Sequelize.Model 을 상속받은 클래스 생성
  - 2개의 메서드 오버라이딩
  ```java
  static init 메서드
  현재 테이블의 대한 설정
  //
  static associate 메서드
  다른 테이블 과의 관계 설정
  ```
  - `static init 메서드( , )`
    - **컬럼에 대한 설정**
      - 자료형 매핑  
        VARCHAR <-> STRING  
        TINYINT(1) <-> BOOLEAN  
        INT, INTEGER <-> INTEGER  
        DATETIME <-> DATE  
        DATE <-> DATEONLY
        - 그 외 나열되지 않은 자료형은 둘 다 동일
      - 제약 조건  
        allowNull  
        unique  
        defaultValue  
        validate
    - **테이블에 대한 설정**
      - timestamps  
        TRUE일 경우 createdAt, updatedAt 컬럼 자동 생성  
        `데이터 생성 날짜 와 수정 날짜 자동 삽입`
      - underscored  
        기본적으로 이름은 Camel case 이나 Snake case로 변경할건지
      - modelName  
        노드 프로젝트에서 사용할 모델 이름
      - tableName  
        데이터베이스의 테이블 이름
      - paranoid  
        `데이터 삭제 시 삭제하지 않고` deletedAt 컬럼 생성  
        컬럼 값 true로 변경하고 조회할 때 제외
      - charset 과 collate  
        캐릭터 셋으로 `한글`은 utf8 이나 utf8_general 로 설정  
        `이모티콘`까지 사용은 utf8mb4 나 uf8mb4_general_ci 설정
  - `static associate 메서드( , )`  
    자신의모델이름.hasMany 나 belongsTo 를 호출  
    `일 대 다 관계의 키 참조 방식`
    - hasMany  
      부모 테이블로, **외래키의 참조 대상**
    - belongsTo  
      외래키로 소유한 경우
      - ( 상대방모델이름,  
        {foreignKey:"외래키이름" , targetKey:"참조 속성"} )

#### 모델을 이용한 쿼리 메서드

```sql
삽입
  create
조회
  findOne
  findAll
수정
  update
삭제
  delete
```

### 5)models 디렉토리에 기존 테이블 과 연동할 모델 파일 추가 생성

### 6)models 디렉토리의 초기 설정 파일에 생성한 모델을 사용할 수 있도록 추가 설정

### 7) Good(모델) 사용할 설정 추가

### 8) Sequalize를 통한 CRUD 작업

`서버에서 이루어지는 동작은 비동기적으로 처리 되어야 함`

`connection.query 즉, DB 와 직접 연결했던 부분을 치환`

`try ... catch 문으로 항상 작업에 대한 오류 처리 진행`

### 9) 메서드 리턴

- **검색**  
  검색 결과 리턴
- **삽입, 삭제, 갱신**  
  삽입, 삭제, 갱신된 데이터가 리턴

### 데이터베이스 2개 연결

#### 프로젝트 생성 후 패키지 설치

express, sequelize, sequelize-cli, mysql2, nodemon 설치

#### sequelize `초기화`

```javascript
npx sequelize init
```

#### `config.json` 파일에 db 정보 저장

#### `models` 의 index.js 파일 수정

- 양식 고정으로 반복 작성 불필요

#### `app.js 로 설정` 사용

#### `테이블 설계`

**일 대 다** 관계

- user 테이블

  - id 정수, 기본키
  - name 문자열(20), not null
  - age 정수, not null
  - created_at 날짜
  - updated_at 날짜

- comments 테이블
  - id 정수
  - comment 문자열(100), not null
  - created_at 날짜
  - updated_at 날짜
  - commenter 정수, user id 참조하는 외래키

#### 테이블 설계

models 디렉토리에서 테이블 model 생성

- comment 와 user

#### index.js 에 model을 통합하고 외부에서 사용 가능하게 변경

#### app.js 에서 각각의 모델을 import 해 사용
