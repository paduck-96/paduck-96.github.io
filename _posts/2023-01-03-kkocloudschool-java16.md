---
title: "Java Spring 의 개념과 사용법"
excerpt: "Java Spring의 개념에 대해 학습하고,
Java Spring을 활용하는 방법에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, Spring, IoC, DI, Annotation]

toc: true
toc_sticky: true

date: 2023-01-03
last_modified_at: 2023-01-03
---

# Spring

## 1. Spring

### Java Enterprise Application 개발을 편리하게 해주는 Open Source

- 경량급 프레임워크
- 빠른 구현 시간
- IoC, DI, AOP 지원

  - **IoC(제어의 역전)**  
    컴포넌트 기반 개발 방법론
    - 클래스는 개발자가 만들지만 `인스턴스 생성/수명 주기`는  
      `Framework 나 Web Container 가 진행`
  - **DI(의존성 주입)**
    - `클래스 내부에서 사용할 인스턴스`를  
      `외부에서 생성해 주입`시켜 주는 것
  - **AOP(관점 지향 프로그래밍)**  
    프로그램을 관점에 따라 나누어 구현한 후 결합

  ## 2. 개발 환경

### JDK

- 설치는 11이상이나, 8 이상이면 큰 영향은 없음  
  Eclipse 최신 버전이 JDK 11 이상 사용, 국내 환경은 8

### IDE

- Eclipse 계열  
  Spring Plug In 설치  
  Spring 전용 Eclipse(STS)

  - Legacy Project 사용의 경우 3버전

  기업이나 단체에서의 Framework(전자 정부 프레임워크, AnyFramework..)

- IntelliJ  
  Spring Boot를 지원하나, Legacy Project 진행 시 별도 설정 필요

## 3. 개발 환경 설정

### STS 3버전 받기

### **lombok**

- 데이터를 표현하는 클래스를 만들 때 반복적으로 생성하는 코드를  
  `컴파일 할 때 자동으로 생성`해주는 라이비ㅡ러리
- https://projectlombok.org/download 에서 jar 파일 다운로드  
  터미널에서 java -jar 로 설정

## 4. lombok 의 annotation

### `생성자`

- @NoArgsConstructor  
  `매개변수가 없는` 생성자
- @AllArgsConstructor  
  `모든 속성 매개변수`인 생성자
- @RequiredArgsConstructor  
  속성 중에서 final 이나 @NonNull 인 속성이 존재하는 경우에  
  외부에 동일한 자료형의 데이터가 있으면 자동으로 주입받는 생성자

### `@Builder`

- `인스턴스 생성시` 생성자 직접 호출하지 않고 `Builder 패턴으로 적용`
- `필요한 부분을 추가하면서 만들어 나가는 패턴`
  - 클래스이름.builder( ).속성이름(값).속성이름(값)...build( )

### @Getter & @Setter

### @ToString

### @EqualsAndHashCode

### @NotNull

- `속성 위에 기재`해 null 여부를 체크해서 null이면 NullPointerException

### @Log

- log 변수 자동 생성

### `@Data`

- 접근자 메서드, ToString, EqualsAndHashCode, RequiredArgsConstructor  
  모두 생성

## 5. Spring Project

### Spring Legacy Project

- `WAS를 별도로 사용`해야하고 `템플릿을 생성해서 사용`하는 프로젝트  
  추가 업데이트는 없지만 아직 국내 환경은 이를 사용

### Spring Boot Project

- `내장된 WAS를 이용`해서 `실행 및 배포가 가능한 수준`의  
  애플리케이션을 빠르게 생성

## 6. 스프링 프로젝트 생성 및 설정 변경

### STS 실행

### Spring Maven 프로젝트 생성

- 오류 발생 시 다른 버전의 STS 설치하거나  
  다른 곳에서 만들어 복사하거나, pom.xml 파일의 내용을 복사

### pom.xml 의 properties 부분 수정

### JRE 버전 변경

## 7. lombok 사용

### pom.xml에 의존성 설정

### 클래스 만들고 속성 확인

### Item 클래스 사용하는 ItemRepository 클래스 만들고 메서드 생성

- persistence 활용

## 8. 다른 클래스의 메서드로 인스턴스 생성

### **Factory 패턴**

- `클래스의 인스턴스를 다른 클래스에서 생성`
- 생성자를 이용하게 되면 `생성자가 Overloading` 된 경우  
  이름이 모두 동일해 `어떠한 방식으로 생성되는지 파악하기 어려움`
- `싱글톤 패턴을 적용하기 편리`
- `매개변수에 따라 다양한 형태의 인스턴스 생성` 가능
  - 상속 관계에 있는 여러 클래스의 인스턴스
- API 문서에서 `알아보기가 어렵고`  
  생성자를 private으로 만들기에 `상속 불가능`

### Factory 메서드의 Naming

- **from**  
  `매개변수 1개`를 받아서 생성
- **of**  
  `매개변수 여러 개`를 받아서 생성
