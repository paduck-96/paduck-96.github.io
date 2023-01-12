---
title: "Java Spring Boot JPA 를 통한 JOIN"
excerpt: "Spring Boot JPA 를 활용해 테이블 JOIN 후 이를 다루는 과정에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot, JPA, JOIN]

toc: true
toc_sticky: true

date: 2023-01-12
last_modified_at: 2023-01-12

---
#  1: N 관계
## 1. 관계
### 관계형 데이터베이스는 테이블 간에 관계 설정 가능
### 관계형 데이터베이스의 관계
- 1:1
- 1:N
- N:N(다대다 관계)
  - 1:N 관계 2개로 분할

### JPA에서의 관계
- 종류  
OneToOne  
OneToMany  
ManyToOne  
ManyToMany

- 방향성  
단방향성  
양방향성

### 회원 과 게시글의 관계
- 한 명의 회원은 0개 이상의 게시글을 작성할 수 있음
- 1개의 게시글은 반드시 한 명의 회원이 작성해야 함
  - 회원 과 게시글의 관계는 1:N 관계

  - 이 경우 회원 테이블의 기본키를 게시글 테이블의 외래키로 설정  
JPA에는 `양방향성`을 가질 수 있고 `반대 방향으로 관계 설정`이 가능  
JPA에서는 이 경우 회원 정보를 가져올 때 게시글을 참조하는 경우가 많은지?  
게시글을 가져올 때 회원 정보를 참조하는 경우가 많은지?  
양쪽의 경우가 모두 많은지?  

### 데이터베이스 구조
- 회원 테이블 과 게시글 테이블  
1:N
- 게시글 테이블 과 댓글 테이블  
1:N
- 회원 테이블 과 댓글 테이블  
1:N


## 2. 게시판 프로젝트 설정
### 프로젝트 생성
- 의존성  
Lombok, SpringWeb, Thymeleaf, Spring Data JPA, Maria DB

### application.properties 파일을 application.yml 파일로 이름을 변경하고 기본 설정 작성

### 실행 클래스 상단에 데이터베이스 감시 옵션을 설정

## 3. domain 클래스 작업
### 공통으로 사용할 수정 날짜 와 생성 날짜를 갖는 Entity

### 회원 정보를 저장할 Entity를 생성 
### 게시글 정보를 저장할 Entity를 생성
### 댓글 정보를 저장할 Entity 생성 

## 4. 관계 어노테이션의 세부 속성
### optional
- `필수 여부를 설정하는 것`으로 기본값은 true
- 반드시 존재해야 하는 경우라면 false로 설정
- 이 값이 true이면 데이터를 가져올 때 outer join을 하게되고 false 이면 inner join을 수행
  - inner join  
  양쪽 테이블에 모두 존재하는 데이터만 join에 참여
  - outer join  
  한쪽 테이블에만 존재하는 데이터도 join에 참여

### mappedBy
- 양방향 관계 설정 시 `다른 테이블에 매핑되는 Entity 필드`를 설정

### cascade
- `상태 변화를 전파`시키는 옵션
  - **PERSIST**  
  부모 Entity 가 영속화될 때(저장) 자식 Entity도 같이 영속화
  - **MERGE**  
  부모 Entity 가 병합될 때 자식 Entity도 같이 병합
  - **REMOVE**  
  부모 Entity 가 삭제될 때 자식 Entity도 같이 삭제
  - **REFRESH**  
  부모 Entity 가 refresh 될 때 자식 Entity도 같이 refresh
  - **DETACH**  
  부모 Entity 가 detach 될 때 자식 Entity도 같이 detach 
    - 더 이상 Context 의 관리받지 않도록 하는 것  
    트랜잭션이 종료되더라도 이 객체는 변화가 생기지 않음
  - **ALL**  
  모든 상태를 전이

### orphanRemoval
- JoinColumn의 값이 NULL 인 자식 객체를 삭제하는 옵션

### fetch
- 외래키로 설정된 데이터를 가져오는 시점에 대한 옵션으로 Eager 와 Lazy 설정

