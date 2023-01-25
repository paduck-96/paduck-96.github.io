---
title: "Java Spring Security를 활용한 로그인과 소셜 로그인"
excerpt: "Spring Security를 활용해 로그인하는 과정에 대해 학습하고,  
OAuth2를 활용한 소셜 로그인에 대해 학습한다"

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
- Controller에서 로그인 한 유저의 정보를 확인

### 회원 가입 처리
- 회원 가입을 위한 DTO 클래스를 생성
- 회원 관련 처리를 위한 서비스 인터페이스를 생성하고 회원 가입 처리 메서드를 선언 

- 회원 관련 처리를 위한 서비스 클래스를 생성하고 회원 가입 처리 메서드를 구현

- 회원 가입을 위한 요청 처리 코드를 MemberController 클래스에 추가
- 회원 가입 화면을 생성

## 7. OAuth
### 개요
- 인증 서비스를 제공하는 업체들의 공통된 인증 방식(**Open Authorization**)

### Kakao Login
- developers.kakao.com에 접속을 해서 애플리케이션을 등록하고 REST API 키를 복사

- 키 화면 하단에서 플랫폼 등록  
Web - http://localhost

- 왼쪽 화면에서 카카오 로그인을 클릭하고 카카로 로그인을 활성화

- 하단으로 내려서 Redirect URI 설정 
  - http://localhost/login/oauth2/code/kakao

- 왼쪽 상단의 메뉴 중에서 동의항목을 클릭해서 수집하고자 하는 정보를 선택

- 왼쪽에서 보안 메뉴를 클릭하고 Client Secret 을 눌러서 코드를 발급 받아서 저장

- build.gradle 파일의 OAuth2 라이브러리의 의존성이 설정되어 있지 않으면 추가
  - implementation   
  'org.springframework.boot:spring-boot-starter-oauth2-client'

- application.yml 파일에 카카오 로그인 설정 관련 코드를 추가
```java
spring:
  security:
    oauth2:
      client:
        registration:
          kakao:
            client-id: 
            client-secret: 
            redirect-uri: http://localhost/login/oauth2/code/kakao
            authorization-grant-type: authorization_code
            client-authentication-method: POST
            client-name: Kakao
            scope:
              - profile_nickname
              - account_email
        provider:
          kakao:
            authorization-uri: https://kauth.kakao.com/oauth/authorize
            token-uri: https://kauth.kakao.com/oauth/token
            user-info-uri: https://kapi.kakao.com/v2/user/me
            user-name-attribute: id
```
- CustomSecurityConfig 클래스의 **filterChain** 메서드,  
OAuth2 가 사용하는 로그인 URL을 설정
```java
        //Oauth2 가 사용할 로그인 URL 설정
        http.oauth2Login().loginPage("/member/login");
```

- login.html 파일에 카카오 로그인 링크를 추가
- 여기까지 작업을 하고 애플리케이션을 실행하면 카카오 로그인까지는 수행

- OAuth2 로그인 방식으로 로그인 된 경우 처리할 서비스 클래스를 추가하고 작성 

### OAuth2를 사용했을 때 문제점
- 매번 새로운 유저로 판단 
  - email 같은 정보를 데이터베이스 테이블에 저장해서,  
  이전에 로그인 한 적이 있는 유저인지 판단
  - 여러가지 로그인 방법을 제시하는 경우 서로 다른 유저로 판단하는 문제  
    - 재가입을 시키는 형태로 해결  
  이 때 이메일을 전부 등록해 소셜로 로그인했을 때 아이디를 찾아주는 방식

### 카카오로 로그인 성공했을 데이터베이스에 등록하기
- Spring Security 가 사용하고 있는 ClubMemberSecurityDTO 클래스를 수정
- 소셜 로그인을 수행한 후 이메일을 가진 사용자를 찾아보고 없는 경우에는 회원가입,  
 있는 경우에는 그 정도를 리턴하도록 CustomOAuth2UserService 클래스 수정

- 실행 한 후 소셜로 로그인 했을 때 회원 가입이 되어 있는지 확인