- valueOf  
  of의 자세한 버전
- **sharedInstance**  
  모두 `동일한 인스턴스로 싱글톤`
- instance, **getInstance**  
  `매개변수를 받는 경우 모두 동일한 인스턴스가 아닐 수 있음`
- create, **newInstance**  
  `항상 새로운 인스턴스`를 생성해서 리턴
- **getType**  
  getInstance 와 동일하나 현재 클래스가 아닌  
  `다른 클래스 인스턴스 리턴`  
  `Type 부분이 그에 해당하는 클래스 이름`이 됨

- **newType**  
  newInstance 와 동일하나 현재 클래스가 아닌  
  `다른 클래스 인스턴스 리턴`  
   `Type 부분이 그에 해당하는 클래스 이름`이 됨
- type  
  getType 과 newType 의 간결화된 버전

### 정적 메서드를 이용한 인스턴스 생성

- Repository의 인스턴스 생성해주는 Factory 클래스 만들고 정적 메서드 추가
- `외부에서 인스턴스를 생성할 수 없도록` 패키지에 생성

## 9. **IoC**(Inversion of Control)

- 개발자가 작성한 프로그램이 `재사용 라이브러리`(Framework)의  
  `흐름 제어를 받게되는 소프트웨어 디자인 패턴`

### 사용 이유

- `구현(개발자) 과 수행(Framework)의 분리`
  - 개발자는 **수명주기** 나 **디자인 패턴**에 신경쓰지 않아도 됨
- `다른 모듈 과의 결합`에 대해 신경 쓸 필요가 없음
- `모듈을 변경`해도 다른 시스템에 부작용을 일으킬 가능성 줄어듦

### **Spring Bean**(bean)

- `스프링이 제어권`을 가지고 `생성하고 관계를 설정하는 오브젝트`
  - Android 나 React 에서는 **Component** 라고 함
- 스프링에서 `제어의 역전이 적용된 인스턴스`
- Bean 을 관리하는 인스턴스를 Bean Factory 라 하고  
  `BeanFactory라는 인터페이스`로 제공

### **ApplicationContext**

- `BeanFactory 인터페이스에 여러가지 기능이 추가`된 인터페이스
- **IoC 컨테이너** 이면서 `Singleton을 저장하고 관리`하는 싱글톤 Registery
  - `모든 인스턴스 Singleton으로 생성`  
    Spring은 대부분 서버 환경에서 구동되기 때문이고, 변경 가능
- 여러가지 클래스에서 구현

### **AnnotationApplicationContext**

- `어노테이션을 이용`해서 `Spring Bean을 생성`할 수 있도록 해주는 Application Context
- `Factory 클래스 위`에는 @Configration을 추가하고  
  `SpringBean 생성 메서드 위`에는 @Bean을 추가
- AnnotationApplicationContext 클래스 인스턴스 생성시  
  `생성자`에 Factory 클래스 설정, `인스턴스 생성`시 getBean 메서드 호출
  ```java
  getBean(메서드 이름) // object 타입 리턴
  getBean(클래스이름.class) // 클래스 타입으로 리턴
  getBean(메서드이름, 클래스이름.class) //메서드 호출해 클래스 타입으로 리턴
  ```

### xml을 이용하는 Spring Bean 생성

- Spring Bean Configuration 파일 추가
- ItemRepository 인스턴스 생성하는 코드 수정

### xml을 이용한 ApplicationContext 이용

- **beans 태그**

  - `루트 엘리먼트`
  - `스키마(정의) 관련 정보`를 가짐
  - 하위 태그로 bean, description, alias, import

    - **import**  
      `다른 설정 파일을 가져와서 사용`할 때 필요

      ```java
      <import resource="다른 설정 파일 경로" />
      ```

    - **bean**  
      `클래스를 등록해서 인스턴스를 생성하기 위한 태그`  
      bean 태그 속성

      - class  
        `생성할 인스턴스의 클래스 경로`, **필수**
      - id  
        `구별하기 위한 이름`, `다른 곳에서 참조`할 때 필요
      - init-method  
        인스턴스가 만들어 질 때 호출되는 메서드
      - destroy-method  
        인스턴스 소멸될 때 호출되는 메서드
      - lazy-init  
        `지연 생성 옵션`
        - `모바일 생태계`에서 매우 중요

      ```java
      public class T(){
        public T(){
            //Date date = new Date()
        }
        public Date getDate(){
            if(date==null){
                date = new Date();
            }
            return date;
        }
      }
      T t = new T();
      t.getDate().toString() //클래스 내 변수가 필요시 메서드를 통해 생성
      ```

      - scope  
        `수명주기`로 기본은 `singleton`  
        prototype, request, session 등 설정 가능

      - abstract  
        추상, 상속시킬 목적으로 설정
      - parent  
        상속받을 때 상위 클래스 설정
      - factory-method  
        `생성자 아닌 메서드로 생성`할 때 설정
      - depends-on  
        다른 `bean을 만들고 만들어져야` 하는 경우 설정, 콜백