## 5. ManyToOne
- 외래키를 소유해야 하는 테이블을 위한 Entity에 설정하는데 @**ManyToOne** 이라고 컬럼 위에 작성
- 테이블의 방향을 반대로 설정할 수 있는데 @**OneToMany**
- @**JoinColumn**을 이용해서 `join 하는 컬럼을 명시적`으로 작성,  
생략시 참조변수이름_참조하는테이블의기본키컬럼이름 으로 생성
- **외래키 값**은 `부모 Entity에서만` 삽입, 수정, 삭제가 가능  
자식 Entity에서는 읽기만 가능

- Board 테이블에 Member 테이블을 참조할 수 있는 관계를 설정
  ```java
    @ManyToOne
    private Member writer;
  ```

- Replay 테이블에 Board 테이블을 참조할 수 있는 관계를 설정
  ```java
    @ManyToOne
    private Board board;
  ```

## 6. Repository 인터페이스 생성

## 7. 샘플 데이터 삽입
- test 디렉토리에 Test 클래스를 생성
- 하나의 테스트 케이스에 여러 @Autowired 어노테이션으로 동시 작업 가능

## 8. Eager/Lazy Loading
- Entity에 관계를 설정하면 데이터를 가져올 때 참조하는 데이터도 같이 가져오게 됨
### 게시글 1개를 가져오는 메서드를 확인
- 실제 실행되는 쿼리
  ```java
    select
        b1_0.bno,
        b1_0.content,
        b1_0.moddate,
        b1_0.regdate,
        b1_0.title,
        w1_0.email,
        w1_0.moddate,
        w1_0.name,
        w1_0.password,
        w1_0.regdate 
    from
        board b1_0 
    left join
        tbl_member w1_0 
            on w1_0.email=b1_0.writer_email 
    where
        b1_0.bno=?
  ```
- join을 수행해서 데이터를 가져옵니다.

### reply 정보 1개를 가져오는 메서드
   
### Lazy Loading
- 지연 로딩
  - `사용할 때 가져오는 것`
- 관계를 설정할 때 **fetch=FetchType.LAZY** 을 추가하면 지연로딩
- 지연 로딩을 사용할 때 주의할 사항
  - toString  
  내부 모든 속성의 toString을 호출해서 그 결과를 가지고 하나의 문자열을 생성
    - 지연로딩을 하게되면 `참조하는 속성의 값이 처음에는 null`  
null.toString이 호출되서 **NullPointerException**이 발생  
이 문제를 해결하기 위해서는 `지연로딩되는 속성은 toString에서 제외`
@**ToString(exclude="제외할 속성이름")**

- 지연로딩을 사용하게 되면 `2개의 select 구문이 수행`  
  - 데이터를 찾아오고, 그 데이터의 외래키를 이용해 참조하는 테이블에서 데이터를 찾아옴
- JPA는 `하나의 메서드에서 하나의 Context 연결`만을 사용하는데,   
`하나의 SQL 구문이 끝나면 Context 와의 연결해제`
  > 2개의 SQL을 실행할 수 없음
  - 이런 경우에는 메서드에 @**Transactional**을 붙여서   
  `메서드의 수행이 종료될 때 까지` 연결 유지

- Board Entity를 수정

- 하나의 게시글을 읽어오는 테스트 메서드 수정
- Reply Entity 수정

## 9. JOIN
### JOIN
- `2개 이상의 테이블`(동일한 테이블 2개도 가능)을 `가로 방향으로 합치는` 작업
- 종류
  - Cross Join(Cartesian Product)  
  `2개 테이블의 모든 조합`을 만들어내는 것  
  `외래키를 설정하지 않았을 때` 수행

  - INNER JOIN (EQUI JOIN)  
양쪽 테이블에 `동일한 의미를 갖는 컬럼의 값이 동일한 경우`에만 결합

  - NON EQUI JOIN  
양쪽 테이블에 `동일한 의미를 갖는 컬럼의 값이 동일한 경우를 제외`한   
방식(>, >=, <, <= 또는 between)으로 결합

  - OUTER JOIN  
