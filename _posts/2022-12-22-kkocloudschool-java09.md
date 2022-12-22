---
title: "Java Util 클래스 와 동기화 처리"
excerpt: "Java util 클래스의 나머지를 학습하고,
자바 thread와 비동기 처리에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, thread, util classes]

toc: true
toc_sticky: true

date: 2022-12-22
last_modified_at: 2022-12-22
---

## 7. Properties
### 이름 과 값의 쌍으로 데이터를 저장하는 구조
- Map은 Key 와 Value에 모든 자료형을 사용할 수 있는데 Key 와 Value에 String 만 가능
- 텍스트 파일에 이름 과 값의 쌍으로 작성한 후 읽어서 사용할 때 이용하던 클래스
현재도 Spring Framework에서는 이 형태를 사용  
  - 최근에는 XML 이나 YML(yaml - 야믈) 형태로 사용 - 환경 설정에 많이 이용
- 메서드
  - String **getProperty**(String key)
  - String **setProperty**(String key, String value)
  - void **store**(OutputSteream out, String comment)  
스트림에 일반 텍스트 형식으로 저장
  - void **storeToXML**(OutputSteream out, String comment)  
  스트림에 XML(태그 와 유사) 형태로 저장
  - **Enumeration**\<K> **keys**()  
  모든 키들을 접근할 수 있는 이터레이터를 리턴
  - **Enumeration**\<V> **values**()  
  모든 값들을 접근할 수 있는 이터레이터를 리턴


## 8. Legacy Collection
- 예전에 만들어진 Collection API Class
- Vector -> ArrayList  
Enumeration -> Iterator  
Stack -> List  
Dictionary Hashtable -> HashMap  
Properties -> HashMap  
BitSet -> HashSet

## 9. Collection 의 동기화
- Collection 클래스들을 사용할 때 `다른 스레드가 수정 중`인 경우 대기했다가 사용할 때  
Collections 클래스를 이용해서 생성
- **Collections.synchronizedCollection**(Collection 클래스의 인스턴스)를 호출하면 동기화된 Collection을 리턴,  
`리턴된 인스턴스를 사용하면 다른 곳에서 수정 중인 경우 대기`

- 다른 곳에서 수정할 수 없도록 만들고자 할 때는 **Collections.unmodifiableCollection**(Collection 클래스의 인스턴스)  
호출해서 리턴된 인스턴스를 사용

## 10. Random 클래스
### 랜덤한 데이터를 생성해주는 클래스
- 정수형 난수의 경우는 `정수 범위 내`에서 생성하고 실수형 난수의 경우 `0.0 ~ 1.0 사이의 난수`를 생성
  - 실수형 난수는 Math.random()으로 가능
- 랜덤을 추출할 때는 **seed** 라고 하는 숫자를 설정한 후 이 `seed를 기반으로 난수표`를 생성,  
 그 난수표에서 `하나씩 숫자를 읽어오기`
`seed를 예측할 수 없다면` 랜덤한 데이터이고 `seed를 알고 있다면` 순서대로 숫자를 읽어옴
  - java는 기본적으로 seed 가 랜덤입니다.
- 랜덤을 사용하는 대표적인 경우가 머신러닝에서 샘플 데이터 추출할 때

### 생성자
- Random()  
seed가 랜덤
- Random(long seed)  
seed 고정

### 메서드
- float nextFloat()
- boolean nextBoolean()
- int nextInt()  
정수 범위 내에서 리턴
- int nextInt(int n)  
0 ~ n 사이의 값을 리턴

### 활용

## 11. StringTokenizer
### 문자열을 분할해주는 클래스
- String 클래스의 split 메서드를 조금 더 잘 활용할 수 있도록 만든 클래스

## 12. java.util.Date
- `날짜 관련 클래스`로 javascript의 Date 와 동일
  - javascript의 Date가 Java의 Date를 가져가서 사용
- 1970년 1월 1일 자정(**epoch time**)을 기준으로 지나온 시간을 정수로 관리
  - 밀리초 단위
- 2038년까지만 사용 가능  
많은 메서드가 Deprecated
- `관계형 데이터베이스의 날짜 자료형 과 매핑` 가능

### 생성자
  - Date()  
  현재 시간
  - Date(long timeInMillis)  
  epoch 시간을 가지고 설정
  - Date(int year, int month, int date)  
  년 - 1900년에서 지나온 연도  
  월 - 현재 월에서 -1(작성 시)  
  일

