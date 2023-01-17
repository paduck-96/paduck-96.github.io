---
title: "Java Spring Boot 서버에 파일 저장 및 Upload"
excerpt: "Spring Boot를 통해 서버에 파일 저장하는 방법에 대해 학습하고,
Upload 시 진행되는 파일의 유효성 검사 등에 대해 학습한다."

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot, file upload]

toc: true
toc_sticky: true

date: 2023-01-17
last_modified_at: 2023-01-17

---

### 영화 상세 보기 Repository

- 특정 영화 아이디를 이용해 데이터 찾아오기

### 영화 리뷰 출력 Repository

- review 안에서 Member 나 Movie 세부 데이터 출력 시 오류 발생
  - `Fetch 모드를 LAZY 로 설정`했기 때문에 발생  
    @**Transactional** 을 적용해,  
    전체 코드 수행이 종료 전에 DB 트랜잭션 종료시키지 않기

### @**EntityGraph**

- `Entity 의 특정 속성을 같이 로딩`하도록 표시하는 어노테이션
- 2개의 속성 설정 가능
  - attributePaths  
    로딩 설정을 `변경하고자 하는 속성의 이름`을 **배열**로 명시
  - type  
    `어떻게 적용할 것`인지 결정
    - FETCH 타입  
      명시한 속성만 EAGER, 나머지는 LAZY 로 처리
    - LOAD  
      명시한 속성만 LAZY, 나머지는 EAGER 로 처리

### 회원 탈퇴 처리

- 삭제 처리 과정에서 동일
- 회원 정보를 삭제하는 경우,  
  `외래키로 연결된 Review 데이터의 처리가 관건`
  - 같이 삭제하기  
    리뷰 데이터 자체가 의미 없다면 삭제
    - 실제 삭제 하거나, 별도 컬럼을 제공해 삭제한 것처럼 꾸미기
  - 외래키의 값 null로 설정하기  
    회원 정보가 없더라도 리뷰 데이터가 의미를 가진다면 null 설정

### ReviewRepository 회원 정보 삭제 메서드 구현

- 회원 정보 삭제시,  
  회원 정보 삭제 후 리뷰 정보 삭제 시 문제 발생
  - 외래키로 참조하는 테이블의 데이터 삭제가 우선

## 8. 파일 업로드

### 파일 업로드 방법

- Servlet 3 버전부터 추가된 `자체적인 파일 업로드 라이브러리` 이용
- 별도 파일 업로드 라이브러리 이용 가능

### 이미지 파일 출력

- `원본 이미지 출력` 방식
- `썸네일 이미지`를 만들어 출력 후,  
  썸네일 이미지 `클릭 시 원본 이미지 출력` 방식

### 이미지 미리보기

- JS를 이용해 업로드 전에 미리보기
- **서버에 업로드 후 미리보기**

### application.yml 에 파일 업로드 설정 추가

### Upload 처리

- Spring 에서는 파일 매개변수로 받을 수 있는 **MultipartFile** 자료형 제공

### upload controller 메서드 수정

### 서버에 파일 저장

- Spring에서 제공하는 FileCopyUtils 클래스 이용
- MultipartFile 클래스에 **transferTo** 메서드를 실행해,  
  `byte 배열 직접 읽고 쓰기`
  - 업로드 된 파일의 `확장자 검사 수행`  
    클라이언트 와 서버 모두에서 실행
    - exe, zip, tar 등의 확장자에 제한을 가하는 경우가 있음
- 파일 이름의 `중복 문제` 고려
  - UUID 나 회원 아이디, 날짜 등 추가해 해결
- `파일 업로드가 많은 경우 디렉토리` 문제
  - 날짜 별로 디렉토리 생성해 파일 업로드

### 파일 업로드 결과 반환 및 화면으로 확인

- 파일 업로드 결과를 위한 DTO 클래스 생성

### 업로드 된 이미지 출력

- Controller 클래스에 파일이름을 받아 내용을 전송하는 메서드

### Thumbnail 이미지 출력

- 목록보기에서 이미지 원본 크기 그대로 출력시,  
  여러 개의 이미지 출력해야 하기 때문에 효율성 하락
  - `Thumbnail 이미지 출력 후 필요시 원본 이미지 출력`
- thumbnail 생성 라이브러리를 이용해 생성
  - 이미지 이름 앞에 s\_ 추가
- ResultDTO 에 이미지 경로를 전달해 클라이언트 측으로 전송,  
  클라이언트는 이미지 확인 가능
- build.gradle 에 의존성 추가

### 업로드 된 이미지 삭제

## 9. 화면 출력을 위한 설정

### static 디렉토리에 bootstrap 파일 복사

### templates 디렉토리에 기본 레이아웃 파일 생성

## 10. 영화 정보 등록

### 개요

- 영화 제목 과 영화에 대한 이미지만 선택해서 업로드
