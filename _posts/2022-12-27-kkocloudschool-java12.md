---
title: "Java maven, Junit, Parsing"
excerpt: "Java maven 프로젝트에 대해 학습하고,
Java의 TDD인 Junit에 대해 학습하고,
Csv와 Json parsing에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, maven, junit, parsing]

toc: true
toc_sticky: true

date: 2022-12-27
last_modified_at: 2022-12-27
---

### 최종 연산

- 매칭
  - allMatch
  - anyMatch
  - noneMatch
- 집계
  - count
  - findFirst
  - max
  - min
  - average
  - reduce
  - sum

> Optional 이 붙는 자료형으로 리턴되는 경우는 null 일 수 있으므로 null 여부를 확인하고 사용하라고 하는 것입니다.

- 루핑  
  forEach
- 수집  
  collect
  - Collectors.to자료형()  
    중간 연산의 결과를 자료형(List, Set, Map)으로 변환
  - Collectors.groupingBy(그룹화할 함수, 집계함수)  
    그룹화해서 결과를 Map으로 리턴  
    `Map의 키`는 자동으로 그룹화할 함수의 값

## 3. 병렬 처리

### 용어

- Multi Processing  
  `2개 이상의 프로세스`가 동작 중인 것
  - 프로세서가 여러 개
- Multi Threading  
  `2개 이상의 스레드`가 동작 중인 것
  - 하나의 프로세서로 가능
- Parellel Processing  
  `동시에 작업`을 처리
- 데이터 병렬성  
  `많은 양의 데이터를 나누어서` 처리
- 작업 병렬성  
  `동시에 발생하는 작업`을 `각각의 스레드`에서 병렬로 처리
  - Web Server

### 스트림 API에서의 병렬 처리

- **데이터 병렬성**
- 스트림을 만들 때 parallelStream 을 호출  
  스트림을 만든 후 parallel() 호출시 데이터를 나누어서 병렬로 처리

# Java GUI Programming

### 프로그래밍 방법

## 1. AWT

- `운영체제의 자원을 사용`하는 방식
  > 무거워서 지금은 거의 사용하지 않음

## 2. Swing

- `JDK 자원을 사용`하기 때문에 AWT 보다는 가벼움
  > 디자인이 어려움

## 3. Java FX

- `스타일 설정`이 가능해지고 `다양한 컴포넌트`를 제공

# Open Source 활용

## 1. Open Source

### Source Code를 누구나 열람할 수 있고 사용할 수 있도록 만든 것

### 장점

- 편리성
  - 직접 만들려고 하면 시간과 비용이 많이 소모
- 성능이 우수할 가능성이 높음
- 버전이 높다면 신뢰성이 높음

### Java Open Source Project

- Hadoop  
  아파치 재단에서 만든 오픈 소스 분산 처리 시스템
- cassandra Database  
  NoSQL
- Lucene  
  검색 엔진 플랫폼
- Maven  
  Java Build 도구
  - Gradle, Jenkins 등의 도구도 있음
- Apache Web Server
- Log4j
- Commons Project

## 2. Maven

### **build**

- `Source Code를 Compile 하고 최종적인 실행 파일`로 만드는 작업
- 일반 App은 **jar** 파일로 생성  
  Web Application 은 **war** 생성  
  android는 **apk** 파일을 생성
- `jdk/bin 디렉토리에 있는 명령어`로 작업을 수행
- `외부 라이브러리를 이용해 build script에 빌드할 내용 기술`후 수행
  - Maven(porm.xml) 이나 Gradle(build.gradle)
- `CI 도구`에서 `자동으로 빌드 후 정기적으로 빌드` 가능한지 확인 후  
  Deploy 까지 자동으로 수행하는 경우도 있음
  - Jenkins 가 많이 사용

### build tool

- Ant
- Maven
- Gradle
- Jenkins

### maven

- **POM**(Project Object Model - pom.xml) 이라는 것에 기초를 두고  
  빌드, 테스트, 도큐먼테이션, 성과물 의 배치 등의  
  `라이프 사이클 전체를 관리하는 도구`

### pom.xml 파일

- `maven 의 설정 파일`
- **repositories**  
  다운로드 받을 저장소 설정
  - Oracle 사용 시 설정
- **dependencies**  
  외부 라이브러리의 의존성 설정

### Maven 프로젝트 생성

- Maven 프로젝트를 생성
- 기존 프로젝트를 Maven 프로젝트로 변환

### 의존성 설정(외부 라이브러리 사용)

- Java Application  
  **Build Path** 에 사용하고자 하는 라이브러리 추가
- Java Web Application  
  **WEB-INF/lib 디렉토리**에 라이브러리 추가
