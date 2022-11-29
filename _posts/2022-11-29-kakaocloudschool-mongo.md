---
title: ""
excerpt: "Node에서 MariaDB를 연동하고
테이블 생성, 컬럼 생성 등의 작업을 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, node, mariadb]

toc: true
toc_sticky: true

date: 2022-11-29
last_modified_at: 2022-11-29
---

## 2. MongoDB

도큐먼트 지향 No SQL  
JSON 형식의 BSON 이라는 데이터 구조를 사용

- 스키미 자주 변경
- 비정형 데이터 저장
- 분산 컴퓨터 환경(샤딩 과 복제) 에서 주로 사용

### 1 설치 및 다운로드

### 2 Server 실행

포트 미설정 시 27017 포트 사용

cmd 창에서 mongod --dbpath 경로 저장 후 사용 가능

- mongod 명렁은 mongo db 설치 경로에 존재

### 3 외부 접속 허용

mongod.conf 파일이나 mongod.cfg 파일 수정

- bindIp 부분을 0.0.0.0으로 설정해야 모든 곳에서 접속 가능

```javascript
//설정 파일 변경
서버 실행 시
  mongod --dbpath 데이터저장경로 --bind_ip 0.0.0.0 으로 실행
```

### 4 접속 프로그램

compass

## 3. Mongo DB 구성 요소

database > collection > document  
관계형 데이터베이스(RDBMS) 와 비교

- Database <-> Database
- Table(Relation) <-> Collection
- Row(Record, Tuple) <-> Document
- Column(Attribute) <-> Field
- Index <-> Index
- Join <-> Embedding & Linking

조회의 결과가 관계형 데이터베이스는 Row, Mongo 는 Cursor

## 4. Mongo DB CRUD

JSON 형식

- 객체  
  {"속성 이름":값,"속성 이름":값...}
- 배열
  [데이터, 데이터,..]

값에 문자열, 숫자, boolean, 날짜, null 이 올 수 있고 다른 객체, 배열도 가능  
데이터 도 모든 종류의 값이 모두 올 수 있다.

### 1) 데이터베이스 작업

Mongo DB 에서 가장 큰 저장소의 개념

```javascript
// 확인
  show dbs;
// 생성
  create로 할 수 있지만, use 데이터베이스이름 으로 바로 사용
  // 데이터가 없으면 데이터베이스 출력 되지 않음
```

```javascript
db.컬렉션이름.insertOne(); // 데이터 삽입
```

### 2) 컬렉션

데이터의 집합  
관계형 DB 의 테이블 과 유사하지만, 모든 종류의 데이터 저장 가능

- 실제로는 동일 모양 데이터 저장

```javascript
db.createCollection("이름"); // collection 생성
// 처음 사용시 자동 생성
show collections // collection 확인

db.컬렉션이름.drop() // collection 제거

db.컬렉션이름.renameCollection(변경이름) // 이름 변경

```

#### Capped Collection

크기를 제한해서 생성할 수 있는 컬렉션

- 크기보다 많은 양의 데이터 저장시 오래된 데이터부터 삭제 후 저장

```javascript
db.createCollection(컬렉션이름, { capped: true, size: 크기 });

db.컬렉션이름.find(); // 컬렉션 데이터 확인
// 반복문으로 컬렉션에 데이터 삽입 가능
for (;;) {
  db.컬렉션이름insertOne({ x: 값 });
}
```

- 크기 단위는 byte

### 3) Document 생성, 데이터 삽입

데이터는 객체 형태로 삽입

`_id`라는 속성을 설정하지 않으면 ObjectId 타입으로 \_id 값을 생성 후 삽입

- primary key 이면서 Index

삽입 시 insert, inserOne, inserMany, save 함수 이용

- insert  
  현재 버전에서 지양
- insertOne  
  하나의 데이터 대입
- insertMany  
  배열의 형태로 대입

관계형 데이터베이스는 `테이블의 컬럼 안에 하나의 값(자료형)`만 가능

- 다른 테이블의 데이터나 배열 대입 불가
- 여러 테이블을 만들어, JOIN을 통해 여러 정보 사용 가능

root 가 배열인 경우 데이터를 분할해서 삽입

```javascript
db.컬렉션이름.insert({:},{:});
db.컬렉션이름.find(); // 분할해서 데이터 대입됌
```

- 데이터 삽입 시 두번째 매개변수 ordered:true 로 설정 시 싱글 스레드
  - 중간에 삽입 실패를 하면 다음 작업 수행하지 않음
