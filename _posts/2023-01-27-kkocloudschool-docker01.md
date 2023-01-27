---
title: "Docker 학습"
excerpt: "Docker에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, docker]

toc: true
toc_sticky: true

date: 2023-01-26
last_modified_at: 2023-01-26

---

## 1. Docker

### Docker

- Container 형 `가상화 기술을 구현`하기 위한 애플리케이션,  
  이 애플리케이션 조작하기 위한 `명령행 도구로 구성`되는 애플리케이션
- 프로그램 과 데이터를 격리시키는 기능을 제공
- 도커는 리눅스 운영체제를 사용  
  윈도우나 Mac OS에서도 도커를 구동시킬 수 있지만 이 경우 `내부적으로 리눅스가 사용`되면 `컨테이너 동작시킬 프로그램도 리눅스 용 프로그램`

> H/W <-> Linux <-> Docker Engine <-> 컨테이너

### 데이터 나 프로그램을 독립된 환경에 격리시켜야 하는 이유

- 마이크로 서비스를 구현하는 이유와도 유사
- 대부분의 프로그램은 단독으로 동작하지 않고 `실행 환경이나 라이브러리 또는 다른 프로그램을 이용`해서 동작
  - PHP 나 Java는 실행 환경이 별도로 필요하고  
    Python은 다른 라이브러리를 사용하는 경우가 많고  
    워드프레스나 레드마인 같은 경우는 MySQL 이나 Maria DB 와 같은 프로그램이 필요
- 다른 프로그램 과 폴더 나 디렉토리 또는 환경 설정 파일을 공유하는 경우,  
  프로그램 하나를 수정하는 경우 다른 프로그램에도 영향을 미치는 경우가 발생할 수 있음

### 도커 와 가상화 기술 차이

- Virtual Box 나 VMWare, UTM(Mac M1 이나 M2 Processor) 같은 가상화 기술  
  `가상의 물리 서버를 만드는 것`으로,  
  **가상화**는 `물리적인 대상을 소프트웨어로 구현했`다는 의미로  
   운영체제를 설치하고 그 위에 소프트웨어를 구동시키는 것

  - 도커는 리눅스 운영체제의 일부 기능만을 물리 서버에 맡겨서 수행하는 형태로 사용할 수 있는 소프트웨어는 리눅스 용으로 제한됨

- AWS EC2는 도커 와 비슷하게 인스턴스라는 개념을 사용하는데,  
  EC2도 가상화 기술로 각각의 인스턴스가 완전히 독립된 컴퓨터처럼 동작하는 방식

  - AWS EC2는 AMI 라는 이미지로부터 생성되기 때문에 인스턴스를 배포하는 방법이 Docker 와 유사  
    가상 서버를 만들지 않아도 컨테이너 이미지를 바로 실행할 수 있는 호스팅 서비스의 형태

- 가상화 기술

  > 하드웨어 <-> 리눅스 커널 <-> 리눅스 셸 <-> 소프트웨어 <-> 사용자

- 리눅스의 도커

  > 하드웨어 <-> 리눅스 커널 <-> 도커 엔진 <-> 이미지(셸 과 소프트웨어) 또는 컨테이너 <-> 사용자

- 윈도우의 도커
  > 하드웨어 <-> 윈도우 <-> Hyper-V, Moby-VM, 리눅스 운영체제(커널) <-> 도커 엔진

### Image 와 Container

- **Image** - Class
- **Container** - Instance
- Container를 수정해서 Image를 생성할 수 있음
- Container는 다른 도커 엔진으로 이동이 가능

  - 이미지를 이용해서 이동한 것 과 같은 효과

- 컨테이너를 이용해서 이미지를 만든 후 export 를 하고,  
  다른 도커 엔진에서 import 하면 사용 가능

- 도커 허브  
  https://hub.docker.com

  - 도커 이미지를 모아 놓은 곳

