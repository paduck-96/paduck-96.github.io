---
title: "Java 자료구조"
excerpt: "Java 의 검색 자료구조에 대해 학습하고,
Java의 배열, List, Map 등의 자료구조에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java]

toc: true
toc_sticky: true

date: 2022-12-21
last_modified_at: 2022-12-21
---

### 동적 정렬

- Comparable 인터페이스를 implements 해서 크기 비교 메서드를 만들면  
  정렬에 이용할 수 있는데 이렇게 되면 `한 가지 방법으로만 정렬 가능`
- **Arrays.sort** 를 Overloading 해  
  `Arrays.sort(배열, Compaator \<T>)을 이용`해  
   **두번째 매개변수**로 크기 비교를 해서 정렬을 수행

---

인터페이스 나 추상 클래스 또는 일반 클래스 `상속받아 사용하는 법`

- 인터페이스나 추상 클래스를 상속하는 클래스를 생성해서  
  인스턴스를 생성한 후 사용하는 방법
  ```java
  class Temp implements Comparator<String>{
      메서드 재정의
  }
  Temp temp = new Temp();
  ```
- anonymous 를 이용하는 방법
  ```java
  new Comparator<String>{
    메서드 재정의
    }
  ```
- `하나의 추상 메서드`만 가진 인터페이스의 경우는 lambda 이용 가능

---

### 검색

검색 알고리즘

- **순차 검색**  
  앞에서부터 끝까지 순회하면서 조회, `데이터가 정렬되지 않아서 수행`

  - 검색 속도는 가장 느림  
    데이터가 존재하지 않는 경우 모든 데이터를 조회해야 함

- **제어 검색**  
  데이터를 `정렬해 놓고 검색`하는 방식
  - `이분 검색`  
    중앙의 데이터 와 비교해서 같으면 찾은 것이고  
    작으면 왼쪽 부분에서 동일한 작업 수행  
    크면 오른쪽에서 동일한 작업 수행
- **피보나치 검색**  
  피보나치 수열 이용
- 보간 검색  
  검색 위치를 계산
  ```
  (찾고자하는 데이터 - 최소 데이터)/(가장 큰 데이터-최소 데이터) * 데이터 개수
  ```
- 트리 검색  
  데이터를 저장할 때 트리 구조로 저장
- 블록 검색  
  블록끼리는 정렬이 되어 있고, 블록 내부는 정렬하지 않은 상태에서 검색
- **해싱**  
  `데이터의 저장 위치를 계산`하는 방식

  - 모든 데이터의 접근 속도가 같음

  한 번의 계산만으로 데이터를 조회할 수 있으므로 `가장 빠른 검색`  
  계산은 운영체제가 진행

- Arrays 클래스는 **binarySearch 메서드**를 제공해 이분 검색 가능

---

### 배열/List 학습법

- 전체 순회
- 정렬
- 데이터 존재 여부 나 위치

---

## 2. Collection

### 여러 개의 데이터를 모아 놓은 것, Vector Data

- java.util.Vector 클래스가 존재

- **Collection** 인터페이스가 존재하는데 이 인터페이스는  
  `List 와 Set 의 상위 인터페이스`
- List 나 Set에서 공통으로 사용할 `메서드의 원형을 선언`해 둠
- add, addAll, clear, contains, equals, isEmpty,  
  remove, removeAll, retainAll, size, toArray, iterator
- Generics 가 적용되어 인스턴스 생성 시 `자료형을 기재하면  
해당 자료형`으로 결과 리턴, `자료형 기재하지 않으면 Object` 타입 리턴
  - 사용시 `강제 형 변환`을 해서 사용

## 3. 반복자

- `Collection 의 데이터를 순차적으로 접근`할 수 있도록 해주는 포인터
- enumerator, iterator, cursor(DB), generator 등
- Enumeration 인터페이스 와 **Iterator** 인터페이스 제공되는데  
  Enumeration은 legacy API
- 다음 데이터의 존재 여부 와 실제 다음 데이터를 가져오는 메서드 제공

---

배열, ArrayList, Vector

- 데이터를 물리적으로 연속된 공간에 배치
  - 배열은 메모리에 가장 최소 공간에  
    ArrayList, Vector는 최대 공간에
- Array 는 크기 고정
- ArrayList 와 Vector 는 크기 가변
  - ArrayList 는 thread unsafe
  - Vector 는 thread safe

Linked List

- 논리적으로 연속  
  다음 데이터의 참조를 기억
- 메모리 낭비가 심하고 조회 속도가 느림
- 삽입, 삭제에 장점

Stack

- LIFO
  Top 포인터의 이동

Queue

- FIFO
- 우선순위 부여 가능

Deque

- Stack + Queue

Hashing

- 주소 계산  
  중복 X
