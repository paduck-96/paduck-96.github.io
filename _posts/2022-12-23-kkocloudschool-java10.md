---
title: "Java File 과 Buffer,Stream 그리고 Network"
excerpt: "Java File 클래스에 대해 학습하고,
이를 활용해 입출력 버퍼와 스트림, serializable에 대해 학습하고
Network 이론에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, java, ]

toc: true
toc_sticky: true

date: 2022-12-23
last_modified_at: 2022-12-23
---

# IO(입출력)

## path(경로)

### 디렉토리 구분 기호

- Windows 로컬  
  \ -> \\\
- Mac, Linux, Web 등  
  /
- java.io.File.seperator  
  운영체제 별로 구분 기호를 설정

### 절대 경로 와 상대 경로

- 절대 경로

  - Windows  
    드라이브:\경로
  - Mac, Linux, Unix  
    /경로
  - Network  
    프로토콜://경로

- 상대 경로  
  `현재 위치`로부터의 경로
  - ./  
    현재 디렉토리인데 생략 가능한 경우가 있음
  - ../  
    상위 디렉토리
  - /  
    루트 디렉토리

## 2. java.io.File

### `파일에 대한 정보`를 가진 클래스

- `파일을 생성하고 삭제하고 정보를 확인`할 수 있게 하는 클래스
- 제일 중요한 작업들은 파일의 `존재 여부` 파악, `마지막 수정 날짜` 파악

  클라이언트-서버 프로그래밍에서  
  클라이언트가 서버의 데이터를 받아서 저장한 후 실행하는 애플리케이션

  - 무조건 `서버에서 데이터를 받아 저장`해서 사용하는 경우  
    서버의 데이터가 아주 자주 갱신되는 경우
  - 그 이외에는 서버에서 데이터를 받아서 저장한 후  
    서버의 데이터와 비교해 `다른 부분이 있으면 그 부분만 갱신`

## 3. java.nio.file.Path

### 파일 경로에 대한 클래스

- nio 패키지는 `io 패키지의 최신` 버전

### 생성

- Paths.get(파일 경로)
- Paths.get(URI uri)  
  URI 인스턴스는 URI.create("file:///경로")

### File 객체로 변환

- **toFile** 이라는 메서드를 호출하면 `File 객체로 변환해서 사용` 가능

## 4. Stream

### `데이터를 운반하는데 사용되는 통로`

### 분류

- **입력** 스트림(읽어오는 것)
- **출력** 스트림(내보내는 것)
- **바이트** 스트림(바이트 단위로 입출력)
- **문자** 스트림(문자 단위로 입출력)
  - 문자는 `양쪽의 인코딩 방식`이 같아야 함  
    양쪽의 인코딩 방식이 `다르면 바이트 스트림 사용`

### 특징

- FIFO 구조
- 단방향성

## 5. 바이트 스트림

- `바이트 단위로 데이터를 입출력`하기 위한 스트림
  - 기본 스트림

### InputStream

- `입력을 위한 스트림의 최상위 클래스`로 추상 클래스
- 읽기 작업을 위한 메서드 선언
  - int **available**( )  
    `읽을 수 있는 바이트 수` 리턴
  - void **close**( )  
    `스트림 닫기`
  - int **read**( )  
    `한 바이트 읽고, 읽은 바이트를 정수`로 리턴  
    `읽은 데이터가 없을 경우 -1 리턴`
  - int read(byte [] b)  
    `b만큼 읽어서 b에 저장`하고 읽은 바이트 수 리턴  
    읽은 데이터가 없을 경우 -1 리턴
  - int read(byte [] b, int offset, int length)  
    `offset 부터 length 만큼 읽어서` b에 저장하고  
    읽은 바이트 수 리턴  
    읽은 데이터가 없으면 -1 리턴
  - long **skip**(long n)  
    n 만큼 `건너뛰기`

### OutputStream

- `바이트 단위로 출력하기 위한 스트림의 최상위 클래스`로 추상 클래스
- 쓰기 작업을 위한 메서드
  - void **close**( )
  - void **write**(byte [] b)  
    `b를 버퍼에 기록`
  - void write(byte [] b, int offset, int length)  
    `b에서 offset부터 length까지 기록`
  - void write(int b)  
    `b를 문자로 변환`해서 기록
  - void **flush**()  
    `버퍼의 내용을 스트림`에 기록

