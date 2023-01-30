---
title: "Docker Compose"
excerpt: "Docker Compose에 대해 학습하고,
관련 명령어를 확인한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, docker]

toc: true
toc_sticky: true

date: 2023-01-30
last_modified_at: 2023-01-30

---
## 9. Docker Compose
### Docker Compose
- `시스템 구축 과 관련된 명령어를 하나의 텍스트 파일`에 기재해서 명령어 한번으로 시스템 전체를 실행  
종료와 폐기까지는 한 번 할 수 있도록 도와주는 도구
- **YAML 포맷**으로 기재된 정의 파일을 이용해서  
`시스템을 일괄 실행 또는 일괄 종료 및 삭제할 수 있는 도구`
- 작성된 명령어는 도커 명령어 와 유사하지만 `도커 명령어는 아님`

### Dockerfile 과 차이
- Dockerfile  
이미지를 만들기 위한 스크립트
- Docker Compose  
`Docker run 명령어를 여러 개 모아놓은 것`과 유사하고  
 컨테이너 와 주변 환경을 생성할 수 있음
   - 네트워크 나 볼륨 까지 생성 가능

### Kubernetes 와 의 차이
- **쿠버네티스는** 도커 컨테이너를 관리하는 도구  
`여러 개의 컨테이너를 다루는 것` 과 관계가 깊음
- Docker Compose에는 컨테이너를 관리하는 기능은 없음

### 사용하기 전 준비 사항
- 도커 컴포즈 설치
- 도커 데스크탑을 사용하는 경우는 별도의 설치 과정이 필요 없음
- 리눅스에서는 Python3 Runtime이 필요
```docker
sudo apt install -y python3 python3-pip
#python 3의 runtime 과 pip 설치
sudo pip3 install docker-compose
#docker compose 설치
```
### 사용법
- docker compose 가 사용할 디렉토리를 생성

- 정의 파일을 디렉토리에 작성 
  - `하나의 디렉토리에 정의 파일은 1개`만 존재  
정의 내에서 사용될 파일들도 모두 동일한 디렉토리에 배치

### 작성법
```docker
docker run --name apa000ex2 -d -p 8080:80 httpd
#httpd:latest 버전의 이미지로 apa000ex2 라는 컨테이너를 생성,  
#백그라운드에서 실행되고,  
#호스트 컴퓨터에서는 8080 포트를 이용, 80 번 포트로 접속하게 생성

version "3"
services:
	apa000ex2:
		image: httpd
		ports:
			- 8080:80
		restart: always

docker run --name wordpress000ex12 -dit --net=wordpress000net1 -p 8085:80 -e WORDPRESS_DB_HOST=mysql000ex11 -e WORDPRESS_DB_NAME=wordpress000db -e WORDPRESS_DB_USER=wordpress000kun -e WORDPRESS_DB_PASSOWORD=wkunpass wordpress

version "3"
services:
	wordpress000ex12:
		image: wordpress
		ports:
			- 8085:80
		networks:
			- wordpress000net1
		depends_on:
			- mysql000ex11
		restart: always
		environment:
			WORDPRESS_DB_HOST=mysql000ex11
			WORDPRESS_DB_NAME=wordpress000db
			WORDPRESS_DB_USER=wordpress000kun
			WORDPRESS_DB_PASSWORD=wkunpass
```

- 정의 파일의 형식은 yml
  - 기본적인 파일 이름은 docker-compose.yml 인데  
  실행할 때 -f 옵션을 이용하면 다른 이름도 가능
- 작성은 맨 앞에 compose 버전을 설정한 후  
services, networks, volumes를 기재
  - docker-compose 와 kubernetes에서는  
  `컨테이너의 집합`을 **service**라는 용어로 부름

- 작성을 할 때는  
주항목 -> 이름 추가 -> 설정 순서로 작성
  - 각 이름들은 `주항목보다 한 단 들여쓰기`를 해야 하는데  
   공백 개수는 몇 개 이던지 상관없지만,  
   한 번 한 단을 공백 한 개로 정하면 그 뒤로는 계속 같은 공백
  - 원칙적으로는 탭은 안되는데,  
  대부분 editor 가 탭 4개 정도를 공백으로 설정해놓은 상태로 사용 가능
