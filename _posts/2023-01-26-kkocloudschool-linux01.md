---
title: "Shell 과 명령어에 대한 학습"
excerpt: "Shell programming에 대해 학습하고,
명령어를 통한 Shell 스크립트 작성을 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, shell, shell script]

toc: true
toc_sticky: true

date: 2023-01-26
last_modified_at: 2023-01-26

---

...

## 3. Editor

### Text Editor

- gedit

### vi 편집기

- vi 파일명  
  파일 없으면 생성, 있으면 수정
- 모드
  - **명령**  
    입력 모드로 전환할 때는 i, l, a, A, o, O 를 누르면 가능
  - **입력**  
    명령 모드로 전환할 때는 esc
  - 라인
- 작업을 수행한 후 종료
  - :q!  
    저장하지 않고 종료
  - :wq!  
    `저장하고 종료`

## 4. Process

### Process

- 실행 중인 프로그램

### 프로세스 번호 와 작업 번호

- **프로세스 번호**  
  `프로세스를 구분`하기 위한 번호
- **작업 번호**  
  백그라운드 프로세스가 CPU를 점유해서 `실행될 떄의 순서` 번호

### Daemon Process

- `다른 서비스를 제공하기 위해 존재하는 프로세스`
- 평상시에는 대기 상태로 존재하다,  
  서비스 요청시 해당 서비스를 제공

### 부모 자식 관계의 프로세스

- 부모 프로세스 종료시, 자식 프로세스도 종료
- **고아** 프로세스  
  자식 프로세스가 `종료되지 않은 상태`에서 부모 프로세스가 종료
- **좀비** 프로세스  
  자식 프로세스가 `정상적으로 종료되지 않은` 상태로 부모 프로세스 종료

### **PS** 명령

- 프로세스 목록 출력

### **Kill** 명령

- 프로세스 종료
- pkill 은 프로세스 이름으로 종료,  
  kill은 `pid로 종료`

## 5. 패키지 관련 명령

### apt 명령

- Advanced Package Tool의 약자로 `패키지 관련 명령`
  - Fedora 와 CentOS 에서는 yum이라는 명령

### **apt-cache**

- `패키지 와 관련된 정보 출력`
- 기본 형식  
  apt-cache [옵션][하위 명령]
- 옵션
  - f  
    전체 정보 출력
  - h  
    도움말 출력
- 하위 명령
  - stats
    - 통계
  - dump
    - 패키지 업그레이드
  - pkgnames
    - 사용 가능한 패키지 이름 출력
  - search [키워드]
    - 검색
  - show [패키지 이름]
    - 패키지 정보 출력
  - showpkg [패키지 이름]
    - 패키지의 의존성과 역의존성에 대한 정보 출력

### **apt-get**

- `패키지 설치 및 업데이트`
- 기본 형식
  - apt-get [옵션][하위 명령]
- 옵션
  - d  
    다운로드
  - f  
    깨진 패키지 수정
  - h  
    도움말
- 서브 명령
  - update
    새로운 패키지 정보 가져오기
  - upgrade  
    모든 패키지를 최신 버전으로 업그레이드
  - install 패키지이름  
    설치
  - remove 패키지이름  
    제거
  - purge 패키지이름  
    패키지 와 설정 파일 모두 제거
  - autoremove  
    시스템에 설치된 패키지를 자동으로 정리 및 삭제
  - autoclean  
    오래된 패키지 와 불완전한 다운로드 패키지 제거
  - check  
    의존성이 꺠진 패키지 확인
  - clean  
    캐싱되어 있는 패키지 삭제

## 6. gcc 컴파일러

- `C 컴파일러`로 리눅스 나 유닉스에서는 제공
- C 프로그래머  
  Visual C++(MS-C)
- ANSI-C 프로그래머  
  gcc 컴파일러를 가지고 프로그래밍

## 7. Shell Script

### **Shell** 의 기능

- `사용자 와 커널 사이에서 중계자 역할`을 수행하는 프로그램

### 종류

- **bourne shell**  
  `처리 속도가 빨라`서 초창기에 많이 이용하고,  
  `sh 명령`을 사용하는 경우가 bourne shell
- **C shell**  
  bourne shell의 기능이 확장된 것으로 작성 형식이 C언어와 유사,  
  `csh 라는 명령`을 사용하는데 `처리 속도가 느린` 편
- **Korn shell**  
  `hourne shell 과 호환성`을 제공하고 처리 속도가 빠른 편이며,  
  우분투에서는 별도의 설치가 필요
- **bash shell**  
  `3가지 shell의 호환성을 유지`하면서 기능 강화한 shell,  
  우분투 리눅스의 기본 쉘이면서 `bash 명령` 사용
  - 우분투 6.10 이상은 dash shell 이 기본 shell
