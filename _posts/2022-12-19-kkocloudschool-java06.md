---
title: "Java 이론 6"
excerpt: "Java 중첩 클래스에 대해 학습하고,
데이터를 공유하는 방법에 대해 학습하고,
예외 와 오류 처리에 대해 학습한다."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, global data, Exception, error]

toc: true
toc_sticky: true

date: 2022-12-19
last_modified_at: 2022-12-19
---

# Nested Class

자바는 `클래스 안에 클래스를 생성`하는 것이 가능  
`클래스 안에 인터페이스 생성` 가능  
`인터페이스 안에 인터페이스 생성` 가능

- 자바는 소스 파일 단위로 컴파일 되는 것이 아니고 `클래스 단위로 컴파일`
- **Inner Class 접근 지정자**에 private 과 protected 사용 가능  
  클래스 특성으로 static 사용 가능
  - **일반 클래스 접근 지정자**는 public 이나 package(생략)  
    클래스 특성으로 abstract 와 final 만 가능

## 1. 종류

### Instance Inner Class

클래스 안에 만들어진 클래스

- `static 멤버가 없어야 함`

### Static Inner Class

클래스 안에 만들어진 클래스

- `static 멤버 가능`

### Local(Method, Function) Inner Class

`메서드 안에 만들어진 클래스`

### Anonymous class

`이름 없는 클래스`

## 2. Instance Inner Class

클래스 안에 만들어지는 클래스

```java
class Outer{
    class Inner {
        속성
        메서드
    }
    ...
}
```

- 컴파일 완료시 **Outer.class** 와 **Outer$Inner.class** 2개의 클래스가 생성
- 외부 사용시 클래스 접근 지정자 public  
  `Outer 클래스의 인스턴스`를 만들고, `이를 통해 Inner 클래스 인스턴스` 생성

## 3. Static Inner Class

`Inner Class 안에 static 멤버`가 있는 경우 일반 클래스로 생성 시 오류

- `static 멤버는 클래스 이름만으로 호출 가능`해야 하는데  
  Instance Inner 클래스로 만들면 인스턴스를 만들고 호출
- 에러 없애기 위해 `Inner Class 에 static 추가`

## 4. Local Inner Class

`내부 클래스가 메서드 안에서 생성`되는 형태

- 메서드 안에서만 사용

Local Inner Class 에서는 `자신을 포함하고 있는 메서드의 지역 변수 사용 불가`

- `final 변수`만 사용 가능
- 메서드는 **Stack** 에 생성, 클래스는 **Static** 영역에 생성  
  static 영역은 한 번 생성시 계속 존재해 stack, heap에서 접근 가능하나  
  static 영역에서 stack, heap의 멤버는 접근할 수 없음
  - stack, heap이 반드시 존재한다는 보장 불가능

## 5. Anonymous(익명, 객체) Class

상속을 받거나 인터페이스를 구현해야 하는 경우  
별도 하위 클래스를 만들지 않고, 인스턴스를 바로 생성

- 인스턴스 1개만 생성해 사용하는 클래스의 경우  
  별도로 클래스를 생성하는 것은 자원 낭비의 가능성
  - 클래스는 한 번 만들어지면 메모리에서 삭제 안 됨

```java
new 상위클래스나인터페이스이름( ){
    // 필요한 속성이나 메서드 정의
}
```

- 인터페이스에 메서드가 1개인 경우 확장된 방식, 람다를 이용

## 6. Java 클래스 상속 및 인터페이스 구현

### **하위 클래스** 만들어 사용

인스턴스를 2개 이상 생성하고자 하는 경우

### **Anonymous class**

인스턴스를 1개만 생성해서 사용하는 경우

- 안드로이드 이벤트 처리
- lambda 와 혼용해 사용

# final

## 1. final 변수

읽기 전용의 변수, 값을 수정할 수 없음

## 2. final 메서드

overriding 할 수 없는 메서드

## 3. final 클래스

상속할 수 없는 클래스

### final 메서드나 final 클래스는 기능 확장 불가

- 시스템을 핸들링 하기 때문에 기능 확장 금지

# 데이터 공유

## 1. 전역 변수(Global Variable)

`모든 곳에서 사용가능한 데이터`

- public class 안에 **static 속성** 만들어 사용

  - 비권장