- 설정을 작성할 때 `앞에 하나의 공백`을 추가
- 하나의 설정인 경우는 바로 작성하면 되지만  
여러 개의 설정을 하는 경우는 앞에 - 를 추가

### 컴포즈 파일의 항목
- 주항목
  - **services**  
  컨테이너(독립된 프로그램 과 데이터)
  - **networks**  
  여러 개의 컨테이너를 `하나로 묶어서 통신이 가능`하도록 하고자 할 때 사용
  - **volumes**  
  호스트 컴퓨터나 도커 엔진의 영역 과 `컨테이너 내부 영역을 매핑`시키고자 할 때 사용

- 자주 사용하는 정의 내용
```docker
image		# 사용할 이미지를 설정
networks		--net 
volumes		-v, --mount
ports		-p
environment	-e
depends_on	# 다른 서비스에 대한 의존 관계, 컨테이너를 생성하는 순서 나 연동 여부를 정의
restart		# 컨테이너를 종료 시 재시작 여부를 설정
		no(재시작하지 않음)
        always(항상 재시작)
        on-failure(프로세스가 0 외의 상태로 종료되었다면 재시작)
        unless-stopped(종료 의 경우는 재시작하지 않고 그 외의 경우는 재시작)

logging		--log-driver에 해당
# 로그 출력 대상을 설정
```

### 컴포즈 파일 작성
- 생성할 네트워크, 볼륨, 컨테이너 정보
  - 네트워크이름  
  wordpress000net1
  - MySQL 볼륨  
  mysql000vol11
  - 워드프레스 볼륨  
  wordpress000vol12
  - MySQL 컨테이너  
  mysql000ex11
  - 워드프레스 컨테이너  
  wordpress000ex12
```docker
# MySQL 컨테이너
##이미지
mysql:5.7
##네트워크
wordpress000net1
##사용할 볼륨
mysql000vol11
##마운트 위치
/var/lib/mysql(mysql의 데이터 디렉토리)
##재시작 설정
always
##환경 변수
	##루트 패스워드(MYSQL_ROOT_PASSWORD)
    myrootpass
	##사용할 데이터베이스이름(MYSQL_DATABASE)
    wordpress000db
	##사용자 이름(MSQL_USER)
    wordpress000kun
	##사용자 비밀번호(MSQL_PASSWORD)
    wkunpass

# wordpress 컨테이너 
##의존관계
mysql000ex11
##이미지
wordpress
##네트워크
wordpress000net1
##사용할 볼륨
wordpress000vol12
##마운트 위치
/var/www/html(html 파일의 경로)
##포트설정
8085:80
##환경 변수
	##데이터베이스 컨테이너이름(WORDPRESS_DB_HOST)
    mysql000ex11
	##데이터베이스 이름(WORDPRESS_DB_NAME)
    wordpress000db
	##데이터베이스 사용자 이름(WORDPRESS_DB_USER)
    wordpress000kun
	##데이터베이스 비밀번호(WORDPRESS_DB_PASSWORD)
    wkunpass

#디렉토리 생성
com_folder

#디렉토리에 docker-compose.yml 파일을 추가하고 작성
version: "3"

services:
  mysql000ex11:
    image: mysql:5.7
    networks:
      - wordpress000net1
    volumes:
      - mysql000vol11:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: myrootpass
      MYSQL_DATABASE: wordpress000db
      MYSQL_USER: wordpress000kun
      MYSQL_PASSWORD: kunpass
  wordpress000ex12:
    depends_on:
      - mysql000ex11
    image: wordpress
    networks:
      - wordpress000net1
    volumes:
      - wordpress000vol12:/var/www/html
    ports:
      - 8085:80
    restart: always
    environment:
      WORDPRESS_DB_HOST: mysql000ex11
      WORDPRESS_DB_NAME: wordpress000db
      WORDPRESS_DB_USER: wordpress000kun
      WORDPRESS_DB_PASSWORD: kunpass
networks:
  wordpress000net1:
volumes:
  mysql000vol11:
  wordpress000vol12:
```
- MySQL 8.0을 사용하고자 하는 경우  
데이터베이스에 접속해서 사용자 패스워드 설정을 변경,  
mysql 8.0 컨테이너를 생성할 때 아래 커맨드를 추가
```docker
(공백 4개 추가하고)command: mysqld --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password
```
### 컴포즈 관련 명령어
- docker compose 명령
  - 명령은 up, down, stop  