- 이미지의 종류

  - 운영체제
  - 소프트웨어가 포함된 이미지
  - 소프트웨어가 여러 개 포함된 이미지

### 데이터를 저장

- 컨테이너를 삭제하면 컨테이너 안에서 만들어지 데이터도 소멸됨
- 도커가 설치된 `물리적 서버의 디스크를 마운트`해서 저장하는 것이 가능

### 도커의 장점

- 개발 환경 과 운영 환경을 거의 동등하게 재현할 수 있음
- 가상화 소프트웨어보다 가벼움
- 물리적 환경의 차이나 서버 구성의 차이를 무시할 수 있음
- 클라우드 플랫폼 지원

### 도커를 추천하지 않는 경우

- 리눅스 계열의 운영체제 동작이 요구되는 경우  
  실제 리눅스 와 도커의 리눅스는 다름

- 도커는 리눅스 머신에서만 동작

### 주 용도

- 동일한 개발 환경 제공
- 새로운 버전을 테스트
- 동일한 구조의 서버가 여러 개 필요한 경우

## 2. 도커 사용

### 사용 방법

- 리눅스 컴퓨터에서 도커 사용
- 윈도우 나 Mac OS 에서 Docker Desktop을 설치해서 사용
- 가상 머신이나 랜탈 환경에 도커를 설치하고 사용

### 제약

- 운영체제가 64bit

### 윈도우에서 사용할 때 수행할 내용

- Hyper-V 기능이 필요  
  확인은 작업 관리자에서 CPU 부분을 확인  
  가상화가 사용으로 되어 있어야 함
  - 사용 안함으로 되어 있다면 BIOS에서 설정.
- WSL2를 다운로드 받아서 설치  
  https://learn.microsoft.com/en-us/windows/wsl/install

### Docker Desktop 설치

- 도커 허브에 회원 가입 후 설치

## 3.Docker의 기본적인 사용

### 명령어의 기본 형식

- 도커의 명령어는 docker로 시작
- 기본 형식  
  docker 명령어 옵션 대상 인자

- **명령어**는 단순 명령어 와 상위 명령어 와 하위 명령어로 구분
- **옵션**은 도커에 대한 옵션
- **인자**는 이미지에 대한 옵션

### 예)

```docker
# penguin 이미지를 다운로드
docker image pull penguin
# penguin 이미지를 실행
docker container run penguin
# penguin 이미지를 background mode에서 사용
docker container run penguin -mode=1
```

### 도커의 버전 확인

docker version

### 상위 커맨드

- container
- image
- network  
  여러 개의 컨테이너를 하나로 묶고자 할 때 사용
- volume

### 하위 커맨드

- **container**

  - start  
    컨테이너 실행
  - stop  
    컨테이너 중지
  - create  
    컨테이너 생성
  - run  
    이미지가 없으면 내려받고 컨테이너를 생성해서 실행  
    image pull, create, start 가 합쳐진 명령
  - rm  
    정지 중인 컨테이너 삭제
  - exec  
    컨테이너 안에서 프로그램을 실행
  - ls  
    컨테이너 목록 출력
  - cp  
    컨테이너 와 호스트 간에 파일을 복사
  - commit  
    컨테이너를 이미지로 변환

- **image**

  - pull  
    이미지 다운로드
  - rm  
    이미지 삭제
  - ls  
    이미지 목록 출력
  - build  
    이미지를 생성

- **volume**

  - create  
    생성
  - inspect  
    상세 정보 출력
  - ls  
    목록 출력
  - prune  
    마운트 되지 않은 볼륨을 모두 삭제
  - rm  
    지정한 볼륨 삭제

- **network**

  - connect  
    컨테이너를 네트워크에 연결
  - disconnect  
    컨테이너를 네트워크 연결에서 해제
  - create  
    네트워크 생성
  - inspect  
    상세 정보 출력
  - ls  
    목록 출력
  - prune  
    현재 컨테이너가 접속하지 않은 경우 삭제
  - rm  
    네트워크 삭제