`한 쪽 테이블에만 존재하는 데이터도 JOIN에 참여`하는 방식  
방향에 따라서 LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN 이 있음

  - SELF JOIN  
`동일한 테이블을 가지고 JOIN`  
`하나의 테이블에 동일한 의미를 갖는 컬럼`이 2개 이상 존재할 때 수행
  - 테이블에 회원 아이디 와 친구 아이디가 같이 존재하는 경우  
  내 친구의 친구를 조회하고자 할 때 

### join query
  ```java
    @Query("select ? from 엔티티이름 left outer join 엔티티안의참조속성 참조하는테이블의별칭")
  ```
- 쿼리의 결과는 Object 타입  
결과를 `Object 타입의 배열`로 형 변환해서 사용  
join을 수행한 경우는 **Arrays.toString** 을 이용해서 내용을 출력해서 확인해보고 사용

### join 테스트
### Board 데이터를 가져올 때 Writer의 데이터도 가져오기
  - BoardRepository 인터페이스에 메서드를 선언

### 테스트 클래스에 메서드를 만들어서 확인
  - 관계가 설정되지 않은 경우에는 on 절을 이용해서 조인이 가능

### 게시글을 가져올 때 연관된 모든 댓글을 가져오기
  - BoardRepository 인터페이스에 메서드를 선언

### 게시글 목록을 가져올 때 게시글 과 작성자 정보 그리고 댓글의 개수를 가져오기
 - BoardRepository 인터페이스에 메서드 선언
 
### 상세 보기를 위한 메서드
- BoardRepository 인터페이스에 메서드 선언

## 10. Service 계층
### 데이터 전송에 사용하는 게시글의 DTO를 생성
### 게시글 관련 서비스 인터페이스를 생성하고 기본 메서드를 작성 
### 게시글 관련 서비스 메서드를 구현할 클래스를 생성
### 게시글 등록 요청을 생성
- BoardService 인터페이스에 게시글 등록 요청을 처리할 메서드를 선언
  
- BoardServiceImpl 클래스에 게시글 등록 요청을 처리할 메서드를 구현

### 게시글 목록 보기를 처리
- 게시글 목록 보기 요청을 저장할 DTO 클래스를 생성 

- 게시글 목록 결과를 출력하기 위한 DTO 클래스 생성    

- 목록보기 요청을 처리하기 위한 메서드를 BoardService 인터페이스에 선언

- 목록보기 요청을 처리하기 위한 메서드를 BoardServiceImpl 클래스에 구현
- ServiceTest 클래스에 메서드를 추가해서 확인
    
### 상세보기
- BoardService 인터페이스에 상세보기를 위한 메서드를 선언

- BoardServiceImpl 클래스에 상세보기 처리를 위한 메서드를 구현
 
- ServiceTest 클래스에 테스트를 위한 메서드를 생성하고 확인
  
### 게시물 삭제
- 부모 Entity에 있는 데이터를 삭제할 때는 자식 Entity의 데이터를 어떻게 할 것인지 고민  
삭제를 할 것인지 아니면 삭제된 효과만 나타낼 것인지
  - 외래키의 값을 null로 하거나 삭제 여부를 나타내는 필드를 추가해서 필드의 값을 변경 등

- ReplyRepository 클래스에 글 번호를 가지고 댓글을 삭제하는 메서드를 생성
  
- BoardService 인터페이스에 글번호를 가지고 댓글을 삭제하는 메서드를 선언

- BoardServiceImpl 클래스에 댓글을 삭제하는 메서드를 구현
  
- ServiceTest 클래스에 메서드를 생성해서 게시글 삭제를 확인
  
### 게시물 수정
- Entity에는 setter를 만들지 않기 때문에 그냥은 수정 안됨

- Board Entity 에 수정 가능한 항목의 setter 메서드를 생성
  
- BoardService 인터페이스에 게시글 수정을 위한 메서드를 생성
  
- BoardServiceImpl 클래스에 게시글 수정을 위한 메서드를 구현





