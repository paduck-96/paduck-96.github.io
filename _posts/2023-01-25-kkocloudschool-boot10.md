---
title: "Java Spring Boot 서버 리뷰 처릭 관련 서비스"
excerpt: "Spring Boot를 통한 리뷰 처리 서비스 과정에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot]

toc: true
toc_sticky: true

date: 2023-01-25
last_modified_at: 2023-01-25

---
### logout
- 기본적으로 제공되는 URL인 logout을 호출,  
  - Spring Security 는 `쿠키 와 세션을 이용해서 로그인을 관리`하고  
  Tomcat은 `JSESSIONID로 시작하는 쿠키`를 생성,  
  이 쿠키를 삭제
- 로그아웃이 되면 자동으로 login 요청을 수행하고,  
이 때 logout 이라는 문자열 파라미터가 전달

- logout이 되서 login 요청으로 이동하는 것을 구분
  - MemberController의 login 요청을 수정

- login.html 파일에 출력을 추가

### 자동 로그인
- **remember me**
- `쿠키를 이용`해서 브라우저에 로그인했던 정보를 유지하는 방식으로 구현
- 이 기능을 사용하기 위해서는 현재 프로젝트가 사용하는 데이터베이스에 `persistent_logins 라는 테이블`을 추가

- CustomSecurityConfig 클래스 수정
- login.html 파일에서 remember-me를 전송할 수 있도록 체크박스를 추가

### 화면에서 인증 정보 사용 과 컨트롤러에서 사용
- 화면에는 **authentication** 이라는 객체를 이용해서 전달
- Controller는 **SecurityContextHolder**.**getContext**().**getAuthentication**() 이용,  
`Authentication 타입`으로 사용

- member.html 파일에 로그인한 유저의 정보를 사용하기 위한 스크립트 추가
- SampleController에서 member 요청을 처리하는 메서드 수정
### 403 에러 처리
- 403 에러
  - 서버에 클라이언트의 요청이 도달했는데,  
  서버가 클라이언트의 접근을 거부할 때 반환하는 요청 코드

- 403 에러 처리를 위한 핸들러 클래스 생성 

- CustomSecurityConfig 클래스의 filterChain 메서드에 핸들러 추가

## 6. Spring Security JPA
### Entity 구성
  ```
    회원 정보 - ClubMember
    아이디: mid
    비밀번호: mpw
    이메일: email
    이름: name
    소셜 가입 여부: social
    탈퇴여부: del
    가입일 
    수정일
```
- 권한 정보  
ClubMemberRole
  - USER  
  ADMIN

### Entity 생성
- 공통 속성을 갖는 Entity
- 권한을 나타내는 enum을 생성
- 회원 정보를 나타내는 ClubMember Entity 생성

- 실행 한 후 테이블 확인
  - club_member 와 club_member_role_set 테이블이 생성되어 있음
  - RoleSet이 Collection 이므로 1:N 관계가 되서 별도의 테이블로 생성

### ClubMember Entity에 데이터베이스 작업을 위한 Repository 

### RepositoryTest

### 소셜로 로그인 한 경우 email을 가지고 로그인 여부를 판단
- ClubMemberRepository 인터페이스에 메서드 선언
  
- Tests 클래스에 메서드를 만들어서 확인

### 로그인 처리 결과
- Spring Security를 사용하는 로그인 처리,  
`로그인 처리 결과를 User 클래스를 상속받는 클래스에 저장`
- 로그인 처리 결과로 사용할 클래스를 생성 

- CustomUserDetailsService 클래스 수정
- 실행 한 후 데이터베이스에 있는 데이터로 로그인되는지 확인
- html 파일에서 `sec 로 시작하는 DTD를 이용하`면 로그인 여부  
  - (isAuthenticated), 권한 소유 여부(hasRole(권한)), authentication 등을 이용해서 사용자 정보를 추출

- member.html 파일을 수정해서 로그인 정보 출력