### 기타 상위 커맨드

- checkpoint
- node
- plugin
- secret
- service
- stack
- swarm
- system

### 단독으로 사용되는 명령

- login
- logout
- search
- version

### **docker run** 명령

- docker image pull, docker container create, docker container start 명령을 합친 것 과 같은 명령
- 기본 형식
  docker run [옵션] 이미지 [인자]
- 옵션

  - --name  
    컨테이너 이름
  - -p  
    포트 포워딩
  - -v  
    볼륨 마운트
  - --net  
    네트워크 연결
  - -d  
    데몬으로 생성
  - -i  
    컨테이너에 터미널을 연결하
  - -t  
    특수 키를 사용할 수 있도록 설정  
    d, i, t 는 같이 사용하는 경우가 많아서 -dit로 설정하는 경우가 많음

### 컨테이너 확인

- docker ps  
  실행 중인 컨테이너 확인
- docker ps -a  
  모든 컨테이너 확인
- 출력되는 정보

  - CONTAINER ID  
    컨테이너의 식별자로 64글자 인데 12글자만 출력
  - IMAGE  
    컨테이너 생성할 때 사용한 이미지 이름
  - COMMAND  
    컨테이너 실행 시에 실행하도록 설정된 프로그램 이름
  - CREATED  
    컨테이너 생성 후 경과한 시간
  - STATUS  
    컨테이너의 현재 상태로,  
    실행 중 이면 up 실행중이 아니면 exited
  - PORTS  
    컨테이너에 할당된 포트 번호로,  
    호스트포트번호 -> 컨테이너포트번호 형식으로 출력되는데,  
    동일한 포트 번호를 사용하면 -> 뒤는 나오지 않음
  - NAMES  
    컨테이너 이름

### 컨테이너 중지

- docker stop 컨테이너ID 또는 이름
- docker stop $(docker ps -a -q)  
  모든 컨테이너 중지

### 컨테이너 삭제

- docker rm 컨테이너ID 또는 이름  
  컨테이너가 실행 중이면 삭제되지 않음
- docker rm $(docker ps -a -q)  
  모든 컨테이너 삭제

### Apache 컨테이너를 이용한 실습

- Apache 와 Nginx  
  웹 서버를 만들어주는 소프트웨어
- 이미지 이름은 httpd 이고 컨테이너 이름은 apa000ex1
  ```docker
    이미지를 다운로드 받고 컨테이너를 생성하고 실행 ->
    컨테이너 상태 확인 ->
    컨테이너 종료 ->
    컨테이너 상태 확인 ->
    컨테이너 삭제 ->
    컨테이너 상태 확인
  ```
- `동일한 하위 명령어가 없다면 상위 명령어는 생략 가능`

```docker
# 생성하고 실행
docker container run --name apa000ex1 -d httpd
# 실행 확인
docker ps
# 컨테이너 중지
docker container stop apa000ex1
# 실행 확인
docker ps
# 삭제
docker container rm apa000ex1(rm 의 경우도 container를 생략 가능)
# 확인
docker ps -a
```

### 포트포워딩

- 외부에서 컨테이너에 접근하려면 `외부 와 접속하기 위해 포트포워딩` 필요
- apache는 80번 포트를 이용해 외부와의 접속을 수행  
  이를 외부에서 접속할 수 있도록 `외부 포트와 연결`
- 포트 포워딩 방법
  ```docker
  -p 호스트포트번호:컨테이너포트번호
  ```
  컨테이너 포트 번호는 중복될 수 있지만 **호스트 포트 번호**는 중복 불가

### Apache 웹 서버를 컨테이너 생성 후 외부 접속 확인

### 다양한 컨테이너