- **dash shell**  
  bash shell의 크기를 줄여서 `빠르게 실행`시키도록 만든 shell

### 기본 shell 확인

```shell
ls -l /bin/sh
```

### 셸에서 환경 변수

- `변수명 앞에 $`를 붙여야 함
- 환경 변수의 이름은 `대문자`가 관례
- HOME(사용자의 홈 디렉토리) 라는 환경 변수 값 확인  
  echo $HOME
- bash 버전 확인이라는 환경 변수 값 확인  
  echo $BASH_VERSION

### 변수 생성

- `이름=문자열 형태`로 작성, `공백을 포함하면 안됨`
- `출력에는 echo` 명령어 사용

### 셸 변수 와 환경 변수 사이의 전환

- export [옵션][셀 변수]
- 옵션으로 -n 을 설정하면 환경 변수를 셸 변수로 전환
- export 셸변수=문자열 의 형태로 설정 가능
- env 로 환경 변수 확인 가능

### 셀 변수 삭제

- unset 변수이름

### Shell Script

- 셸이나 커맨드라인 인터프리터에서 수행되도록 작성된,  
  `운영체제를 위한 스크립트`
- 장점
  - 다른 프로그래밍 언어에어 작성된 코드보다 `빠르게 처리`
- 단점
  - `고품질의 코드 와 확장을 기록하기가 힘듦`

### 셸 스크립트 작성 및 실행

- 현재 디렉토리에 셸 스크립트 파일 생성 후 작성

  ```shell
  #! /bin/sh

  echo "Name Print"
  echo ">> Connect Name: " $USERNAME

  exit 0
  ```

- 실행은 `sh 파일명.sh`
- 실행시 다른 디렉토리에 존재하면 실행 오류 발생  
  `sh 파일을 현재 디렉토리`로 옮기거나,  
  `chmod +x 스크립트파일명 명령`을 이용해 실행 가능 속성 추가

### 키보드 입력

- read 변수명
- 스크립트 파일 생성

```shell
# 파일 생성
sudo gedit inout.sh >>>

#! /bin/sh

echo "Input: "
read input_string
echo "Output: $input_string"

exit 0
```

- 실행

### 산술 연산

- 백틱(\`) 다음에 export 입력 후 산술 연산 수행,  
  다시 백틱(\`)으로 막으면 완료

### 조건문

```shell
if [조건식]
then
    조건식이 참일 경우 수행
fi
```

- 같다 는 = 으로 사용
- [ ] 에서는 `뒤 와 앞에 공백`이 있어야 하고,  
       = 도 `좌우에 공백`이 있어야 함

### 관계 연산자

- 문자열 비교
  - =
  - !=
  - -n  
    null 이 아닌 값
  - -z  
    null 값
- 산술 비교
  - -eq
  - -ne
  - -gt
  - -ge
  - -lt
  - -le
  - !

### **case ~ esac**

- if 구문은 조건이 많아지면 구문이 복잡해지는데,  
  case ~ esac 구문은 여러 개의 조건을 펼쳐놓고  
  어느 조건에 해당하는지 판별해 명령을 수행
  - if ~ else 에 비해 간결하고 이해하기 쉬움

```shell
case 파라미터 또는 키보드입력값 in
  조건1)
  조건1에 해당할 경우 실행 명령
  조건2)
  조건2에 해당할 경우 실행 명령
  ...
  *)
  앞에서 주어진 조건 외 모든 경우 실행 명령
esac
```

- 작성

```shell
#! /bin/sh
echo ">> season choice : Spring / Summer/Fall/Winter"
case "$1" in
  Spring)
  echo ">> choice : Spring";;
  Summer)
  echo ">> choice : Summer";;
  Fall)
  echo ">> choice : Fall";;
  Winter)
  echo ">> choice : Winter";;
  *)
  echo "No Choice ";;
esac

exit 0
```

- 실행은 sh 파일명 Spring(파라미터)

### and 와 or

- and  
  -a 또는 &&
- or  
  -o 또는 ||

### 반복문

- for

```shell
for 임시변수 in [범위](리스트 또는 배열, 묶음 등)
do
  반복 수행 내용
done
```

```shell
#! /bin/sh
cnt = 0
for num in 1 2 3 4 5
do
  echo " >>No.$cnt : $num"
  cnt=`expr $cnt + 1`
done

exit 0
```

- **seq**  
  범위를 만들 때 사용하는 것으로 \`seq [시작값][종료값] \`
- 범위 대신에 디렉토리 이름도 가능
- **while**

```shell
while [조건식]
do
  반복 수행할 내용
