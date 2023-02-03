---
title: "Kubernetes PV"
excerpt: "쿠버네티스 PV에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-03
last_modified_at: 2023-02-03

---

# 클라우드

클라우드 <-> On-Premise

- 원격지에 접속해 확인되는 서비스를 이용할 경우,  
  클라우드 서비스를 이용하고 있는 것

## IaaS

- 기반 시설만 제공

## PaaS

- 운영,개발 환경을 제공
- 플랫폼을 제공

## SaaS

- 완성된 프로그램을 대여

## Public IP(공인 아이피)

- 외부에서 통용되는 아이피
- 서버에 할당시 찾아갈 수 있는 주소

## Private IP(사설아이피)

- 내부에서만 통용되는 아이피
- 외부에서 찾아갈 수 없음

```docker
# ec2
# 인스턴스 생성
# 키-페어 생성
# AWS에 접속하기 위한 세션을 하나 생성.
##설정에서 public key로 바꾸고 키-페어 등록

#[ec2-user@ip-172-31-43-159 ~]$
#사용자 상태로 관리자는 #, 사용시 sudo 명령어 사용

# 웹서버 데몬
sudo yum -y install httpd
#(daemon, 백그라운드에서 동작하는 프로그램)
##sudo : 관리자 권한으로 명령을 실행하겠다.
##yum : 패키지 관리 명령어.
## -y : 의존성이 물음에 대해 ‘yes’를 누르겠다.
##install : 설치하겠다.
##httpd : http 데몬.

# 데몬의 상태(status)
sudo systemctl status httpd

# 웹서버를 동작.
sudo systemctl restart httpd
#공인아이피로 접속이 잘 되는것을 확인 가능.

```