- ordered:false 로 설정 시 멀티 스레드
  - 중간에 작업이 실패하더라도 나머지 작업 수행
  - `스레드는 다른 스레드에 영향을 주지 않음`
  - ( { [ { ... } , { ... } ] , { ordered : false } )

insertOne 은 하나의 데이터 삽입, inserMany 는 여러 개의 데이터 삽입

#### ObjectId

- MongoDB의 자료형으로 12byte 로 구성
- 컬렉션에 기본 키를 생성하기 위해 제공
- 삽입 시 \_id 라는 컬럼에 자동 대입
  - 직접 생성은 new ObjectId() 로 인스턴스 생성 해 가능

### 4) 조회

```javascript
db.컬렉션이름.find(<query>, <projection>)
```

- selection  
  SELECTION 구문에서 WHERE 절에 해당  
  조건으로 테이블을 수평 분할하기 위한 연산
- projection  
  SELECT 구문에서 SELECT 절에 해당  
  컬럼 이름으로 테이블을 수직 분할하기 위한 연산

```javascript
db.컬렉션이름.find(); // 전체 조회
db.컬렉션이름.find({ 조건1, 조건2,... }); // 조건 조회

db.컬렉션이름.find({}, {속성이름:true || false})// 컬럼 추출
// true 설정시에만 출력
```

비교 연산자

- $eq: =
- $ne: !=
- $gt: >
- $gte: >=
- $lt: <
- $lte: <>=
- $in: in
- $nin: not in

```javascript
db.컬렉션이름.find({ 키: { $eq: "값" } });
db.컬렉션이름.find({ 키: { $ge: "값" } });
db.컬렉션이름.find({ 키: { $in: "[값]" } });
```

- `조회할 때 문자열 자리에 정규 표현식 사용 가능`

논리 연산자

- $not
- $or
- $and
- $nor  
  not을 제외하고는 조건을 `배열의 형태`로 설정

```javascript
db.inventory.find({ $or: [{ qty: { $gt: 100 } }, { qty: { $lt: 10 } }] });
```

문자열

- `값에 정규식 사용 가능`

데이터 개수 제한

- limit( )
- 1개 조회시에는 limit(1), findOne( )

건너뛰기

- skip( )

데이터 정렬

- sort( )

```javascript
sort({ 속성이름: 1 || -1 });
// 1은 오름차순
// -1은 내림차순
```

- 속성 이름 대신 natural 사용시 삽입 순서대로 조회 가능

**`Cursor`**  
도큐먼트를 순서대로 접근할 수 있도록 해주는 포인터

- find 함수의 결과로 리턴되는 자료형
- Enumeration(Enumerator) 나 Iterator 와 유사한 개념
- hasNext( )
  - 다음 데이터의 존재 여부 리턴
- next( )
  - 다음 데이터 리턴

### 5) 수정

- update
- updateOne
- updateMany
- replaceOne

```javascript
update({조건}, {수정할형식});
// 수정형식
{$set:{속성이름:수정데이터,...}}
```

### 6) 데이터 삭제

- remove
- deleteOne
- deleteMany

```javascript
db.컬렉션이름.deleteOne({ 속성이름: 값 }); // 항목 하나 삭제
db.컬렉션이름.deleteMany({}); // 전체삭제
```

### 7) 트랜잭션 처리

Mongo DB 는 느슨한 트랜잭션 지원

```javascript
session = db.getMongo().startSession();
session.startTransaction()

// 작업 수행
session.commitTransaction() 호출
session.abortTransaction() 호출
```

## 5. Node + MongoDB

```javascript
let MongoClient = require("mongodb").MongoClient;
MongoClient.connect(
  "mongodb://아이디:비밀번호@ip:포트번호",
  (err, database) => {
    if (err) {
    } else {
      let db = database.db("데이터베이스 이름");
      // db 작업
    }
  }
);
```

### 샘플 데이터 작성 및 프로젝트 생성

### 전체 페이지 확인

```javascript
MongoClient.connect(databaseUrl, { useNewUrlParser: true }, (err, database) => {
  if (err) {
    console.log(err);
    res.json({ result: false, message: err.message });
  } else {
    let db = database.db("node");
    db.collection("item")
      .find()
      .sort({ itemid: -1 })
      .toArray((err, items) => {
        if (err) {
          console.log(err);
          res.json({ result: false, message: err.message });
        } else {
          res.json({ result: true, list: items, count: items.length });
        }
      });
  }
});
```

### 일부 페이지 확인

### 상세 페이지 확인

### 데이터 삽입

### 수정

```javascript
.update({속성이름:값},{$set:{속성이름:수정할값,...}},(err,result)=>{})
```

### 삭제

```javascript
.deleteOne({속성이름:값},(err,result)=>{})
```

`MongoDB는 숫자 와 문자열을 명확하게 구분`
