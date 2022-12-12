---
title: "Java에 대한 이론적 설명"
excerpt: "Java의 환경에 대해 학습하고,
Java의 구성 요소에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, Java]

toc: true
toc_sticky: true

date: 2022-12-12
last_modified_at: 2022-12-12
---

# Java

## Java

Sun Micro Systems 에서 만든 프로그래밍 언어

- 여러 플랫폼에서 실행되는 프로그램을 `한 번만 작성하기 위해` 언어 개발

### C++ 과 가장 큰 차이점

- C++
  - `플랫폼 종석적`  
    운영체제 별로 소스 코드를 다르게 작성
    - 컴파일러가 `운영체제에서 실행되는 코드` 생성
- Java
  - `플랫폼 독립적`  
    여러 운영체제에서 실행되는 프로그램 한 번만 작성
    - 컴파일러가 `JRE(JVM) 이 이해하는 코드` 생성  
      운영체제 별 JRE를 설치해 각자 해석해 운영체제에서 실행되게

### Java 사용 이유

- 오픈 소스 프로젝트가 많이 구현

  - 뛰어난 Echo System

- 자바 개발성 향상 라이브러리

  - apache common

- 서버 개발성 향상 라이브러리

  - spring, struts

- 검색 엔진

  - Lucene

- NoSQL

  - Cassandra

- 분산 파일 시스템

  - Hadoop

플랫폼으로서의 역할  
JVM 기반의 언어가 많음

- Jython, Scala, Kotlin, Closure, Jruby, Groovy 등

`소스 코드를 작성 후 컴파일하면 JRE 가 이해할 수 있는 코드로 번역`

### Java 개발 플랫폼

- **J2SE**(Standard Edition)  
  `PC용 애플리케이션 개발 플랫폼`
  - 웹 프로그래밍 불가하나 J2EE 웹 API인 WAS 나 Spring이 제공
- **J2ME**(Micro Edition)  
  `embedded 관련 애플리케이션 개발 플랫폼`으로 J2SE 에서 많은 기능 제거
  - 용량 축소를 위해 기능 삭제
- **J2EE**  
  가장 많은 기능을 가진 유료 버전이었으나 Eclipse 재단 소유로 Open source

### Java 환경

**JDK**(Java Development Kit)  
자바 개발 도구

- Java API  
  자바로 프로그램을 만들 수 있도록 `제공되는 클래스 집합`
- JVM(Java Virtual Machine)  
  자바 프로그램을 실행할 수 있게 `추상화한 영역`으로
  - 자바 프로그램이 `실행될 때`  
    `메모리 영역`(Register, Stack, Heap, Method 등)을 구분해서 확보

**JRE**(Java Runtime Environment)  
 자바 실행 환경  
 자바로 만든 프로그램을 `실행하기 위한 플랫폼`

- **JVM**  
  `JVM 이 라이센스`가 있는데 Oracle의 Hot Spot  
  오픈소스인 OpenJDK
- **glue**  
  `플랫폼 고유`의 라이브러리 와 JNI
- **byte code**  
  `JDK 를 이용`해서 개발 후 컴파일하면 생성되는  
  `JRE 가 이해가능한 코드`
  - Kotlin 도 동일한 byte code 생성

### JVM 구성

**Native Method 영역**

- `운영체제에게 전달할 메서드`를 소유한 영역

**Register 영역**

- `CPU에게 전달할` 코드 영역

**Stack 영역**

- 메서드를 호출했을 때 `메서드에게 할당`되는 영역

**Heap 영역**

- `객체게 사용하는 메모리 영역`
  - Young Generation
  - `Old Generation`  
    가비지 컬렉션의 대상이 되지만  
    실 가비지 컬렉터는 `Young 영역보다 적게 참조`
  - `Permanent`  
    클래스 정보

**Method 영역**

- `클래스의 메서드가 사용`하는 영역  
  **Static** 한 영역 혹은, **Class** 영역

## 개발 환경

### JDK