### 파일에 바이트 단위로 읽고 쓰기 위한 스트림

- **FileInputStream**  
  파일에서 `읽어오기 위한` 스트림
  - FileInputStream(String 경로)
  - FileInputStream(File file)
- **FileOutputStream**  
  파일에 `기록하기 위한` 스트림
  - FileOutputStream(String 경로)  
    매번 파일에 새로 기록
  - FileOutputStream(String 경로, boolean appendMode)  
    `파일이 존재하는 경우 추가`할 수 있게 설정
  - FileOutputStream(File file)
  - FileOutputStream(File file, boolean appendMode)

### 버퍼를 이용하는 스트림

- `읽고 쓰기를 할 때 버퍼를 사용하는 이유`  
  파일 읽고 쓰기 작업은 OS(운영체제)의 `Native Method 호출` 후 수행

  - 버퍼 사용하지 않으면 `요청마다 해당 메서드를 지속해서 호출`  
    반복 작업 시 효율이 떨어짐

  버퍼에 모아서 `버퍼가 가득차거나`, `강제로 버퍼의 내용을 비우는` 형태  
   Native Method 의 호출 횟수를 줄일 수 있음

  자바는 바이트 단위 입력 시  
  버퍼링을 위한 **BufferedInputStream** 클래스 제공  
  바이트 단위 출력을 할 때는 버퍼링을 위한 **PrintStream** 클래스 제공

  - 생성자는 `다른 스트림의 인스턴스`를 받아 인스턴스 생성하게 형성
  - **PrintStream** 에는 write 외 print 와 같은 출력 메서드 제공
  - `문자열의 경우 byte 변환 없이 기록 가능`

## 6. 문자 스트림

### 문자 단위로 읽고 쓰기 위한 스트림

### Reader

- `문자 단위로 읽는 스트림` 중 최상위 클래스로 `추상 클래스`
- 메서드
  - int **read**( )  
    `하나의 문자를 읽어서 정수`로 리턴
  - int read(char [] buf)  
    `buf만큼 읽어서 저장 후 읽은 개수` 리턴  
    읽어온 게 없으면 -1 리턴
  - int read(char [] buf, int offset, int length)  
    `buf에 저장`, `offset부터 length 만큼 읽어오고 개수` 리턴  
    읽어온 게 없으면 -1 리턴
  - void **close**( )

### Writer

- `문자 단위로 기록을 하는 스트림` 중 최상위 클래스로 추상 클래스
- 메서드
  - void **close**( )
  - void **write**(String str)
  - void write(String str, int offset, int length)
  - void **flush**( )

### InputStreamReader 와 OutputStreamWriter

- `바이트 스트림을 받아서 문자 스트림으로 변환`
- **네트워크 프로그래밍**에서는 `바이트 스트림만` 제공  
  **채팅** 같은 서비스 나 **웹의 문자열**을 가져오는 서비스는  
  `읽고 쓰는 단위가 문자열`이므로  
  이 클래스들을 이용해서 `문자 스트림으로 변환`해서 읽는게 편리

### FileReader 와 FileWriter

- `파일에 문자 단위로 입출력`하기 위한 스트림

### BufferedReader 와 PrintWriter

- `버퍼를 이용해서 문자 단위로 입출력`하기 위한 스트림
- BufferedReader 에는 줄 단위로 읽는 **readLine** 메서드 제공

### properties file

- `속성과 값을 쌍으로 저장`하는 텍스트 파일
- 용도

  - `처음 한 번 설정시 변경되지 않고 사용되는 문자열` 저장
    (환경 설정)
  - `국가나 위치에 따라 보여지는 문자열이 달라져야 하는 경우`  
    메시지를 작성하고 국가나 지역 설정에 따라 다르게 보여지게함  
    (지역화)
    - 확장자 앞에 국가 코드 나 언어 코드를 사용해 구분

