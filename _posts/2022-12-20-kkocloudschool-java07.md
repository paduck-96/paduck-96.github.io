---
title: "Java API"
excerpt: "다양한 자바 API에 대해 학습하고,
Java String 과 Array 메서드에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, API]

toc: true
toc_sticky: true

date: 2022-12-20
last_modified_at: 2022-12-20
---

# Java API

# `java.lang.package`

기본 패키지

- import 하지 않고 사용 가능
- String > java.lang.String
- **Wrapper 클래스** 와 **String** 그리고 **시스템 관련** 클래스 존재

## 1. java.lang.Object 클래스

- 자바의 `최상위 클래스`
- 자바의 `모든 클래스는 이 클래스로부터 상속`

### JDK 의 API 클래스에서 Object 사용되는 경우

- `Object 클래스가 메서드의 매개변수`로 사용되는 경우  
  `메서드의 매개변수로 어떠한 데이터`도 가능
- 메서드의 `리턴 타입이 Object`인 경우  
  반드시 `원래의 자료형으로 형 변환`해서 사용

### Object 클래스의 메서드

`모든 인스턴스가 사용 가능`

- **Object clone( )**  
  `복제를 위한 메서드`로 `재정의 해서 사용`

  - clone 을 재정의 할 때 `Cloneable 인터페이스를 implements` 권장
    - API 클래스 중 Cloneable를 implements 한 클래스는  
      **clone 이라는 메서드** 호출하면 `깊은 복사 수행한 결과 리턴`
  - **자료구조** 와 관련된 클래스들은 `대부분 구현`  
    **Wrapper** 나 **String 클래스**는 복사 생성자 를 이용해 복제
    - 복사 생성자  
      `자신과 동일한 데이터 타입을 매개변수로 받는 생성자`

- **boolean equals**(Object obejct)  
  참조가 아니라 `내용을 비교하고자 할 때 재정의`해 사용

- **int hashCode**( )  
  `해시코드를 리턴`하는 메서드인데 `재정의 가능`

  - 리턴 값은 실제 해시코드가 아님  
    **재정의**하여 `값이 같으면 equals 의 리턴이 true`가 되게 하는 걸 권장

- **void finalize**( )  
  `인스턴스가 메모리에 정리`될 때 호출되는 메서드로 `재정의` 해서 사용

  - **소멸자**

- **String toString**( )  
  `인스턴스를 문자열로 표현`하게 제공되는 메서드로 `재정의` 해서 사용

  > 출력 메서드에 인스턴스 참조 대입시 이 메서드의 리턴 값 출력

- **notify( )**, **notifyAll( )**, **wait( )**  
  쉬고 있는 `스레드를 꺠우는` 메서드  
  동작 중인 `스레드를 쉬게` 해주는 메서드

### 인스턴스의 등가성 판단

- **=**  
  `참조가 같은 지` 여부를 리턴
- **equals( )**  
  `내용이 같은지` 여부를 리턴
  - `재정의` 해서 사용

### weak copy 와 deep copy

- **얕은 복사**  
  참조를 대입하지 않고 `내부 데이터를 복사해 복사본을 만듦`  
  `재귀적으로 복사하지 않는 것`

  - `참조하는 형태의 데이터 안에 다시 참조하는 데이터`가 있는 경우  
    `복사본이 원본에 영향`을 줄 수 있음

- **깊은 복사**  
  `재귀적으로 복사본`을 만들어서 주는 것  
  `복사본이 원본에 영향을 줄 수 없음`

### equals( ) 와

## 3. Wrapper Class

- 자바의 `기본형 데이터를 인스턴스로 생성`할 수 있게 하는 클래스
  - 참조를 저장하기는 하지만 `null 은 대입할 수 없는 구조`
- 기본형 과 다른 자료형은 데이터의 의미가 달라 `형 변환 불가`

### 기본 자료형의 데이터를 인스턴스화 해 저장할 수 있는 클래스

- boolean(Boolean)  
  byte(Byte)  
  short(Short)  
  char(Character)  
  int(Integer)  
  long(Long)  
  float(Float)  
  double(Double)

- Object 클래스로부터 상속을 받았기 때문에  
  `인스턴스 생성해 Object 클래스 타입의 변수에 대입`  
   `null 도 저장`할 수 있고  
   클래스이므로 자신과 관련된 `메서드 나 속성 소유` 가능
- **DB 에서** null 저장 가능한 컬럼에는 기본형 보다는 Wrapper Class 사용

### 인스턴스 생성