- 리눅스 운영체제게 담긴 컨테이너
  - ubuntu  
    -d 없이 it 옵션만으로 사용
  - debian  
    -d 없이 it 옵션만으로 사용
  - centos  
    -d 없이 it 옵션만으로 사용
  - fedora  
    -d 없이 it 옵션만으로 사용

### 웹 서버 및 데이터베이스 서버용 컨테이너

- httpd  
  apache 웹 서버로, -d로 백그라운드 실행을 하고 -p로 포트 설정
- nginx  
  웹 서버로, -d로 백그랑ㄴ두 실행을 하고 -p로 포트 설정
- mysql  
  -d를 사용하고 -e MYSQL_ROOT_PASSWORD 로 루트 비밀번호 지정
- mariadb  
  -d를 사용하고 -e MYSQL_ROOT_PASSWORD 로 루트 비밀번호 지정
- postgres  
  -d를 사용하고 -e POSTGRES_ROOT_PASSWORD 로 루트 비밀번호 지정
- oracle  
  다양한 이미지 이름

### 프로그래밍 언어

- openjdk
- python
- php
- ruby
- perl
- gcc
- node
- registry  
  도커 레지스트리
- wordpress  
  블로그와 같은 웹 사이트 개발 툴
- redmine  
  todo 와 유사한 형태의 일정 관리 툴
- nextcloud

### 이미지 관련 명령어

- 이미지 삭제  
  docker image rm 이미지이름 나열
  - 컨테이너가 `사용 중인 이미지는 삭제가 불가`
- 이미지 정보 확인  
  docker image ls

### 우분투 컨테이너 생성

- 우분투 이미지 조회  
  docker search --limit 10 ubuntu
- 컨테이너 생성  
  docker run --name ubuntu -it ubuntu
  - 바로 shell 에 접속

### 오라클 컨테이너 사용

- 8080 포트와 1521(외부 접속) 포트를 사용

### oracle 11g 컨테이너 생성

- docker run --name oracle11g -d -p 8080:8080 -p 1521:1521 jaspeen/oracle-xe-11g
- 외부 접속을 위한 추가 설정 불필요  
  서비스 이름은 xe, 관리자 계정의 비밀번호는 oracle

## 4. 여러 개의 컨테이너를 하나의 네트워크로 묶어 연동

### 워드프레스 와 MySQL 연동

- 워드프레스  
  웹 사이트를 만들기 위한 소프트웨어로 서버에 설치해 사용

  - 아파치, php 런타임, DB 필요  
    MariaDB 와 MySQL 지원
  - 워드프레스 본체 와 아파치, php 런타임 내장

  구동을 위해 외부 접속 DB가 있거나 DB 컨테이너가 필요

- MySQL
  - 컨테이너 생성  
    docker run --name 컨테이너이름 -dit --net=접속네트워크이름 -e MYSQL_ROOT_PASSWORD=관리자비밀번호 -e MYSQL_DATABASE=DB이름 -e MYSQL_USER=사용자이름 -e MYSQL_PASSWORD=사용자비밀번호 -p 호스트포트번호:3306 이미지이름 --character-set-server=문자인코딩 --collation-server=정렬순서 --default-authentication-plugin=인증방식
  - e 옵션은 환경 변수  
    관리자 비밀번호는 필수로,  
    관리자 계정으로 모든 작업 수행할 수 있지만  
    이를 DB 작업에 쓰는 건 `보안 문제 발생 가능성`이 있어 사용자 권장
  - MySQL / MariaDB에서 authentication-plugin이 다른 의미,  
    보안을 위해 비밀번호를 해싱해 보관하는 형태로 변경
    - 이전 방식으로 하고 싶을 경우 --default-authentication-plugin에 mysql_native_password 설정
    - 데이터베이스에서 관리자로 접속해 변경하는 것도 가능

### 도커 네트워크

- `가상의 네트워크`로,  
  `네트워크에 속한 컨테이너끼리 연결`해 서로 접속 가능
- 생성  
  docker network create 이름
