---
title: "Java Spring Boot 서버 리뷰 처릭 관련 서비스"
excerpt: "Spring Boot를 통한 리뷰 처리 서비스 과정에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, spring boot]

toc: true
toc_sticky: true

date: 2023-01-18
last_modified_at: 2023-01-18

---
### 영화에 대한 요청을 처리할 Controller를 생성하고 등록 요청을 처리할 메서드를 생성

### 영화 등록을 위한 화면을 templates/moview/register.html 파일로 생성

### 영화 이미지 데이터를 위한 DTO 클래스 생성 

### 영화 데이터를 위한 DTO 클래스를 생성 

### 영화 데이터 처리를 위한 서비스 인터페이스를 생성하고 삽입 메서드를 선언 

### 영화 데이터에 대한 처리를 위한 서비스 클래스를 생성하고 삽입 메서드를 구현 

### )MovieController 클래스에 등록 요청을 처리하는 메서드를 작성
    
### register.html 파일에 파일 업로드 와 이미지 미리보기 코드를 작성
- 파일을 선택했을 때 업로드 하기 위한 스크립트 코드를 작성
  - register.html 파일에 이미지를 출력할 영역을 생성
 - uploadResult 에 대한 스타일 설정
- 이미지 업로드 결과를 받아 업로드 한 이미지를 미리보기 해주는 함수를 스크립트에 추가
- 이미지 업로드에 성공했을 때 핸들러에 이미지 출력 함수를 호출하는 코드를 추가
- 이미지 위의 삭제 버튼을 누르면 업로드된 이미지를 삭제하는 스크립트 코드를 작성
  ### register.html 파일에 데이터 등록을 위한 스크립트 코드를 작성

## 11. 영화 목록 보기
- 영화 번호, 영화 대표 이미지 1개, 내용, review의 grade 평균 그리고 등록일을 출력

### 목록 요청을 위한 DTO 클래스를 생성

### 목록 보기 요청에 응답을 하기 위한 DTO 생성 
### MovieDTO 수정 - 화면 출력을 위한 속성 추가
### MovieService 인터페이스에 Entity 와 DTO 사이의 변환을 수행해주는 메서드를 추가
- 목록을 가져오면 Movie, List<MovieImageList> , 리뷰 평균, 리뷰 개수
  
### MovieService 인터페이스에 목록보기를 위한 메서드를 선언
### MovieServiceImpl 클래스에 목록보기를 위한 메서드를 구현
### MovieController 클래스에 목록 보기 요청을 위한 메서드를 생성
### movie 디렉토리에 list.html 파일을 생성하고 화면 출력을 작성
## 12. 상세보기
### MovieService 인터페이스에 상세보기를 위한 메서드를 선언
### MovieServiceImpl 클래스에 상세보기를 위한 메서드를 구현
### Test 디렉토리에 Service 테스트를 위한 클래스를 생성하고 메서드를 테스트
### MovieController 클래스에 상세보기 요청을 처리할 메서드를 생성
-  list.html 파일의 제목 출력 부분을 수정
- 상세보기 화면을 movie 디렉토리의 read.html 파일을 생성하고 작성
## 13. 리뷰 작업
### Review Entity 클래스에 수정이 가능하도록 메서드를 추가
   ### 리뷰 작업을 위한 DTO 클래스를 생성 
### Review 작업을 위한 메서드를 선언할 인터페이스 생성 
### Review 작업을 위한 메서드를 구현할 서비스 클래스 생성 
### Review 작업을 수행할 Controller 클래스를 생성
- review의 작업은 화면 전환을 하지 않는 경우가 대부분이라서 화면을 보여주는 Controller 
대신에 데이터를 넘겨주는 REST Controller를 주로 이용  
화면에서도 요청을 ajax 형태로 요청을 전송


### 별점 처리
- https://github.com/dobtco/starrr 라이브러리 이용
  - jquery plugin

### read.html 파일에 2개의 모달 창을 추가 
- 하나의 모달 창은 리뷰 작업을 위한 모달,  
다른 하나는 이미지 원본 보기 모달
### 별점 라이브러리 설정
- https://github.com/dobtco/starrr 에서 다운로드

- 압축을 해제하고 dist 디렉토리의 css 파일은 static/css 디렉토리에 복사,  
js 파일은 static/js 디렉토리에 복사

- 설정을 추가 
  - <script> 태그 위에 설정
### 리뷰 등록
- read.html 파일에 리뷰 등록 버튼 과 리뷰 출력 영역을 생성
  
### 리뷰 출력