- `singleton 패턴`으로 클래스 디자인해 속성이나 메서드 만들어 사용  
  `클래스의 인스턴스를 1개만 만들게 하는 패턴`으로  
  Server Application 에서 주로 이용
  - 안드로이드, ios, Web App 등이 이를 통해 entry point 에 접근 가능
  - Design Pattern  
    클래스를 용도에 맞게 설계하는 기법
  ```java
  // 생성자 private
  // 자신의 type 으로 만들어진 static 속성 정의
  // static 속성에 데이터 만들어 주입후 리턴하는 static 메서드 생성
  ```
- 서버는 `하나의 인스턴스를 이용해 멀티 스레드`로 클라이언트의 요청 처리
  - `하나의 클래스에 대한 인스턴스를 1개만 만드는 것`이 일반적

## 2. 동일한 클래스로부터 만들어진 인스턴스 사이의 데이터 공유

### `static 속성` 사용

## 3. 클래스 안에서 다른 클래스의 인스턴스 만드는 경우

- has a 관계  
  **포함하고 있는 클래스의 인스턴스**는 `포함되는 클래스`의 인스턴스 참조를 알기 때문에 `참조를 이용해 바로 접근 가능`  
  포함된 클래스의 인스턴스에서는 외부 클래스의 속성에 바로 접근 안됨
  - **생성자를 이용한 주입** 또는 **setter 메서드를 이용한 주입**  
    생성자 나 setter 메서드를 이용해서 포함하는 클래스 인스턴스 참조를 넘겨주기

## 4. 데이터 공유

데이터를 공유하기 위해서는 `매개변수로 계속 넘겨주는 방법`

- react의 props 사용 방식

공유 데이터를 `모든 곳에서 접근 가능`하도록 만들어 사용하는 방법

- 모바일 앱

`포함되는 형태에서의 데이터 공유`

- **네비게이션 구조**(생성자 나 setter 를 이용한 주입)

# Java Document

자바의 `클래스 나 메서드에 기술하는 주석`  
자바는 `문서화 주석 기능`을 제공

- 외부 라이브러리를 이용해도 생성 가능

문서화 주석 만들 때는 /\*\* 내용 \*/

문서화 주석을 만들 때 IDE 에서 메서드에 마우스 커스를 대면  
툴팁으로 메서드의 문서화 주석이 표시

문서화 주석은 클래스, 메서드, 필드에 작성 가능

자바 문서화 주석에 추가할 수 있는 **태그**

- @author  
  작성자
- @deprecated  
  폐지되었거나 폐지될 가능성이 있는 코드
- @param  
  메서드의 파라미터 나 제너릭에 대한 설명
- @return  
  리턴값에 대한 설명
- @since, @version  
  버전에 대한 설명
- @throws  
  예외에 대한 설명

```java
//이클립스
- 생성
project>Generate Javadoc

//
https://docs.oracle.com/javase/8/docs/api
처럼 나옴
```

# Exception Handling

## 1. 오류의 종류

### `Compile Error`

물리적 오류

- .java 파일을 .class 파일로 만들 때 발생하는 오류
- `문법 오류`
- IDE를 통해 체크 가능

### `Logical Error`

논리적 오류

- 알고리즘의 잘못으로 인해서 `잘못된 결과가 만들어지는 경우`
- **black box test**(기능 테스트)  
  입력을 주고 정확한 출력이 만들어지는지 확인

  - 발견되고 `debugging을 이용`해서 수정

  ### `Exception`

  예외

  - 문법적인 오류는 없어서 컴파일, 빌드가 되어 실행이 되나  
    `실행 도중 예기치 않은 상황이 발생`해서 프로그램 중단
  - `디버깅을 이용해서 예외 발생 가능한 코드 부분을 수정`하거나  
    `예외 처리`를 통해 예외가 발생해도 정상적인 동작 수행되게

### `Assertion`

단언

- 문법적으로 아무런 문제가 없지만  
  `강제로 예외를 발생시켜 프로그램 중단`
- 별도 기능으로 제공했지만 최근에는 `예외 처리로 수행`하는게 대부분

## 2. Debugging

`메모리의 값을 확인`하는 작업

- 방법  
  **출력 메서드**를 이용해서 확인

  - System.out.print 메서드 이용

  IDE가 제공하는 **Debugging Tool** 이용

  테스트를 수행해주는 **외부 라이브러리 나 테스팅 툴** 이용

## 3. Eclipse 의 디버깅 툴 이용

## 3. **Exception**

### Exception(예외)