SE 버전을 설치

- 버전 번호는 8, 11, 17 버전을 많이 사용

**Java 8**  
람다 와 스트림이 적용  
전자 정보 프레임워크가 이 버전 기반

**Java 11**  
Spring이 사용하는 버전  
최신 Eclipse 도 이 버전부터 사용 가능

Java 17  
최신 버전

### IDE

- **Eclipse**  
  오픈 소스  
  `플러그인 형태로 별도의 라이브러리`를 가진 형태로 제공
  - 전자 정보 프레임워크, Spring Tool Suite, 애니 프레임워크
- **IntelliJ**  
  웹 프로그래밍은 상업용 버전에서만 가능

## 작성 및 실행

### 과정

source code 작성

- `파일 확장자는 java`

PC에서 실행되는 Application 을 만들 때는  
`static void main` `메서드 클래스 필수`

- **entry point**

**compile** 수행

- `javac 명령 수행`  
  `JVM이 인식할 수 있는 코드`를 만들어주는 과정  
  문법 검사 수행  
  Compile 실패 시 문법 오류 도출

**build**(javaw 명령)

- `운영체제 나 하드웨어가 인식`할 수 있는 코드 생성

**run**(java 명령)

- `메모리 할당을 한 후 실행`  
  오류 발생 시 `메모리 오류 나 예외` 발생

### IDE 사용

모든 과정을 한 번에 수행

코드 작성 후 저장 순간마다 compile 을 수행해서 문법 오류 발생 시 표시

### Eclipse 애플리케이션 생성

- workspace  
  동일한 환경 설정을 사용하는 단위

- pc용 Application 생성  
  FIle - New - Others - Java - Java Project

- 소스 코드 작성  
  객체 지향 언어로 `모든 코드가 클래스 안에` 들어가야 하고  
  `클래스 이름 과 파일 이름이 일치`해야 함

- 구조
  - **package**  
    자신이 속한 패키지 이름
    - 생략 되거나 한 번만 나와야 함
  - **import**  
    이름을 줄여쓸 패키지 나 클래스 이름
    - 0번 이상 무제한
  - **class**
  ```java
  클래스이름 {
    코드
  }
  ```

마우스 오른쪽을 눌러서 New - class 누른 후 옵션 설정 시 소스 코드 작성

Run- run 눌러서 코드 실행

### 작성 시 유의 사항

`대소문자 구별`  
한 번에 실행되어야 하는 `문장의 끝은 ;`

- 블럭 생성 명령어(클래스, 메서드, 제어 블럭, try~catch 등)은 불필요

행의 개념이 없으므로 `한 줄에 여러 개의 명령어` 사용 가능

블럭(영역)의 생성은 { }

## 명명 규칙

데이터와 메서드 그리고 클래스에 붙이는 이름(Identifier)을 만드는 규칙

`예약어는 이름으로 만들 수 없음`

`동일한 영역에 이름을 중복해서 만들 수 는 없음`

- package > class > 변수 와 메서드의 원형 > 메서드 안에 변수 와 블럭
  ```java
  void method() =/ void method(int) // 가능
  void method1(int x) == void method1(int y) // 불가능
  ```

## 구성 요소

- **Keyword**  
  java 가 `정해준 기능`
- **데이터**  
  `variable`(데이터 이름 있고 변경도 가능)

  - 소문자로 시작

  `constant`(데이터 이름 있으나 변경 불가)

  - 대문자 시작

  `literal`(사용자 직접 입력 데이터)

- **Operator**  
  연산자
- **Control statement**  
  제어문
- **Array**  
  `동일한 모양`을 갖는 데이터의 `연속적인 모임`
- **Class** 와 **Instance**, **Interface**  
  `데이터 와 Method(기능)` 을 같이 소유하고 있는 것
- **Annotation**  
  `@로 시작하는 명령어`로 `자주 사용하거나 복잡한 구문`을 하나의 이름으로
  - java 에서는 `class로 취급`
- **Comment**  
  주석