- 삭제  
  docker network rm 이름
- 목록  
  docker network ls 이름

### 워드프레스 컨테이너 생성

- docker run --name 컨테이너이름 -dit --net=접속할네트워크이름 -p 포트번호 -e WORDPRESS_DB_HOST=DB컨테이너이름 -e WORDPRESS_DB_NAME=DB이름 -e WORDPRESS_DB_USER=DB사용자계정 -e WORDPRESS_DB_PASSWORD=DB사용자 비밀번호

- 명령어 수행(네트워크 생성)  
  docker network create wordpress000net1
- DB컨테이너 생성  
  --net=wordpress000net1 ... --character-set-server=utfmb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
- 워드프레스 컨테이너 생성  
  --net=wordpress000net1
- `종료는 생성의 역순으로 진행`  
  워드프레스 > mysql > 네트워크 순
- 확인  
  docker ps -a  
  docker network ls

### MySQL 컨테이너에서 유저 생성 및 접속 권한 부여

- 외부에서 사용할 수 있도록 컨테이너 생성  
  docker run --name mysql -e MYSQL_ROOT_PASSWORD=비밀번호 -d -p 접속할포트번호:3306 mysql
- 컨테이너 내부 접속  
  docker exec -it mysql bash
- 관리자 접속  
  mysql -u root -p
- db 작업

```docker
 create database 이름;
 create user '유저아이디'@'접속할 IP' identified by "비밀번호"

 - 모든 곳에서 접속시 ip 대신에 % 사용

 grant all privileges on DB이름 to '유저아이디'@'접속할IP';

 - 모든 DB 접속 가능시 이름 대신 *.* 설정

 alter user '이름'@'ip' identified with mysql_native_password by '이름';
   - MySQL 8.0은 인증 방식을 예전으로 해야 외부에서 접속 가능

 flush privilegs;
```

### 레드 마인 및 Maria DB 컨테이너 결합

- **레드마인**

  - 웹 기반의 `프로젝트 관리와 버그 추적 기능`을 제공
  - 프로젝트 관리에 도움이 되는 달력과  
    Gantt Chart를 제공하고 일정 관리 기능을 제공

  ```docker
    #MariaDB 컨테이너 생성
    이미지 이름이 mariaDB이고 생성 방법은 동일
    #네트워크 생성
    docker network create redmine000net3
    #MariaDB 컨테이너 생성
    docker run --name mariadb000ex15 -dit --net redmine000net3 -e MYSQL_ROOT_PASSWORD=비밀번호 -e MYSQL_DATABASE=redmine000db -e MYSQL_USER=유저명 -e MYSQL_PASSWORD=유저비밀번호 mariadb --charac ter-set-server=utf8mb4 --collationserver=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
    #레드마인 컨테이너 생성
    docker run -dit --name redmine000ex16 --net redmine000net3 -p 8087:3000 -e REDMINE_DB_MYSQL=DB컨테이너명 -e REDMINE_DB_DATABASE=DB자체이름 -e REDMINE_DB_USERNAM=유저명 -e REDMINE_DB_PASSWORD=유저비밀번호 redmine
    #확인
    localhost:레드마인포트번호
    #뒷정리
    docker stop redmine000ex16
    docker stop mariadb000ex15

    docker container rm redmine000ex16
    docker container rm redmine000ex15
  ```

## 5. 컨테이너 와 호스트 간에 파일 복사

- 프로그램 만으로 구성된 시스템은 그리 많지 않음
- 실제 시스템은 프로그램, 프로그래밍 언어의 런타임,  
  웹 서버, DB 서버 등이 `함께 시스템을 구성`
  - 이외에도 `파일 데이터가 포함`되기도 함
- apache의 경우 내부적으로 index.html을 소유,  
  웹에서 호출을 하면 index.html을 출력