- build tool 을 사용  
  **설정 파일에 작성**만 하면 다운로드를 받아 `로컬에 저장`하고  
  `build를 할 때 그 라이브러리를 프로젝트에 포함`

### Maven 프로젝트를 이용한 MySQL 라이브러리 사용

- MySQL 드라이버 다운로드 후 Libraries에 추가
- Maven project로 변환했다면
  - dependencies에 추가

### 의존성 설정 후 동작

- `pom.xml 파일`에 작성된 **dependencies** 를 확인 한 후 >>  
  현재 사용자의 .m2 라는 디렉토리에서 라이브러리의 존재 여부 확인 >>  
  없을 경우 중앙 저장소에서 다운 받아 .m2에 저장하고 >>  
  빌드 할 때 다운로드 받은 라이브러리를 프로젝트에 복사 후 실행

...

## 3. JUnit

### 개요

- `단위 테스트를 수정해주는 프레임워크`
  - 전체 프로그램 구성하는 `기본 단위 프로그램의 오류 여부 확인`  
    `클래스 나 메서드`
- **TDD**(테스트 주도 개발)에서 가장 많이 사용되는 라이브러리
- Eclipse 등에 내장

### 테스트 방법

- `TestClass 로 부터 상속`받는 클래스 생성
  - 클래스 안에 만들어진 모든 메서드 테스트
- Object 클래스로부터 상속받는 클래스 만들고  
  `테스트 하고자 하려는 메서드 위에 @Test 추가`
  - 결과를 테스트 할 때 **assertThat**( ) 나 **assertEquals**( ) 활용  
    첫번째 인수로 `실제 값`, 두번째 인수에 `기대하는 값`을 기재해 호출
    - 양쪽의 값이 다르면 AssertionError 예외 발생

### Testcase 클래스를 이용한 테스트

- 테스트 수행하기 위한 클래스 생성

### @Test 사용

### 실제 테스트 수행 시 별도의 패키지에서 수행

- `배포 시 제외`하기 위해서
- 최근 프레임워크는 프로젝트 생성시 `test 위한 디렉토리` 제공  
  해당 디렉토리에서 작성한 내용은 빌드시 제외

## 4. CSV 활용

### csv(Comma Seperated Values)

- `항목을 쉼표로 구분해서 만든 텍스트 파일`, 확장자는 **csv**  
  최근에는 `분할 할 수 있는 텍스트는 전부 csv`로 간주
- 대부분의 경우 `첫번째 라인의 데이터`는 **헤더** 항목(컬럼 이름)  
  `두번째 라인부터가 실제 데이터`
- `변하지 않는 고정적인 데이터` 제공할 때 주로 이용
- 외부 라이브러리를 이용해서 주로 사용
- **super-csv** 라는 외부 라이브러리가 csv를 읽고 쓰기 편리하게 해줌

### 사용 설정

- 일반 프로젝트의 경우 라이브러리를 다운받아 build-path 에 추가
- Maven/Gradle 프로젝트는 의존성 설정
  - Maven 의 경우는 pom.xml 파일의 dependencies 태그 안에 추가

### 샘플로 사용할 csv 파일 추가

- 확장자는 의미 없음

### csv 파일 데이터 표현하기 위한 VO 클래스

### super-csv를 활용한 csv 파싱

## 5. JSON Parsing

### **JSON**(JavaScript Object Notation)

- `자바스크립트의 데이터 표현법`을 이용해 데이터 표현하는 `문자열 서식`
- { } 안에 `Key 와 Value 형태`로 **객체**(Object) 생성하고  
  **배열**(Array)은 [ ] 안에 `데이터를 나열`
- `문자열은 큰 따음표`를 해야 하고, `key는 무조건 문자열`

### Java에서 JSON 파싱

- **org.json.json** 이라는 라이브러리 사용 가능
- **JSONObject** 라는 클래스 와 **JSONArray** 라는 클래스 존재  
  json 문자열을 파악해서 `2개 중 하나의 클래스 생성자에 문자열 대입`
  - 인스턴스 생성
  - 데이터를 꺼내 올 때는 **get자료형**(키 또는 인덱스)로 데이터 추출

### JSON 파싱

- 샘플 데이터 -> https://jsonplaceholder.typicode.com/todos
- 프로젝트에 json 라이브러리의 의존성 설정

### JSON 생성

- Spring Framework 에서는 이 기능을 자동으로 수행
- 직접 수행하고자 할 경우 의존성 설정
  - jackson-core, jackson-bind

## 6. 기타

- **XML Parsing**
  - 내장 클래스 이용
- **HTML Parsing**
  - jsoup 라이브러리 이용  
    `ajax 나 frame으로 만들어진 데이터`를 가져오기 위해서는 **selenium**