- 요소에 이름을 붙일 수 있으면 Map  
  이름을 붙일 수 없으면 Set

---

'

## 4. List 구조

### List 인터페이스

- List들이 공통으로 가져야하는 메서드를 소유하고 있는 인터페이스
- `Generics 가 적용`  
  인스턴스를 만들 때 `실제 자료형 설정 권장`
- `toString 메서드를 재정의` 했기 때문에 데이터 내용 확인 가능
- 메서드
  - add(데이터)
  - add(인덱스, 데이터)
  - set(인덱스, 데이터)  
    수정
  - get(인덱스)  
    인덱스번째 데이터 리턴
  - sort(Comparator \<T> c)
  - remove(인덱스 나 인스턴스)
  - size( )

### Vector, ArrayList 클래스

- `데이터를 물리적으로 연속해서 저장`한 클래스
- **조회**는 LinkedList 보다 우수  
  데이터를 `중간에 삽입하거나 삭제하는 것은 효율 떨어짐`

### LinkedList 클래스

- `데이터를 논리적으로 연속해서 저장`한 클래스

### 자주 사용하는 메서드

### 확장 클래스

- java.util.concurrent.CopyOnWriteArrayList  
  `멀티스레드 환경에서 하나의 리스트`를 사용하고 있는 중  
  다른 곳에서 list에 변경을 가하면 **ConcurrentModification** 예외 발생
  - `데이터 읽기 작업을 할 때 복사본`을 만들어서 수행하는 클래스
  - 멀티 스레드 환경에서 ArrayList 를 사용하고자 할 때  
    이 클래스 `이용시 동시 사용` 문제 해결
- 내부 데이터 수정하지 못하도록 하는 UnmodifiableList 클래스 제공

### Stack

- **LIFO 구조**로 동작하는 List
- 삽입은 push  
  사출은 pop(마지막 데이터 삭제) 과 peek(마지막 데이터 삭제않고 리턴)

### Queue

- `FIFO 구조`의 자료구조
- `인터페이스로 제공`
- 삽입은 offer  
  첫번째 데이터 리턴은 peek 와 poll
- Queue를 implements 한 대표적인 클래스는  
  `데이터의 크기 순으로 조회`하는 **Priority Queue** 클래스
  - `데이터 삽입 시 정렬을 수행`하기 때문에 크기 비교 가능한 인스턴스,  
    (**Comparable** 인터페이스를 implements) 한 인터페이스만 삽입

### Deque

`양쪽 입구에서 삽입 과 삭제`가 가능한 자료구조

- `인터페이스 형태`로 재공  
  ArrayDeque 클래스가 Deque를 구현한 대표적인 클래스

## 5. Set

### 데이터를 `중복없이 해싱`을 이용해서 저장하는 자료구조

- `key 없이 저장`
- 저장 순서도 알 수 없음
- `전체 데이터 순회`는 이터레이터 나 빠른 열거 사용
- 인터페이스로 제공

### 구현한 클래스

- HashSet  
  저장 순서르 알 수 없음
- LinkedHashSet  
  저장 순서를 기억하는 Set
- TreeSet
- 비교 가능한 메서드를 가진 인스턴스만 저장

### 자료구조 활용

- 로또 프로그램 작성

## 6. Map

- `Key 와 Value 를 쌍으로 저장`하는 자료구조
- **Dictioary**(dict) 나 **HashTable** 이라고도 함
- Map은 `인터페이스이고 2개의 Generic`을 사용  
  **Key의 자료형** 과 **Value의 자료형**
  ```java
  Map<키의 자료형, 값의 자료형>변수명
  ```
- Key를 `Set 으로 구성`
  - Key 중복 불가
- 특별한 경우가 아니면 `key의 자료형은 String`
- 데이터 추가  
  **put**(key, value)  
  존재하는 key를 이용하면 수정
- 데이터 1개 가져오기  
  **get**(key)  
  존재하지 않는 key를 사용하면 null 리턴
- 데이터 삭제  
  **remove**(key)
- 데이터 개수  
  **size**()
- 모든 key 리턴  
  Set\<K> **keySet**()

### 구현 클래스

- **HashMap**  
  Key의 순서를 알 수 없음
- **LinkedHashMap**  
  key가 저장한 순서
- **TreeMap**  
  key를 정렬

### 용도

- `데이터를 저장해서 전달`하는 용도로 주로 이용

### MVC Pattern

- M  
  model
- V  
  view
- C  
  Controller
- Model 이 변경되더라도 View에는 영향을 주지 않기 위해 사용  
  `2차원 배열의 경우 Map 형태`로 만드는 것이 디자인 패턴 적용에 용이
  - 데이터 전달은 Map을 대체로 사용

### Map의 활용