### 메서드
- get 으로 시작하는 메서드를 이용해서 `특정값을 추출`하는 값이 가능
- `toString 메서드가 재정의`되어 있어서 바로 출력 가능합니다.


## 13. java.util.Calendar
- 추상 클래스
- `GregorianCalendar 클래스`가 하위 클래스
### 인스턴스 생성
  - Calendar.getInstance()  
  현재 날짜 및 시간
  - new GregorianCalendar() 나 년월일 또는 년월일시분초 를 설정해서 생성
    - 월만 -1을 해서 설정

### 날짜 및 시간 가져오기 와 설정
- 가져오기  
get(Calendar.상수)
- 설정하기  
set(Calendar.상수, 값)
- add 메서드를 이용해서 추가하거나 감소시키는 것도 가능

### Data 와의 변환
  ```java
  Date 변수 = new Date(Calendar.getTimeInMillis());
  Calendar객체.setTime(Date객체)
  ```
## 14. java.text.SimpleDateFormat
- `java.util.Date 타입`을 받아서 `원하는 형식의 문자열로 변환`해주는 클래스
### 사용
  ```java
  (new SimpleDateFormat(String 날짜 서식)).format(Data 인스턴스)
  ```

## 15. 날짜 와 시간 관련 클래스
- java.sql.Date, java.sql.Time, java.sql.DateTime, java.sql.Timestamp 
- java.time.LocalDate, java.time.LocalTime, **java.time.LocalDateTime**  
  - 시간대 설정 가능
최근의 JPA 같은 프레임워크에서는 java.util.Date 대신에 java.time.LocalDateTime를 사용

## 16. 정규 표현식
### 특정 문자열 패턴을 만드는데 사용하는 클래스
- java에서는 java.util.regex 패키지에 **Match 클래스** 와 **Pattern 클래스**를 제공
- Pattern 클래스로 `정규 표현식 인스턴스를 생성`  
matcher 메서드를 이용해서 `처리를 한 후 Matcher 클래스로 결과를 사용`합니다.
- 문자열의 유효성 검사에 많이 이용 

# Thread
## 1.동기(Synchronize) 와 비동기(ASynchronize) 
- 동기
작업을 `순서대로 하나씩` 처리
- 비동기  
하나의 작업이 `완료되기 전에 다른 다른 작업을 처리`할 수 있는 방식

## 2.Thread
- Task  
작업, Process 와 Thread 모두를 Task
- Process  
`운영체제에서 자원을 할당`받아서 독립적으로 수행되는 현재 `실행 중인 프로그램`
- Thread  
`Process 내에서 생성되는 작업의 단위`로 실행 중 다른 작업으로 `제어권`을 넘길 수 있습니다.
  - 비동기적으로 작업을 수행하고자 할 때 사용

### Single Thread & Multi Thread
- Single Thread  
현재 실행 중인 스레드가 1개 인 경우
- Multi Thread  
현재 실행 중인 스레드가 2개 이상인 경우, 하나의 jvm 안에서 동작

### Multi Thread Programming의 장점
- `CPU의 사용률`을 향상

### Multi Thread Programming의 단점
- 동기화 나 교착 상태 등을 방지하기 위한 코드가 삽입되어야 해서 프로그래밍이 어려워짐
- 너무 많은 스레드를 생성하면  
**Thrashing**(스레드가 교체되는 시기에 현재까지 작업한 내용을 저장하고 수행할 스레드의 Context를 불러오는 시간 증가)  
현상이 발생해서 성능이 저하될 수 있음

### Thread 생성 및 실행
- **Thread** 클래스 나 **Runnable** 인터페이스를 이용할 수 있음
- Thread 클래스  
`상속받아서 public void run 이라는 메서드`에 스레드로 수행할 내용을 작성  
`인스턴스를 생성`하고 `start()`를 호출
- Runnable 인터페이스  
`Runnable 인터페이스를 구현한 클래스`를 만들어서 `public void run 이라는 메서드`에 스레드로 수행할 내용을 작성  
`인스턴스를 만든 후` 이 인스턴스를 `Thread 클래스의 생성자에 대입`해서 인스턴스를 만든 후 `start()를 호출`

