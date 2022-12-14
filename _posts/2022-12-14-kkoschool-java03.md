---
title: "반응형 웹, 자바스크립트의 문법 일부"
excerpt: "반응형 웹에 어울리는 flex와 grid. 자바스크립트에 대한 설명과 기본 문법까지..."

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, responsive web, javascript]

toc: true
toc_sticky: true

date: 2022-12-14
last_modified_at: 2022-12-14
---

...

## 9. 할당 연산자

- =  
  오른쪽의 데이터를 왼쪽의 변수가 가리키도록 하는 연산자
- 연산자 =  
  왼쪽의 데이터 와 오른쪽의 데이터를 연산자를 이용해서 연산을 수행하고 그 결과를 왼쪽의 변수가 가르키도록 하는 연산자

  ```java
  int a = 10;
  a+=20;
  System.out.println(a) // a=30

  ```

- 변수의 값을 1 증가시키는 방법
  - a++; > a+=1 > a = a+1;

## 10. 문자열의 + 연산자

- 문자열은 모든 데이터와 + 연산 가능
- 기본형 데이터의 경우는 무낮열로 형 변환을 해서 결합  
  나머지 데이터는 toString 메서드를 호출해 그 결과를 결합하는 방식
- 문자열 리터럴은 static 영역에 저장되기 때문에 내용 변경 안됨  
  문자열을 + 로 결합하면 메모리 낭비 발생  
  문자열 + 연산은 자주 사용하지 않는 것을 권장하고 String.format 이나 StringBuilder 사용 권장
  - java 1.7 부터 문자열 + 연산을 자바가 내부적으로 Builder 를 이용해 수행함

---

# 제어문

## 1. java 에서의 import

- java 는 애플리케이션을 실행하면 jdk 가 제공하는 클래스는 jre가 제공을 해주고  
  우리가 만든 모든 클래스를 jvm에 로딩하여 실행
- 근본적으로 자바는 import 과정이 필요 없엄
  - 메모리 가져오는 개념이 아니고 줄여쓰기 위한 개념

## 2. 콘솔에서 입력 받기

### java.io.BufferedReader 클래스

- 이 방법은 예외처리 강제

### java.util.Scanner 클래스

- .next자료형( ) 으로 문자열을 받아 자료형으로 변환 후 리턴

## 3. if

표현식 : 변수, 상수(리터럴은 제외), 연산식, 메서드 호출 구문

### 형식

```java
if(boolean이 리턴되는 표현식){
  true 수행 내용
}else if(){
  ...
}else{
  ...
}
```

- if는 1번만
- if는 필수
- else if 는 0번 이상 여러 번 작성 가능
- else if는 dead code(결코 수행되지 않는 code) 조건을 만들면 안 됌
- else 는 0번이나 한 번만 작성
- else if 와 else 가 같이 사용되는 경우에는 else 에서 예외적인 상황 처리
  - try~catch 가 없을 때는 else 로 예외 처리
