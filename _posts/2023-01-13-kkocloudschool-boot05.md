---
title: "Java Spring Boot JPA의 검색 기능 구현"
excerpt: "Spring Boot와 JPA를 활용해 검색 기능 구현에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot, JPA]

toc: true
toc_sticky: true

date: 2023-01-13
last_modified_at: 2023-01-13

---

### 게시물 등록

- 처리 과정
  - 게시물 등록 링크 클릭해 게시물 등록 화면으로 이동,  
    데이터 입력 후 게시물 등록을 클릭해 등록,  
    이후 목록 화면으로 **리다이렉트**
- BoardController 에 URL 매핑 작업 작성
- templates/board 디렉토리에 게시물 등록을 위한 view 파일 작성
  - 메시지 출력

### 상세보기

- 상세보기 링크 설정
- boardController 클래스에 요청 처리 메서드 구현
- templates/board 에 상세보기를 위한 view 파일 작성

### 수정 과 삭제

- 수정 과 삭제는 상세 보기 이후에 진행
  - 로그인이 있다면,  
    `로그인한 유저와 작성한 유저의 기본키가 같을 때`만 링크 보임
- view 파일에 수정 링크 추가
  - 수정 화면은 별도의 파일이 필요하므로 Controller 요청 처리
    - 삽입 화면과 동일한 화면 출력 / 상세보기 에서 읽기 속성 수정
- controller 클래스에 메서드 처리 요청 추가

## 11. 검색 구현

- `검색은 동적 쿼리가 필요`
  - 필요 값이 계속 변경되기 때문에 파라미터로 처리 어려움

### 동적 쿼리 생성을 위해 Querydsl 사용 설정

- springboot 버전에 따라 설정 변경
  - dependencies 수정
- 검색을 위한 메서드를 소유할 Repository 인터페이스 생성
  - implements 클래스의 메서드 구현

```
# JPA에서 쿼리 실행
- JPARepository 가 제공하는 기본 메서드 사용
  - 테이블 전체 조회, 테이블 기본키로 데이터 조회, 삽입
    기본키 이용해 조건 만들어 수정, 기본키로 삭제,
    Entity 로 삭제
- Query method
  - Repository 인터페이스에 하나의 테이블에 대한 검색 및 삭제
    메서드를 이름으로 생성
- @Query를 이용한 JPQL / Native SQL
- Querydsl
  - JPQL을 문자열이 아니라 Java 코드로 작성
    // 문자열을 사용하지 않으므로 컴파일 시 오류 확인 가능
    //조건문을 이용한 동적 쿼리 생성 가능
    (상황에 따라 커럶이름이나 테이블이름이 변경되는 쿼리)
```

- Boardrepository 인터페이스 변경
  - spring 은 `상속과 인터페이스 모두 개별 클래스`를 만들어서 가져옴  
    ~Impl로 사용자 정의해놓으면 이걸 사용하게 됨
- join 을 수행해주는 메서드로 left Join이 있는데,  
  on 메서드에 조인 조건을 작성

### Board 와 Reply 를 Join

```java
QBoard board = QBoard.board; //join 할 테이블
QReply reply = QReply.reply; //join 할 테이블 2
JPQLQuery<Board>> jpqlQuery = from(board); // 누구 기준
jpqlQuery.leftJoin(reply).on(reply.board.eq(board));
// 동일한 보드 가진 얘들만 찾아오기
```

- 테스트 코드 작성
- 실행한 결과가 List 타입의 list
  - 안쪽 List 의 첫번째 요소가 board의 Board 객체  
    두번째는 member.email에 해당하는 문자열  
    세번째는 reply.count()에 해당하는 데이터
- **Tuple**  
  관계형 데이터베이스에서 `하나의 행(row)를 Tuple`이라고 함
  - `여러 개의 데이터를 묶어서 하나의 데이터 표현하는 자료형`  
    Struct(구조체) 가 가장 유사한 형태
    - spring 에서 제공, Java는 Tuple 없음

### 검색 구현

- SearachBoardRepository 인터페이스에 메서드 선언
  - `Pageable과 검색 항목을 매개변수로 해서 페이지 단위로 조회`
- RepositoryImpl 클래스에 구현
- PageRequestDTO 에 검색 속성이 존재하는지 확인
- 페이지번호 나오는 항목 모두 수정

## 12. 댓글 처리

- 댓글 작업은 RestController 이용
  ```java
  방식          url            파라미터      반환 데이터
  GET  /replies/board/{bno} 게시글 번호 댓글배열/댓글 포함 객체
  POST /replies             JSON 문자열 추가된 댓글 번호(객체)
  DELETE /replies/{rno}     댓글번호    삭제 댓글 번호(객체)
  PUT /replies/{rno}        댓글번호+내용 수정댓글번호(객체)
  ```

### ReplyRepository 인터페이스에 메서드 선언

- 테스트 메서드 처리

### 댓글 서비스 와 Controller 그리고 View 사이의 데이터 전달 DTO

### 댓글 요청 처리 Service 인터페이스 생성