- Default Constructor 가 존재하지 않음  
  new Integer( ) 는 에러
- `매개변수가 1개인 생성자 이용`  
  new Integer(10);
- **다른 종류의 데이터**(문자열)을 이용해 생성  
  static 메서드인 `parse자료형(데이터) 를 이용`
  - integer.parseInt("10");

### Wrapper Class 의 데이터 문자열로 변환

- **toString( ) 메서드** 호출

### Wrapper 클래스 와 String 클래스는 내부 데이터 수정 불가

- 데이터 변경 희망 시 `새로운 공간을 할당` 받아서 `데이터 저장 후 참조 기억`

### Auto Boxing 과 Auto UnBoxing

- **Auto Boxing**  
  기본형 데이터를 자동으로 `Wrapper Class 타입으로 변환`

```java
int a = 10; //기본형
Integer i = new Integer(a) //기본형을 Wrapper 로

Integer k = a //a 부분을 new Integer(a)로 변환
```

- **Auto Unboxing**  
  Wrapper 클래스 데이터를 `기본형으로 변환`

  ```java
  Integer x = new Integer(10);
  int y = x.intValue();

  int z = x //메서드 호출하지 않고 직접 대입
  ```

### 메서드 재정의

- **equals** 와 **hashCode** 메서드가 재정의
- 크기 비교하는 **compare** 메서드도 재정의
- **toString** 메서드도 재정의  
  저장하고 있는 데이터 문자열로 리턴

### BigInteger 와 BigDecimal

- **BigInteger**  
  `아주 큰 정수`를 저장하기 위한 자료형  
  `내부는 int 배열`로 구성  
  `문자열로된 숫자를 생성자의 매개변수`로 받아서 사용  
  Long 이 64비트라서 사용빈도 낮음
- **BigDecimal**  
  `정밀한 숫자를 저장`하기 위한 자료형  
  `소수의 연산` 때문에 만들어진 클래스

  - float 이나 double 로는 정밀한 소수 계산 어려움

  `숫자로 된 문자열을 생성자의 매개변수`로 받아서 생성

  - 일반 정수 나 실수로 변환할 때  
    intValue, floatValue, doubleValue 등 이용
  - 오라클 number로 만든 숫자 데이터에 자료형 미설정 시 BigDecimal  
    MyBatis로 Map으로 테이블 매핑하면 발생

## 4. String 클래스

### 문자열 클래스

- **String**  
  `고정된 문자열` 저장  
  문자열을 `수정할 수 없음`
- **StringBuffer**  
  `변경 가능한` 문자열 저장  
  Multi `Thread 에 Safe`

  - 사용시 다른 쓰레드의 `사용 유무 확인`

  Legacy Class

- **StringBuilder**  
  `변경 가능`한 문자열 저장  
  Multi `Thread 에 Unsafe`

  - 사용시 다른 쓰레드의 `사용 유무 미확인`

- `문자열 연산`을 많이 해야 하는 경우에는 **Builder** 를 이용해 연산하고  
  `String으로 변환`해서 사용하는 것을 권장

### 인스턴스 생성

- **문자열 리터럴**을 이용해서 생성이 가능하고 **생성자**를 이용해서도 가능

### 메서드

- `int length( )`  
  문자열의 길이를 리턴
- `char charAt(int idx)`  
   idx 번째 문자 리턴
  > 문자열이 변경되는 메서드는 전부 변경 후 리턴

### 문자열 처리 시 고려 사항

- 영문의 경우 `대소문자 구분`
- `좌우 공백` 제거
- `문자열 분할`  
  **위치**를 가지고 분할, 특정 **패턴**을 찾아 분할
- 특정 문자열 `패턴의 존재 여부` 확인  
  **정규식** 이용
- 문자열의 `동일성` 여부  
  **equals**
- 문자열의 `크기` 비교  
  **compare**
- 여러 데이터를 `조합해 하나의 문자열`  
  **String.format( )**  
  printf 와 사용법 동일
- 한글의 경우는 `인코딩`  
  **getBytes( )** 으로 `인코딩 방식 설정하여 byte 배열`로 리턴  
  **new String(바이트 배열,"인코딩 방식")**으로 `문자열로 리턴`
  - 이기종 간의 통신에서 사용  
    utf-8 사용으로 흔하지 않은 문제

## 5. System 클래스

- `실행 환경과 관련된 속성 과 메서드를 제공`하는 클래스
- `공개된 생성자가 없음`  
  외부에서 인스턴스 생성 할 수 없음