## 3. Thread를 만들어서 실행하는 것 과 그렇지 않은 경우의 차이
  ```java
		//스레드를 사용하지 않은 경우
		new Thread() {
			public void run() {
				for(int i=0; i<10; i++) {
					try {
						Thread.sleep(1000);
						System.out.println(i);
					}catch(Exception e) {}
				}
			}
		}.run();
		
		new Thread() {
			public void run() {
				for(int i=0; i<10; i++) {
					try {
						Thread.sleep(1000);
						System.out.println(i);
					}catch(Exception e) {}
				}
			}
		}.run();
		
		//스레드를 사용하는 경우
		new Thread() {
			public void run() {
				for(int i=0; i<10; i++) {
					try {
						Thread.sleep(1000);
						System.out.println(i);
					}catch(Exception e) {}
				}
			}
		}.start();
		
		new Thread() {
			public void run() {
				for(int i=0; i<10; i++) {
					try {
						Thread.sleep(1000);
						System.out.println(i);
					}catch(Exception e) {}
				}
			}
		}.start();
  ```

## 4. Thread 클래스
### 생성자
- Thread()  
name은 jvm이 설정
- Thread(String name)  
name을 설정해서 생성
- Thread(Runnable runnable)  
Runnable 인스턴스를 받아서 생성
- Thread(Runnable runnable, String name)

### 메서드
- static void **sleep**(long msec) throws InterruptedException  
현재 스레드를 mses 밀리 초 동안 대기
- void **run**()  
스레드가 수행할 내용을 가진 메서드
- void **start**()  
스레드를 시작시키는 메서드

## 5. 스레드 생성 과 시작
  ```java
    //Thread 클래스로부터 상속받는 클래스
    class ThreadEx extends Thread{
        @Override
        public void run() {
            //1초마다 스레드 이름을 10번 출력
            for(int i=0; i<10; i++) {
                try {
                    Thread.sleep(1000);
                    System.out.println(getName());
                }catch(Exception e) {}
            }
        }
    }

    public class ThreadCreateMain {

        public static void main(String[] args) {
            //클래스를 상속받은 경우 
            //대부분의 경우는 변수를 만들 때는 상위 클래스 이름을 사용
            Thread th1 = new ThreadEx();
            th1.start();
            
            //annonymous 클래스 사용
            Runnable r = new Runnable() {
                @Override
                public void run() {
                    //1초마다 i 값을 10번 출력
                    for(int i=0; i<10; i++) {
                        try {
                            Thread.sleep(1000);
                            System.out.println(i);
                        }catch(Exception e) {}
                    }
                }
            };
            Thread th2 = new Thread(r);
            th2.start();

        }
    }
```

### 6. Thread 상태 - 수명주기
- **NEW**  
`생성 만` 된 상태
- **RUNNABLE**  
스레드가 `실행 중`인 상태
- **WAITING**  
다른 스레드가 notify 나 notifyAll을 불러주기를 `기다리는` 상태
- **TIMED_WAITING**  
`일정 시간 대기`하고 있는 상태
- **BLOCKD**  
CPU를 사용할 필요가 없는 `입출력 작업을 수행` 중인 상태
- **TERMINATED**  
스레드가 `종료`된 상태
- Thread는 `run 메서드의 내용이 종료되면 종료`됩니다.
- 다른 Thread를 `강제 종료`시킬 수 있습니다.
- 한 번 종료된 스레드는 `다시 실행 시 킬 수 없습니다`.

## 7. Daemon Thread
- `Daemon이 아닌 스레드가 동작 중이지 않으면 종료`되는 스레드
- 다른 스레드의 `보조적인 역할(중요하지 않은 작업)`을 수행하는 스레드
  - 스레드를 생성하고 시작하기 전에 setDaemon에 true 만 설정
  - 대표적인 Daemon Thread 가  main 메서드를 실행시켜 만들어지는 main thread 와 garbage collection

## 8. Thread 의 Priority(우선 순위)
- 모든 스레드의 `우선 순위는 동일`  
  - 어떤 스레드를 먼저 수행하고 어떤 스레드가 자주 실행되는지를 알 수 없음
- 우선 순위를 설정해도 정확하게 동작하지는 않으나,  
우선 순위가 높은 스레드가 먼저 또는 자주 실행될 가능 증가
- 우선 순위 설정은 **setPriority**라는 메서드  
메서드의 매개변수는 정수이고 숫자가 높으면 고 우선순위
- 우선 순위 설정을 위해서 JDK 에서는 Thread 클래스에 `static final 변수(Field Summary)`를 제공
  - MIN_PRIORITY  
  NORM_PRIORITY  
  MAX_PRIORITY

## 9. Thread 의 강제 종료
- run 메서드에서 **InterruptedException** 발생시 return 하도록 생성 후, 
`강제로 종료시키고자 하는 스레드`에서 **interrupt**()라는 메서드 호출
  - interrupt()는 InterruptedException을 발생시키는 메서드