- 문법적으로 이상이 없어서 컴파일 시점에는 아무런 문제가 되지 않지만  
  실행 중에 발생하는 `예기치 않은 사건으로 프로그램 중단`되는 현상

### 예외 처리

- 예외가 발생할 가능성이 있는 코드를 `예외 처리 구문`으로 묶어서  
  `예외가 발생한 후의 동작을 설정`하는 것
- 예외가 발생했을 때 `예외 내용을 로깅`하거나  
  예외 발생 시 `정상적인 값으로 변경`해 동작하거나  
  예외 발생해도 `무시하고 동작`하도록 하기 위해 수행

### 예외 발생

### 예외 처리

- 기본 형식
  ```java
  try{
    예외 발생 가능성 코드
  }catch(예외처리클래스 변수명){
    예외 발생시 수행 내용
  }...
  finally{
    예외 발생 여부 상관없이 수행
  }
  ```
- catch 는 `예외 처리 클래스 이름을 달리`해서 여러 개 작성 가능
- finally 는 생략 가능하고 1번만 작성
- 예외 처리 블럭은 `각각의 블럭으로 메모리 할당`
  - try,catch,finally에서 공통으로 사용할 데이터는  
    `try 외부에 생성`

### 예외 종류

- **검사 예외**  
  java 는 프로그램 작성 시에 예상할 수 있는 비정상적인 상태를  
  통지하기 위해서 `반드시 검사해야하는 예외`를 가지고 있음

  **io** 또는 **network**, **database** 관련된 클래스의 메서드를 사용  
   `예외 처리 강제`  
   메서드를 호출할 때 `예외 처리 반드시 진행`

  - 예외 처리 미진행 시 `컴파일 오류 발생`

- **실행 예외**(Runtime Exception)  
  프로그램 작성 시 `처리하지 않아도 되는 예외`

### java 의 예외 처리 클래스

- 예외 관련 최상위  
  **Throwable**
  - Error 와 Exception 클래스가 상속  
    **Error** 는 심각한 예외  
    **Exception** 은 덜 심각한 예외

### Throwable 클래스의 멤버

- String getMessage( ), String getLocalizedMessage( )  
  `예외 인스턴스의 상세 메시지 문자열`로 리턴
- void printStackTrace( )  
  `예외 인스턴스 및 그 백트레이스를 표준 에러 스트림`에 출력
  - 예외가 발생한 지점까지 호출된 메서드를 역순으로 빨갛게 출력

### 예외 처리 방법

- 메서드 내에서 처리
- 메서드 호출한 지점으로 예외 처리 양도 (throws)

### 예외 강제 발생

```java
throw new 예외클래스이름(매개변수)
```

- 매개변수는 보통 메시지

`예외를 강제로 발생`시키는 경우는 **유효성 검사**를 위해  
 **사용자 정의 예외 클래스**를 이용해 `예외의 내용을 자세히 알려주게`

### 호출하는 메서드에 예외를 처리

- 메서드의 매개변수 뒤에 **throws 예외처리클래스이름** 나열 하면  
  `이 메서드를 호출하는 메서드`에서  
  반드시 예외처리클래스에 `해당하는 예외 처리`
  - 하지 않으면 `컴파일 에러 발생`
- Java 애플리케이션 main 메서드는 운영체제가 호출
  - main 메서드에서 throws 로 예외를 던지면 처리 불필요  
    학습 시에는 하는 경우 있으나 **실제는 불필요**

### 사용자 정의 예외 클래스

- `예외 클래스를 상속`받아서 `클래스를 직접 생성해서 사용`하는 것
- 일반 개발 환경에서는 사용 빈도가 적고  
  프레임워크를 개발하는 경우 필수
- 데이터베이스 연동에서는 모든 예외를 **SQLException** 으로 던지는데  
  예외가 발생했을 떄 `예외의 내용 파악이 어려움`
  - spring에서는 이를 세분화해서 문법 오류는 **SyntaxException**
- `Exception 클래스를 상속` 받아 `메시지를 상위 클래스 생성자`에 대입  
  메시지를 원하는대로 출력하도록 하는 경우가 많음

## 5. try ~ resource

```java
try(인스턴스 생성){
  인스턴스 사용
}
```

- 인스턴스를 만들 때 사용된 클래스가 **AutoCloseable 인터페이스**,  
  **Closeable 인터페이스**를 구현한 경우 `직접 자원해제 불필요`
- 정상적인 수행 여부에 상관없이 `try 블럭 종료 시 자동으로 자원 해제`