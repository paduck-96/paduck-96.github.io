---
title: "MariaDB"
excerpt: ""

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop]

toc: true
toc_sticky: true

date: 2022-11-21
last_modified_at: 2022-11-21
---

# MariaDB

## 개요

### 1. 정의

SQL에 기반을 둔 RDBMS(관계형 데이터 베이스)로 Open Source 형태로 제공
MySQL 개발자가 만들어서 거의 유사

- SQL도 거의 차이가 없음

### 2. 작업 단위

데이터베이스 > 테이블

- 하나의 데이터베이스를 여러 유저가 공유

## 데이터베이스 서버 설치

### 1. OS에 직접 설치

window

- https://mariadb.org/download

`InnoDB vs MyIsam`

- Linked List 와 Array List 의 차이에 기반

### 2. 가상화 컨테이너에 설치

Docker 와 같은 가상화 컨테이너에 설치

- mariaDB image 다운

  - 터미널 에서 docker pull mariadb  
    버전 생략 시 자동 최신 버전
    - `운영체제 설치한 것에 따라 명령어 사용 위치 변경`

- mariaDB 컨테이너 실행

  ```Docker
  docker run --name mariadb -d -p 외부에서접속할포트번호:MariaDB포트번호 -e MYSQL_ROOT_PASSWORD=루트비밀번호 컨테이너이름
  ```

  ###### - docker run --name mariadb -d -p 3306:3306 -e MYSQL_ROOT_PASSWORD=rootPassword mariadb

- docker ps 로 컨테이너 실행 여부 확인 가능

  - -a로 존재 컨테이너 모두 확인 가능

- 컨테이너 중지 / (재)시작

  - docker stop 컨테이너이름 이나 아이디
  - docker (re)start 컨테이너이름 이나 아이디

- 컨테이너 삭제
  - docker rm 컨테이너이름 이나 아이디
  - docker rm -f 컨테이너이름 이나 아이디

## 데이터베이스 IDE 설치

데이터베이스 접속 도구

- 데이터베이스 콘솔 에서 작업할 경우 `복제 없이 원본에 적용`
  - 관리자 의 경우 콘솔에서 가능할 수 있으나  
    사용자 같은 경우에는 복제 본에 작업 필수
- IDE 없이 `외부 데이터베이스에 접속` 어려움

#### DBeaver

오픈 소스 이고 여러 데이터베이스 접속 가능

- 금융 분야 는 토드 나 오렌지 사용

## 데이터베이스 서버 실행 및 접속 확인

### 데이터베이스 접속 도구에서

```
HOST : localhost(로컬 접속 시)
PORT : 3306(도커 설치 시 변경했으면 수정)
DATABASE:mysql(기본 제공)
USERNAME:root(기본 제공)
PASSWORD:설치 시 사용한 비밀번호
```

## 데이터베이스 외부 접속 허용

1. `권한 설정`

```javascript
GRANT all privileges on 사용할DB이름 TO '계정'@'접속할IP;`
 // 모든 데이터베이스 사용 은 *.*
 // 모든 곳에서 접속 시 % 설정
 // 로컬 접속 시 localhost

 //root 계정
 //모든 곳 접속 시
 //모든 데이터베이스 접속 시
 //접속 설정
 GRANT all privileges on *.* TO 'root'@'%'
```

`권한 설정 명령은 설정 후 적용 명령을 수행`

```javascript
FLUSH privileges;
```

2. 서버 설정  
   윈도우 직접 설치 시 해당 과정 생략

- 도커의 경우 **직접 파일 수정 불가**로 컨테이너의 bash 로 접속

```javascript
docker exec -it 컨테이너이름 bash

// 최초 1번 작성
// vim으로 수정
apt update
apt upgrade
apt install vim

vim /etc/mysql/mariadb.conf.d/50-server.cnf
// i > esc > :wq
```

```javascript
/etc/mysql/mariadb.conf.d/50-server.cnf 파일
-> bind-address 부분을 허용할 IP로 변경
// 0.0.0.0 이면 모든 곳에서 접속
```

`실제 서버 설정이라면 Application Server IP 만 허용`

이후 컨테이너 재시작