## 10. Thread 의 실행 제어 메서드
- join  
`일정 시간 동안 작업을 수행`하도록 해주는 메서드  
  - Time Sharing(시분할) System
- suspend  
`스레드를 일시 정지`시키는 메서드
- resume  
일시 정지된 `스레드를 다시 시작`시키는 메서드
- yield  
다른 스레드에게 `실행 상태를 양보`하고 자신은 `준비 상태로 변경`되는 스레드  
  - 다른 스레드의 작업이 끝날때 가지 대기
	
## 11. MultiThread
### 2개 이상의 스레드가 동시에 실행 중인 상황
### 장점
- 작업이 수행 도중 대기 시간이 있는 경우 대기 시간을 효율적으로 사용 가능
- 긴 작업 과 짧은 작업이 있는 경우 짧은 작업이 긴 작업을 무한정 기다리는 것도 방지

### 단점
- 메모리 사용량이 증가
- 너무 많은 스레드가 동시에 실행되면 처리 속도가 느려질 수 있음
- 공유 자원 사용의 문제점
    **critical section**
      - `임계 영역`(공유 자원을 사용하는 코드 영역) 에서의 `데이터 수정` 문제
    **생산자와 소비자 문제**  
      - 생산자가 `생산을 하지 않은 상태에서 소비자가 공유 자원에 접근`하는 문제
    **Dead Lock**(교착 상태) 문제 
      - `결코 발생할 수 없는 사건을 무한정 기다리는 것`

### 공유 자원의 수정 문제
- 하나의 스레드가 `사용 중인 공유 자원을 다른 스레드가 수정`하면서 발생할 수 있는 문제
- 공유 자원의 수정 문제 발생

- 실행해보면 1-2000까지의 합이 나오지 않고 실행할 때 마다 결과 변경
- 이 문제를 해결하는 방법은 2가지
  - 스레드로 작업할 때 데이터를 복제하지 말고 `원본에 작업`
    - 이 때 사용되는 예약어 **volatile**
  - 작업을 순차적으로 실행
    ```java
    synchronized(인스턴스){ }
    // { } 내에서는 인스턴스를 사용하는 부분이 동시에 실행될 수 없음
    //메서드에 synchornized를 추가
    //메서드 자체 synchronize 와 영역에 대한 부분인데
    //영역에 대한 synchronize를 권장
    ```
### 생산자 와 소비자 스레드 문제(producer-consumer problem)
- 생산자 스레드가 만든 자원을 소비자 스레드가 `동시에 사용하는 것`은 가능
  생산자 스레드가 `자원을 생성하지 못했는데 소비자 스레드가 자원을 사용`하려고 하면 자원이 만들어지지 않아 예외 발생
- 이런 경우에는 `사용할 자원이 없으면 소비자 스레드를 waiting`  
생산자 스레드는 `자원을 생성하면 소비자 스레드에게 signal(notify)`을 주어야 합니다.
  - java에서는 Object 클래스에서 waiting을 위한 **wait** 메서드 와 **notify** 와 **notifyAll** 메서드를 제공  
  이 메서드들은 synchronized 메서드 안에서만 동작

### 생산자 와 소비자 문제 발생

- 실행을 하면 소비자 스레드가 예외가 발생해 중지
공유 자원에 데이터가 없는데 꺼낼려고 해서 발생하는 문제

- `데이터를 꺼내서 사용하는 메서드`에서 데이터가 없으면 `기다리라고 하고`  
`데이터를 생성하는 메서드`에서는 `데이터를 생성하면 notify를 호출`해서 wait 중인 스레드를 수행

### Dead Lock
- 결코 발생할 수 없는 사건을 무한정 기다리는 것
- `동기화된 메서드 안에서 다른 동기화된 메서드를 호출`하는 것  
동기화 된 코드 안에 동기화 된 코드가 존재하는 경우에 발생하는 경우가 많음

### Semaphore
### 동시에 수행할 수 있는 스레드의 개수를 설정할 수 있는 클래스
- 공유 자원이 여러 개인 경우 각각의 Lock을 설정  
이런 경우 `모두 사용 중이라는 것을 알려면 모든 스레드를 전부 조사`
  - 사용할 수 있는 `Lock을 관리하는 인스턴스`를 하나 만들어서  
  `사용하기 전에 사용할 수 있는 Lock의 개수를 확인`해서 수행
- 공유 자원을 사용할 때 **acquire**()를 호출하고 공유 자원이 사용이 끝나면 **release** 호출