## 10. **DI**(Dependency Injection)

### 개요

- **의존성**  
  `인스턴스 내부`에서 `다른 클래스의 인스턴스를 사용`하는 것
- **주입**  
  `내부의 속성을 대입`받는 것
- **의존성 주입**  
  `인스턴스 내부`에서 사용하는 `다른 클래스의 인스턴스`를  
  직접 생성하지 않고 `외부에서 생성된 걸 대입`받아 사용

### 목적

- 인스턴스의 `생성 과 사용의 관심을 분리`
- `가독성`의 증가
- `재사용성` 증가

### 전제

- `사용되는 서비스` 객체 필요
- 사용하는 `서비스에 의존하는 클라이언트` 객체 필요
  ```
  Repository -> Service -> Controller
  // Controller는 주입되지 않음
  ```
- `클라이언트의 서비스 사용 방법`을 정의하는 인터페이스
  - `템플릿 메서드 패턴` 적용  
    **Repository** 와 **Service에** 적용
    - **Repository를** 인터페이스로 만들면 프레임워크가 구현
- 프레임워크  
  서비스를 생성하고 클라이언트를 주입하는 책임을 갖는 주입자

### 주입 방법

- `생성자 이용` 방법
  ```java
  <bean id="아이디" class="클래스 경로">
    <constructor-arg value="값" 또는 ref="다른 bean id" />
  </bean>
  ```
  - 타입을 설정하지 않으면 `기본적으로 String`
  - 생성자의 매개변수가 여러 개라면 여러 개 생성하면 되는데  
    이때 순서는 의미 없고 `자료형이 일치하는 매개변수 이용이 중요`
  - `index 속성을 추가해서 몇 번째 인지 설정`하는 것이 가능
- `setter 이용` 방법
  ```java
  <bean id="아이디" class="클래스 경로">
    <property name="속성 이름" value="값" 또는 ref="다른 bean id" />
  </bean>
  // 속성의 이름은 변수 명이 아니고
  // set을 제외한 부분의 첫글자로 변경
  ```
- `속성에 작성할 때는 문자열의 형태`로 값을 설정  
  `태그 와 태그 사이에 설정할 때는 값`만 설정하면 됨
  ```java
  <propperty name="age" value="29"/>
  ```

### Spring Bean Configuration 파일에 작성해 main에서 사용

- xml에 bean 생성 코드 작성
- 다른 bean을 참조하도록 설정

### `Collection Type` 의 주입

- \<**list**>  
  java.util.List 타입이나 배열에서 사용
  ```java
  <list value-type="자료형">
  <value>값</value>...</list>
  ```
  - 환경 설정에서 주로 이용
- \<**set**>  
  java.util.Set 타입
  ```java
  <set value-type="자료형">
  <value>값</value>...</set>
  ```
- \<**map**>  
  java.uti.Map 타입
  ```java
  <map>
   <entry>
     <key><value>키값</value></key>
     <value><value>값</value></value>
   </entry>
   ...
  </map>
  ```
- \<**properties**>  
  Properties 타입
  ```java
  <props>
    <prop key="값">값</prop>
    ...
  </props>
  ```

## 11. 어노테이션을 이용한 의존성 주입

### **@Autowired**

- `속성 위에 기재`해서 `동일한 자료형의 bean이 존재`하면 자동 주입
- 동일한 자료형의 bean이 없거나 2개 이상이면 예외 발생
- `필수 해제`  
  @Autowired(required=falses)로 설정
- `동일한 자료형의 bean이 2개 이상`인 경우  
  @Autowired  
  @Qualifier("사용할 bean의 아이디")

### **동일한 기능**을 해주는 어노테이션

- @Resource(name="bean의 아이디")
- @Inject 와 @Named  
  javax.inject 라이브러리의 의존성 설정이 필요

### 생성자에서 생성

- 매개변수가 있는 생성자 위에 @ConstructorProperties({"bean아이디"})
- 주입받을 속성에 final 적용 후 클래스 상단에 @RequiredArgsConstructor

### 어노테이션 과 xml을 같이 사용하고자 하는 경우 설정

- **context : annotation-config** 가 설정 필요
  - Spring boot에서는 상기 설정 불필요

### 관계형 DB 사용하는 Web APP 구조

- `Entity 클래스`로 관계형 `DB 테이블과 연동`
- 데이터베이스 `연동 관련 메서드 선언할 인터페이스`  
  이를 구현한 클래스(Impl)
- Service / Controller / View Layer 사이의  
  `데이터 교환을 위한 DTO 클래스` 생성
- 사용자의 `요청 메서드 원형을 가진 Service 인터페이스` 생성  
  이를 구현한 Service 클래스 생성
- 사용자 요청에 따라 필요한 `Business Logic 호출`  
  그 `결과를 뷰에게 전달하는 Controller`

### @Autowired 대체

- @RequiredArgsConstructor 사용
  - 어노테이션을 클래시 위에 작성하고 주입 받는 속성은 final 설정