- **생성자가 없는 경우 확인 사항**  
  `interface 나 abstract class` 인지 확인

  - 인스턴스를 만들 수 없어 보이지 않음

  `모든 멤버가 static` 인지 확인

  - 인스턴스 생성 불필요

  `자기 자신을 리턴하는 static 메서드`가 있는지 확인

  - 디자인 패턴 적용된 클래스

  `자신의 이름 뒤에 Builder 나 Factory 붙는 클래스`가 있는지 확인

  - 직접 생성이 번거로워 대신 생성해주는 클래스를 이용

> System 클래스는 모든 멤버가 static이라 인스턴스 생성 불필요  
>  &nbsp;&nbsp;생성자 숨김

- 속성  
  내부 객체

  - **in**  
    표준 입력 객체  
    일반적으로 키보드
  - **out**  
    표준 출력 객체  
    일반적으로 모니터
  - **err**  
    표준 에러 객체  
    out처럼 출력으로 이용 가능하나  
    출력 시 빨간색 표시, out과 별개 동작해 출력 순서 알 수 없음

- 메서드

  - `현재 시간`을 밀리초 나 나노초 단위로 리턴

    - currentTimeMillis  
      nanoTime

  - `환경 설정 값`을 가져오는 메서드
    - get Property  
      getenv
  - `해시 코드 리턴`해주는 메서드
    - identityHashCode

## 6. Math

- 수학 관련 메서드를 소유한 클래스
- `모든 멤버가 static` 이라서 인스턴스 생성 불필요
  - 공개된 생성자 없음
- 클래스의 메서드가 리턴하는 결과는 `운영체제 별로 다를 수 있음`
- 운영체제의 특정 기능을 사용하지 않고  
  공통 기능만을 이용해 연산하는 **StrictMath** 클래스도 제공

## 7. Runtime

- 프로세스 실행과 관련된 클래스
- `Singleton 패턴`으로 디자인된 클래스로 **getRuntime**( )로 인스턴스 생성
- **exec** 메서드를 이용하면 특정 프로세스 실행 가능

## 8. Fast Enumeration(빠른 열거)

- `Collection 의 데이터를 순서대로 빠르게 접근`하는 것
- `배열 과 Collection 데이터`들에서 가능
  - jdk 1.8 에서는 이 작업도 내부적으로 구현해서  
    더 빠르게 접근하는 **Stream API** 추가

```java
for(임시변수 : 배열 또는 COllection 데이터){
    Collection 데이터 순차 접근하며 수행할 작업
}
```

## 9. Generics

### Template Programming(일반화 프로그래밍)

- `데이터 타입`을 미리 결정짓지 않고 `실행할 때 결정`
- 클래스 내부에서 사용할 `데이터 타입`을 `인스턴스 생성 시 결정`
- 초창기에는 Object 클래스로 많이 구현했는데, 1.5버전에서 추가
- 자료구조 나 알고리즘 구현시 동일한 알고리즘에도 불구하고  
  `데이터 자료형이 달라 여러 개 구현해야 하는 번거로움 해결`
- 클래스 만들 때 `미지정 자료형 사용`하고  
  인스턴스 만들 때 `실제 자료형 설정`
  > Object 클래스 사용보다 에러 발생 가능성 낮아짐

### Generics 선언 형식

```java
class 클래스이름 <미지정 자료형 이름 나열>{
  미지정 자료형
}
// 인스턴스 생성
클래스이름<미지정 자료형 실제 자료형 나열> 변수 =
  new 생성자<>(매개변수 나열);

  //생성자 호출 시 <>만 해도 되고 자료형을 다시 나열해도 됌
```

- 인스턴스 생성시 자료형 기재하지 않으면 경고 발생  
  `데이터는 Object 클래스 타입`으로 간주
  - `데이터 형 변환해서 사용`해야 함

### 특징

- `변수를 선언`하거나 `메서드의 매개변수` 그리고 `리턴 타입`으로 사용 가능
- `static 속성이나 메서드에는 적용 안됨`
  - **Generic** 은 인스턴스 생성 시 자료형 결정되나  
    **static** 멤버는 클래스를 메모리에 로드할 때 만들어지기 때문
- `미지정 자료형을 이용`해서 배열 변수를 만드는 것은 되지만  
  `배열을 만드는 것은 안 됨`
- 미지정 자료형을 만들 때  
  <자료형이름 **extends** 인터페이스 나 클래스>로 작성 시  
  `인터페이스나 클래스를 상속하는 클래스의 자료형`만 가능
  - 자료형이 일치하지 않으면 실행 시 ClassCast예외 발생