- DB들도 백업시 자신의 특정한 디렉토리에 백업 파일 보관  
  이런 파일은 호스트에게 전송할 수 있고,  
  호스트에게서 받아서 사용할 수도 있음

### 명령

- 호스트에서 컨테이너로 복사  
  docker cp 호스트경로 컨테이너이름:컨테이너경로
- 컨테이너에서 호스트로 복사  
  docker cp 컨테이너이름:컨테이너경로 호스트경로

### apache의 index.html 위치

- /usr/local/apach2/htdocs

### 호스트 컴퓨터의 index.html 파일을 apache 컨테이너껄로 사용

```docker
#apache 컨테이너 생성
docker run --name apa000ex19 -d -p 8089:80 httpd
#웹브라우저에서 확인시 정상 출력
#index.html 로컬에 생성
#호스트 파일 컨테이너에 복사
docker cp 파일경로 apa000ex19:/usr/local/apache2/htdocs
#브라우저에서 새로고침
```

### 컨테이너의 파일을 호스트로 복사

- docker cp apa000ex19:/usr/local/apache2/htdocs/index.html 호스트 디렉토리 경로

## 6. 스토리지 마운트

- 볼륨 마운트

### 용어

- **Volume**  
  스토리지의 한 영역을 분할 한 것
- **Mount**  
  연결을 해서 운영체제 나 소프트웨어가 관리할 수 있게 하는 것

### 스토리지 마운트 이유

- 컨테이너 안에 만들어진 데이터는 컨테이너 소멸시 같이 삭제,  
  데이터를 남겨 다른 컨테이너나 호스트에서 사용을 위해
- `Data Persistency`  
  데이터를 `다른 외부 장치와 연결해 컨테이너 독립성` 유지  
  이를 위해 스토리지 마운트 수행

### 마운트 방식

- 볼륨 마운트  
  도커 엔진이 관리하는 영역 내에 만들어진 볼륨을,  
  `컨테이너에 디스크 형태`로 제공
- 바인드 마운트  
  도커 엔진이 설치된 `컴퓨터의 디렉토리 와 연결`하는 방식

```javascript
                볼륨 마운트    |바인드 마운트
스토리지 영역       볼륨        |  디렉토리
물리적 위치   도커 엔진 관리 영역|어디든지 가능
마운트 절차   볼륨 생성 후 마운트|기존 파일이나 디렉토리
내용 편집     도커 컨테이너 이용 |연관된 프로그램
백업          복잡             |일반 파일과 동일 방식
```

### 마운트 명령어

- docker container run 을 할 때 옵션 형태로 제공
- 스토리지 경로가 컨테이너 속 특정 경로와 연결되게 설정
- 컨테이너의 경로는 도커 이미지의 문서를 참조해 확인
  - apache 의 경우 /usr/local/apache2/htdocs  
    mysql 의 경우 /var/lib/mysql

### 바인드 마운트 실습

```docker
#바인드 할 디렉토리 결정
# apache 컨테이너 생성
docker run --name apa000ex20 -d -p 8090:80 -v 로컬경로:연결할 경로 httpd
# 웹 브라우저에서 apache가 아닌 index of 가 출력
# 바인드 된 디렉토리에 파일을 복사하고 새로고침하면 확인
```

### 볼륨 마운트

- 기본 명령
  - 볼륨 생성  
    docker volumne create 볼륨이름
  - 볼륨 상세 정보 확인  
    docker volumne inspect 볼륨이름
  - 볼륨 삭제  
    docker volume rm 볼륨이름

### 실습

```docker
# 볼륨 생성
docker volume create apa000vol1
# 마운트
docker run --name apa000ex21 -d -p 8091:80 -v apa000vol1:/usr/local/apache2/htdocs httpd
# 상세 정보 확인
docker volume inspect apa000vol1
docker container inspect apa000ex21
```

## 7. 컨테이너로 이미지 만들기

- `다른 개발자와 동일한 개발 환경`을 만들기 위해 이용

