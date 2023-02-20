---
title: "Kubernetes  Auth - 카카오 특강"
excerpt: "Kubernets의 인증에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-17
last_modified_at: 2023-02-17
---
```docker 
# 가상화 프로그램
# 운영체제 다운로드

- <가상머신 생성> 
=> Bare-metal(OS가 설치 안된 머신)한 VM을 생성

# 사용자로 접속시 $
# 루트(root)계정에 대한 암호가 설정이 되어있지않은 상태.

# ip add를 쳐서 나온 ip주소를 터미널 프로그램(putty, xshell, mac터미널)으로 접속
##루트 계정 접속이 막혀있음
sudo vi /etc/ssh/sshd_config
:set nu
##줄번호가 붙음
##앞에 주석 삭제, prohibit-password => yes

sudo systemctl restart sshd
##설정을 반영시키기 위해 openssh서버데몬(sshd)을 재시작
## 데몬(daemon) > 백그라운드에서 동작하는 프로그램

<아이피를 수동으로 입력하는 방법>
##되도록이면 dhcp를 통해 자동으로 부여받은 아이피를 고정아이피로 가져가는게 좋음.
vi /etc/netplan
##탭을 치면 자동완성됨
##addresses 에 적을 내용은 ip add를 쳤을때 나오는 내용

netplan apply
##네트워크 설정값 반영.
##만약에 이 서버를 템플릿으로 만들어놓고 복제해서 쓴다면 반드시 겹치지 않는 다른 아이피를 부여

<호스트네임 변경하는법>
hostnamectl set-hostname <변경 이름>

<도커설치>
curl -fsSL https://get.docker.com -o get-docker.sh
##위 페이지에서 get-docker.sh 라는 쉘스크립트 파일을 다운

sudo sh get-docker.sh
##쉘스크립트 파일을 실행.
##실행방법
sh <파일>
./<파일>

chmod +x get-docker.sh
#만약 쉘스크립트 파일이 실행이 안되는경우엔 위의 명령어를 쳐준다.
## +x : excute(실행 권한 부여)
## +r , +w : 각각 읽기 쓰기 권한부여
## chmod 777 get-docker.sh : 모든 권한 부여.

# docker -v를 쳤을때 버전이 뜨면 설치가 잘 된것

<템플릿 생성>
#내가 이후에 생성할 vm들의 고정아이피를 부여할때 어떻게 해야하나?
##192.168.<부여받은 아이피의 뒷자리>.<0~255>
##ex) 192.168.47.10 /16
##가상머신을 복제해서 쓰고싶다면
##위의 과정을 반복한 후
##IP 및 호스트네임을 변경

vi /etc/netplan/00-installer-config.yaml
#ip 변경
##원본 VM의 맨 뒷자리의 아이피가 47이었기때문에 3번째 자리의 IP를 47
##4번째 자리의 아이피는 본인이 원하는 아이피를 사용

netplan apply
#20.04 이상의 우분투 배포판에서만 통용되는 명령어.
#변경하는 순간 터미널 연결은 끊긴다.


#세션을 하나 복사해서, 해당 주소로 다시 접속 후 확인

#도커 스웜, 쿠버네티스 로컬 클러스터를 구성하는 경우에는 반드시 호스트네임을 바꿔야 함
<도커에서 알아야하는것 >
##1.Dockerfile 작성법
##2.Dockerfile로 부터 이미지를 빌드(내가 만든 어플리케이션을 이미지에 삽입)
##3.빌드한 이미지를 도커허브(공식,공용저장소) 혹은 사설저장소에 push하는법.

1. 서버에 springboot로 만든 코드를 빌드해서 배포.

root@clone-test:~# apt-get -y install openjdk-8-jdk
#자바 설치

root@clone-test:~# apt-get -y install maven
#maven 설치

root@clone-test:~# git clone https://github.com/pcmin929/sb_code
#간단한 simple spring boot code를 다운로드


#소스코드 클론 후 디렉토리로 이동.

root@clone-test:~/sb_code# mvn clean install
#maven으로 빌드
#target 폴더에 jar 파일이 생성된것을 확인.

<Dockerfile에 위 내용을 추가>

#FROM : 베이스이미지
#ADD : 호스트의 파일(왼쪽)을 컨테이너에 삽입(오른쪽)
#ENTRYPOINT : 컨테이너가 생성될때 실행되는 명령어.

docker image build -t <이름> <도커파일위치>

##포트는 내 마음대로(0~65536) 설정할 수 있지만 well-known 포트(1000번 이하)는 피하기
docker run -d -p 7878:8085 --name spring sbimage:1.0

docker ps
##현재 ‘동작’중인 컨테이너 목록
##어떤 특정한 이유로 인해 컨테이너가 생성은 되지만 동작은 하고 있지 않은경우 위 목록에 없음

# docker ps 목록에 없는경우
##docker ps -a 를 쳐서 ‘모든’ 컨테이너 목록상에 동작하고 있지 않은 컨테이너가 있는지 확인

> Dockerfile 작성
> docker build 명령어
> docker run
  - 서버에 도커를 설치해서 app이 포함된 컨테이너를 배포

—--------------------
<컨테이너에 랜덤한 포트 부여>
# 이것을 하려면 Dockerfile에 EXPOSE옵션이 필요.

docker run -d -P ~
## -P : 대문자 P는 호스트의 포트를 랜덤하게 부여
##--restart=always 컨테이너가 중지됐을때(systemctl restart docker같은 명령으로 인해) 항상 재시작

curl localhost:포트번호
docker build <저장소아이디>/<이미지잉름>
#도커허브 아이디를 적으셔야함.
docker tag ~ 
#빌드를 새로하거나 혹은 이미 이미지가 존재하는경우 해당 이미지에 새로운 태그를 붙여서 만들 수도 있다.

docker login
# 아이디 비밀번호 입력
docker push <이미지 이름>
# 도커허브로 push

##업로드한 이미지를 hub.docker.com에 로그인해서 확인
##로컬에 있는 oolralra/ubun:1.0 삭제후 컨테이너 실행
##컨테이너 전부 삭제
—------------

<사설저장소>
docker run -dp 5000:5000 --name reg --restart=always -v /registry:/var/lib/registry/docker/registry/v2 registry
- 레포지토리가 하나도 없는 상태.

# 사설저장소를 사용하려면 TLS인증서가 필요한데(HTTPS) insecure로 사용가능하도록 아래와 같이 수정
vi /etc/docker/daemon.json
    {
    "insecure-registries": ["<내서버의IP>:5000"]
    }
##원래 사설소를 사용하려면 TSL 인증서(https)가 있어야 하는데, 인증서 없이도 사설저장소를 쓸 수 있게끔 해주는 절차.

systemctl restart docker
docker push 192.168.47.10:5000/ubun:1.0
docker run -dp 5000:5000 --name reg --restart=always -v /registry:/var/lib/registry/docker/registry/v2 registry
##볼륨 옵션을 추가하여 사설저장소 구성
##컨테이너가 삭제되더라도 이미지는 호스트에 남도록
##컨테이너를 똑같은 옵션으로 재시작하게 되면 이미지는 저장이 되있을것.
##API를 통해 이미지가 존재 하는것을 확인.

docker run -d -P --name <이름> <저장소/이미지>
#사설저장소의 이미지로 컨테이너 실행.

docker run -dp 5000:5000 --name reg --restart=always -v /registry:/var/lib/registry/docker/registry/v2 registry
##사설저장소 구성

docker run -d -p 5001:8080 --name registry_web --link reg -e REGISTRY_URL=http://192.168.47.10:5000/v2 -e REGISTRY_NAME=192.168.47.10:5000 hyper/docker-registry-web
##GUI로 확인할 수 있는 컨테이너 생성. IP부분만 맞춰주면 된다. 5001번 포트로 접속
```