- 미지정 자료형을 만들 때  
  <자료형이름 **super** 인터페이스 나 클래스>로 작성 시  
  `동일한 자료형 또는 상위 타입의 자료형`만 가능
- **<?>** 로 사용 시 모든 자료형 가능
  - `자료형 이름만 기재해도 기본적으로 모든 자료형` 가능

## 10. enum(나열형 상수, 열거형 상수)

### 개요

- `상수의 모임`을 만드는 것
- `선택을 제한`하기 위한 목적으로 사용
  - jdk api에서는 enum 보다는 static final 상수 이용

### 선언

```java
enum 이름{
  상수 이름 나열;
}

이름.상수이름 //사용
```

- 변수를 만들 때 `enum의 이름을 이용해서 생성`하면  
  `enum에 정의된 상수만 대입`이 가능

### 특징

- 생성자 와 일반 메서드 생성 가능
- 자바는 `enum을 하나의 클래스`로 간주하고 각 `상수는 하나의 인스턴스`
- **==** 으로 일치 여부를 확인할 수 있고, **compareTo**( )로 크기 비교

## 11. Annotation

- `@ 다음에 특정한 단어를 기재해서 주석`을 만들거나  
  `자바 코드에 특별한 의미 또는 기능을 부여`하는 것

### 용도

- `컴파일러가 특정 오류 체크`하도록 지시
- `Build 나 Batch 를 할 때 코드 자동 생성`  
  프레임워크 가 이 기능을 많이 사용

- `Runtime 시 특정 코드 실행`

### JDK가 기본적으로 제공하는 것

- **@Override**  
  `오버라이딩`을 한다
  - 메서드가 상위 클래스 나 인터페이스에 없으면 컴파일 에러
- **@Deprecated**  
  더 이상 `사용하지 않는 것`을 권장한다
- **@SupressWarning**  
  `경고를 발생시키지 않도록` 한다
- **@SafeVarargs**  
  `varargs 에 Generics` 를 적용한다
- **@FunctionInterface**  
  `함수형 인터페이스라`는 의미다
  - `추상 메서드 하나`로 만들어진 인터페이스  
    `람다 적용이 가능`한 인터페이스
  - 안드로이드에서는 이 인터페이스로  
    anonymous class 구현 시 람다식으로 변경

# java.util 패키지

자료구조 클래스 와 자주 사용되는 util 관련 클래스가 존재하는 패키지

import가 안되어 있으므로 `import 해서 사용`해야 클래스 이름만으로 사용 가능

## 1. Arrays 클래스

- `배열을 조작`하기 위한 클래스
- `static 메서드만 존재`하기 때문에 공개된 생성자는 없음

### **copyOf**(원본 배열, 데이터 개수)

- `원본 배열에서 데이터 개수만큼 복제를 해서 배열로 리턴`
- 배열의 데이터 개수보다 `더 많은 데이터 개수`를 기재하면  
  나머지는 `기본 값으로 채워서` 리턴
- 내부적으로는 System 클래스의 **arraycopy** 라는 메서드 이용

### **toString**(배열)

- 배열의 `모든 요소를 toString 호출`해 그 결과를 `하나의 문자열`로 리턴

### **sort**

- `데이터를 정렬`해주는 메소드
- 배열만 대입하는 메서드는 크기 비교를 해서 오름차순 정렬 수행  
  `숫자 데이터는 부등호`를 이용해서 비교  
  이외의 `데이터는 Comparable 인터페이스의 compareTo( )`
  - 숫자 데이터가 아니라면 `compareTo( ) 재정의`  
    재정의 하지 않으면 ClassCast예외 발생
  ```java
  //compareTo( ) 메서드는 정수 리턴
  //양수 리턴시 데이터 순서 변경
  //0이나 음수 리턴시 데이터 순서 미변경
  ```
- 배열 과 Comparator<T> 인터페이스를 구현한 인스턴스 대입 메서드는  
  배열의 데이터를 Comparator에 정의된 대로 오름차순 정렬

  - `인터페이스를 구현한 클래스의 인스턴스를 대입`할 때는  
    대부분의 경우 **anonymous class** 를 사용

- 데이터를 정렬하기 위해서는 `크기 비교를 위한 방법이 제공`되어야 하는데  
  그렇지 않은 경우 `인터페이스를 이용`한다
  - `데이터 클래스에 Comparable 인터페이스 구현 필요`