### 이미지 만드는 방법

- commit 커맨드로 컨테이너 이미지 변환
- Dockerfile 스크립트로 이미지 생성

### commit 명령으로 이미지 만들기

- docker commit 컨테이너이름 이미지이름

### Dockerfile 스크립트로 이미지 만들기

- 스크립트 파일에 명령어 기재 후, 빌드를 해서 만듦
- 빌드  
  docker build -t 이미지이름 스크립트파일의디렉토리경로
- 스크립트 명령어
  - FROM  
    토대가 되는 이미지 지정
  - ADD  
    이미지에 파일이나 폴더 추가
  - COPY  
    이미지에 파일이나 폴더 추가
  - RUN  
    이미지 빌드시 실행할 명령어 지정
  - CMD  
    컨테이너 실행시 실행할 명령어 지정

### commit 명령으로 apache 컨테이너 이미지 생성

- 현재 이미지 확인  
  docker image ls
- apache 컨테이너 생성  
  docker run --name apa000ex22 -d -p 8092:80 httpd
- index.html을 생성된 컨테이너에 복사  
  docker cp 파일경로 apa000ex22:/usr/local/apach2/htdocs/
- 컨테이너를 이미지로 변환  
  docker commit apa000ex22 ex22_copy
- 이미지 목록 확인  
  docker image ls
- 새로 만든 이미지 기반으로 컨테이너 생성  
  docker run --name apa000ex23 -d -p 8093:80 ex22_copy

### Dockerfile 스크립트로 이미지 만들기

- 컨테이너에 추가할 파일과 스크립트가 저장될 파일을 위한 디렉토리 결정
- 컨테이너와 함께 저장될 파일을 위의 디렉토리에 복사
- Dockerfile 파일을 작성  
  메모장에서 작성 후 확장자 제거
  ```docker
  FROM httpd
  COPY index.html /usr/local/apache2/htdocs
  ```
- Dockerfile 파일을 빌드해서 이미지 생성  
  docker build -t 이미지이름 Dockerfile 경로
- 이미지 생성 여부 확인  
  docker image ls

## 8. 컨테이너 개조

- 도커를 실제 운용하는 현엽에서는 사내 개발 시스템에 주로 사용  
  기본 이미지를 커스터마이징한 이미지를 많이 사용

### 컨테이너 개조

- 파일 복사 와 마운트를 이용하는 방법
- 컨테이너에서 리눅스 명령을 실행해 소프트웨어 설치/설정 변경

`리눅스 명령어를 실행하려면 셸에서 작업을 수행`

- 주로 bash 셸 많이 사용
- 컨테이너를 만들 때 옵션 없이 생성하면 셸 들어갈 수 없는 경우 발생
- bash 셸 사용할 수 있는 상태 생성시
  - docker exec 옵션 컨테이너이름 /bin/bash
  - docker exec 옵션 이미지이름 /bin/bash  
    컨테이너 실행되지 않고 bash 실행됨

### bash 명령

- 대부분의 경우 debian 계열의 리눅스에서 이미지 생성

## 9. 도커 허브

### 도커 허브

- 이미지를 업/다운로드 할 수 있는,  
  도커 제작사에서 운영하는 공식 도커 레지스트리
- 오픈 소스 재단의 많은 개발자들이 도커 허브에 참여

### 레제스트리 와 레포지토리

- 레지스트리  
  배포하는 장소
- 레포지토리  
  레지스트리를 구성하는 소프트웨어

### 이미지 이름 과 태그

- 도커 허브에 공개로 이미지를 업로드하는 경우, 이미지 태그 부여
- 태그 형식  
  레지스트리주소/레포지토리이름:버전
- 이미지에 태그를 부여하기
  - 처음부터 이미지 이름을 태그 형식으로 만들어 사용  
    docker tag 원본이미지이름 태그

### 도커 허브에 업로드

- docker push 태그