done
```

- until

```shell
until [조건식]
do
  조건식이 false이면 반복 수행할 내용
done
```

### 기타 명령어

- break  
  반복문 중단
- continue  
  조건식으로 이동
- **exit**  
  프로그램 종료 명령어로 정수를 하나 같이 전달  
  이 정수는 운영체제에게 종료된 이유를 설명하는 숫자
- return  
  함수의 수행을 종료하고 호출한 곳으로 돌아가는 명령어

### 함수 와 파라미터

- **function**  
  자주 사용되는 코드를 하나의 이름으로 묶어서 사용하기 위한 개념
- 함수 사용 이유
  - 한 번에 수행되어야 하는 코드가 너무 길어서 분할(**모듈화**)
  - 동일한 코드를 자주 호출
- 선언

```shell
함수이름( ){
  함수내용
  $1
  $2
}
```

- 함수 호출  
  함수이름 파리미터1 파라미터2...

## 8. **네트워크**

### 네트워크 인터페이스 설정 확인

- ifconfig 인터페이스이름 옵션 값
- 옵션
  - -a
  - sudo apt install net-tools 명령으로 설치 후 수행

### 주소 체제

- **IPv4**  
  32비트 주소 체제
  - 8bit 씩 묶어서 10진수로 표현  
    127.0.0.1 : Loopback
- **IPv6**  
  128비트 주소 체제
  - 4비트씩 묶고 다시 4개씩 묶어서 16진수로 표현  
    0::0::0::0::0::0::0::1(Loopback)

### 용어

- **netmask**  
  `하나의 그룹을 만들기 위한 영역을 설정`하는 주소
  - 0, 1로 구성되는데 1인 부분이 같으면 동일한 네트워크
- **broadcast**  
  방송을 위한 주소
  - `네트워크의 모든 구성 요소에게 데이터 전달` 시 사용
- **gateway**  
  `다른 통신망과 원할한 접속`을 유지하기 위해 사용되는 네트워크 포인트

### **ping**

- ping IP 주소 나 URL  
  메시지를 전송하고 돌려받음

### **DNS**(Domain Name Service)

- `IP 주소와 도메인 사이의 변환`을 위한 서비스

### **nslookup**

- nslookup 호스트네임 또는 DNS  
  `호스트 네임이나 DNS 서버가 제대로 동작` 중인지 확인

## 9. Telnet

### **텔넷**

- `원격지에 있는 서버에 접속`하는 프로그램

### 텔넷 서버 생성

- 텔넷 서버 설정을 위한 패키지 설치  
  sudo apt-get install xinetd  
  sudo apt-get install telnetd
- 환경 설정 파일을 열어서 추가  
  sudo gedit /etc/xinedt.conf
- 데몬으로 동작  
  sudo /etc/init.d/xinetd restart

### 텔넷 서버 접속

- 자기 컴퓨터에 접속  
  telnet 0
- 다른 컴퓨터에 접속  
  telnet  
  open IP주소

### Open SSH

- 텔넷은 서버에 접속할 수 있는 대표적인 방법이지만,  
  `데이터가 암호회되지 않은 상태`로 전송되어 보얀이 취약,  
  패킷 캡쳐 프로그램을 이용하면 데이터 전부 확인 가능
- **SSH**(Secure Shell)  
  `공개키 방식의 암호화 방식`을 이용해서 원격지 시스템에 접근,  
  암호화된 메시지를 전송할 수 있는 시스템

  - **PuTTY가** 가장 많이 사용됌

- PuTTY를 이용해 리눅스 서버에 접근하려면 23번 포트 활성화  
  sudo iptables -A INPUT -p tcp --dport23 -j ACCEPT  
  sudo iptables -save  
  sudo /etc/init.d/xinetd restart

## 10. MariaDB 사용

### 설치

- sudo apt-get update  
  sudo apt-get install mariadb-server

### maria db 재시작

- systemctl status mariadb.service  
  || sudo systemctl start mysql

### 접속

- sudo mysql -u root -p 수행 후 관리자 비밀번호 입력

## 11. Web Server 생성

### apache2 패키지 설치

- sudo apt-get install apache2

### 서비스 실행

- systemctl status apache2  
  || sudo service apache2 start

### 서비스 확인

- ps ef | grep apache

### 브라우저에서 확인

### 방화벽 설정

- 현재 컴퓨터가 아닌 다른 컴퓨터에서 접속이 안되는 경우,  
  방화벽에서 80번 포트를 제외해주어야 함
- 방화벽 실행  
  sudo ufw enable
- 포트 허용  
  sudo ufw allow 80/tcp
- 확인  
  sudo ufw status
- 서비스 재시작  
  systemctl status apache2  
   || sudo service apache2 start