- 읽는 방법  
  `File 인스턴스`로 생성, Properties 인스턴스의 **load**( )와 **getProperty**( ) 이용
  - 대부분의 프레임워크는 여기까지 자동으로 수행

## 7. Serializable

### `인스턴스를 스트림에 전송`하기 위한 것

- `스트림을 통해 직접 작성한 클래스의 인스턴스를 전송`하기 위한 기술
  - `직접 작성한 클래스의 인스턴스`는 기본적으로  
    네트워크를 통해 `인스턴스 단위로 전송 불가`
- 이것 때문에 `응용 프로그램끼리 데이터 호환이 어려움`

### Serializable 이 가능한 클래스 생성

- `Serializable 인터페이스를 implements` 하면  
  **모든 속성**이 Serializable 대상
- 속성 중에서 `제외하고자 하는 경우` 앞에 trasient 추가

### 인스턴스 단위로 읽고 쓰기위한 스트림

- ObjectOutputStream, ObjectInputStream을 이용해  
  writeObject이 쓰고, readObject이 읽는 것

### 실습

- Serializable 인터페이스를 implements

## 8. log.txt 파일에서 가장 마지막 항목 구하기

### log.txt 파일 읽기

# Network

## 1. 용어

### Protocol

`통신을 하기 위한 규칙`

- **TCP/IP**  
  인터넷 프로토콜
- **HTTP**  
  Hyper Text Transfer Protocol
- **HTTPS**  
  Hyper Text Transfer Protocol over Secure SOcket Layer
- **FTP**  
  파일 전송 프로토콜
- **TELNET**  
  원격 접속

- **IP**  
  인터넷 프로토콜

  - **IPv4**  
    `32비트 주소 체계`로 0.0.0.0 ~ 255.255.255.255
  - **사설 IP** 대역  
    10.0.0.0 ~ 10.255.255.255  
    172.16.0.0 ~ 172.31.255.255  
    192.168.0.0 ~ 192.168.255.255
  - **IPv6**  
    `128비트 주소 체계`
  - **Loopback**  
    `자기 자신의 컴퓨터의 IP`로  
    IPv4-127.0.0.1, IPv6-0::0::0::0::0::0::0::1

- **PORT**  
  `서비스를 구별하기 위한 번호`

  - 하나의 컴퓨터 안에서 `Process를 구별하기 위한 번호`
  - 0 ~ 65535 까지 사용 가능
  - `시스템 예약`(0 ~ 1024까지)
    - http(80)
    - https(443)
    - ftp(20, 21)
    - SSH(22)
    - Telnet(23)...
  - `프로그램의 기본 포트`
    - Oracle(1521, 8080)
    - Tomcat(8080)
    - MYSQL, MariaDB(3306)
    - MSSQL(1443),
    - MongoDB(27017) ...

- **DOMAIN**  
  `IP와 PORT 에 매핑시킨 문자열 주소`

- **URI**(Uniform Resource Identifier)  
  `인터넷에 있는 자원을 나타내는 유일한 주소`

  - URL  
    네트워크 상의 자원 식별자
  - URN  
    영속적인 식별자

- **Routing**  
  `최적의 경로를 찾는 것`을 의미  
  웹 프로그래밍에서는 `URL과 요청 방식에 따라 분리`해서 처리

- **Proxy**  
  `클라이언트가 외부 네트워크에 요청`을 할 때  
  `네트워크 내부에서 처리`할 수 있도록 해주는 시스템/프로그램
  - React 프로젝트에서 Node, Spring 서버에 요청을 해서  
    데이터를 가져올 때 proxy를 package.json에 설정
    - 서버 애플리케이션에서 CORS 설정
- **Firewall**  
  `외부에서 내부 네트워크로 접속할 때 제한` 하는 프로그램/하드웨어

  - 자신의 컴퓨터가 `서버일 경우 해제`

- **RPC**(Remote Procedure Call - 원격 프로시저 호출)  
  `다른 컴퓨터의 프로시저`를 호출하는 것

- **Open API**  
  누구나 사용할 수 있도록 공개된 API(라이브러리 나 데이터)

- **REST**  
  `소프트웨어 아키텍쳐`의 한 형식  
  `일관성 있는 URL` 사용

- ping 명령  
  echo