**up은** 컴포즈 파일에 정의된 `컨테이너 및 네트워크를 생성`  
**down은** 컨테이너와 네트워크를 종료하고 삭제  
**stop은** 컨테이너와 네트워를 종료

- docker-compose -f 야믈파일의경로 up 옵션
  - 옵션  
-d			백그라운드로 실행  
--no-color		화면 출력 내용을 흑백으로 출력  
--no-deps			링크된 서비스를 실행하지 않음  
--force-recreate		설정 또는 이미지가 변경되지 않더라도 컨테이너를 재생성  
--no-create		컨테이너가 이미 존재하는 경우 재생성하지 않음  
--no-build		이미지가 없어도 이미지를 빌드하지 않음  
--build			컨테이너를 실행하기 전에 이미지를 빌드  
--abort-on-container-exit	컨테이너가 하나로도 종료되면 모든 컨테이너를 종료  
-t, -timeout		컨테이너를 종료할 때 타임아웃 옵션으로 기본은 10초  
--remove-orphans		컴포즈 파일에 정의되지 않은 서비스의 컨테이너를 삭제  
--scale			컨테이너의 수 변경  

- docker-compose -f compose 파일 경로 down 옵션  
  - 옵션  
--rmi 종류		삭제할 때 이미지도 같이 삭제하는데 all로 설정하면 모든 이미지가 삭제되고 local로 지정하면 태그가 없는 이미지만 삭제  
-v, --volumes		볼륨을 삭제  
--remove-orphans		컴포즈 파일에 정의되지 않은 서비스의 컨테이너도 삭제  

- docker-compose -f compose 파일 경로 stop 옵션  
컨테이너를 중지하고 삭제하지는 않음

- 그 이외 명령  
ps		:컨테이너 목록 출력  
config		: 컴포즈 파일을 확인하고 내용을 출력  
port		: 포트 설정 내용을 출력  
logs		:컨테이너가 출력한 내용을 화면에 출력  
start		: 컨테이너 시작  
kill		: 컨테이너 강제 종료  
exec		: 명령어를 실행  
run		: 컨테이너 실행  
create		: 컨테이너 생성  
restart		: 컨테이너 재시작  
pause		: 컨테이너 일시 중지   
unpause		: 컨테이너 일시 중지 해제  
rm		: 종료된 컨테이너 삭제  
build		: 컨테이너에 사용되는 이미지를 빌드 또는 재빌드  
pull		: 이미지 다운로드  
scale		: 컨테이너 수를 지정 - 동일한 이미지를 사용하는 여러 개 컨테이너 생성(실제로는 kubenetes를 이용하는 경우가 많음)  
events		: 컨테이너로부터 실시간으로 이벤트를 수신  
help		: 도움말   

### 실습
- 컴포즈 내용을 이용해서 생성  
docker-compose -f 파일경로 up -d

- 실행 확인  
docker ps

- 웹 브라우저에서 접속  
http://localhost:8085
(wordpress 화면이 나오는지 확인)

- 컨테이너 와 네트워크 삭제  
docker-compose -f 파일경로 down

- 컨테이너 삭제 여부 확인  
docker ps  
- 실행 중인 컨테이너 목록 보기  
docker ps -a  
- 모든 컨테이너 목록 보기  
docker volume list  
- 볼륨 목록 확인  
docker volume rm 볼륨이름







