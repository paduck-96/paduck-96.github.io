---
title: "Cloud란? - 카카오 특강"
excerpt: "Cloud에 대한 전반적인 내용을 카카오 개발자를 통해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, cloud]

toc: true
toc_sticky: true

date: 2023-02-09
last_modified_at: 2023-02-09
---

# What is Cloud?

## 클라우드

- 인터넷 통신망 어딘가에 IT 자원이 있고,  
  원하는 대로 가져가고 사용한 만큼 비용을 지불하는 시스템

### 장점

- 서버 놓을 자리, 서버에 활용할 물리적 세팅(케이블 등)  
  서버 환경 세팅(톰캣, IDE 등) 의 고려가 불필요

### 단점

- 장애 종속성
  - 제공 업체의 장애 또는 중단의 영향
- 보안 위험
  - 애플리케이션과 데이터, 클라우드 내 높은 보안 필요

### 서비스 형태

- On premises
  - 데이터센터, 서바 장비 등 부터 사용자가 설계 및 설정
- IaaS
  - 네트워크, 스토리지, 서버, 가상화 서버는 제공
  - OS, 미들웨어(톰캣 등), 런타임 등은 사용자가 설정
- PaaS
  - 서버, OS, 미들웨어 까지 제공
  - 런타임 환경과 코드만 사용자가 설정
- SaaS
  - 전부 제공

### 클라우드 종류

- Private
- Public
- Hybrid
- Multi
  - 여러 클라우드 사용

---

# 클라우드 네이티브

- 클라우드의 이점을 최대한 활용할 수 있도록 APP 구축

## 목표

- 유연하고, 가용성이 높으며 확장 가능한 소프트웨어 제작
  - CI/CD
  - DevOps
  - Microservices
  - Containers

## CI/CD

### CI

- 빌드/테스트 자동화
- 코드 변경 사항이 자동으로 빌드, 테스트
  - 소스/버전 관리 시스템에 변경사항을 정기적으로 커밋하고,  
    이 과정에 빌드와 테스트가 자동으로 이루어져  
    문제 발생 제거

### CD

- 배포 자동화 과정
- 레퍼지토리까지, 더 나아가서는 클러스트까지 자동으로 배포

## DevOps

- 개발과 운영의 결합,  
  개발자와 운영자 사이의 소통, 협업, 통합 및 자동화 강조

### 원칙

- 협업 및 커뮤니케이션
- 소프트웨어 개발 라이프사이클 자동화
  - 인적 오류를 줄이고, 효율성 개선
- 지속적인 개선 및 낭비 최소화

### 이점

- 빠른 시장 출시
- 향상된 협업
- 효율성, 품질 향상
- 고객 만족도, 보안 항상

## MSA

- 소프트웨어를 독립적으로 배치 가능한 작은 서비스 조합으로 설계

### 클라우드 도입 이후

- after cloud
  - 비즈니스 로직을 제외한 후 클라우드로 이관

## 방법론의 전화

- 복잡하고 불확실한 비즈니스 한경과 시장 니즈
  - 데브옵스  
    일하는 방식, 문화 바꾸기
  - 마이크로서비스  
    소프트웨어 아키텍처에 빠르게 반응
  - 클라우드  
    인프라를 유연하고 확장 가능하게

---

# Container

## IT에서의 선적

- 배 > 서버
- 화물 > 컨테이너로 패키징된 APP
- 선적 기중기 > 쿠버네티스
  - 많은 서버에 컨테이너 자동 배포
  - 마이크로서비스에 효과적
  - 대표적인 리눅스 컨테이너 관리 도구인 도커가 있음

## 컨테이너의 발전

- 전통적인 배포
  - 하나의 컴퓨터에 하나의 서버
- 가상화 배포
  - 호스트 OS 위에 Hypervisor를 통해 다양한 VM 얹기
- 컨테이너 배포
  - 호스트 OS 위에 Docker를 통해 컨테이너로 개발
- 쿠버네티스 배포
  - Docker의 개수가 많아질 때 통합

## 컨테이너란

- 컨테이너는 실행 환경과 성능을 격리, 표준화
- 애플리케이션을 OS 환경까지 가상화해서 패키징
  - 어플리케이션 런타임 표준화
- 애플리케이션의 성능 격리
  - IT 리소스에 대한 설정을 통해 격리 실행
- `컨테이너는 가상 머신이 아닌, 프로세스`
  - 격리된 환경에서 실행되는 프로세스
- 운영체제 관점에서,  
  컨테이너 이미지 > 실행 파일  
  컨테이너 > 프로세스

## 작은 격리 컨테이너

```docker
chroot 명령어로 격리 환경을 생성할 수 있음
# pwd 를 통해 현재 root 위치를 확인해서 격리 여부 확인

# docker도 마찬가지로 하나의 로컬디스크 안에서 공간을 같이 사용
## docker layer로 이미지가 확인되므로,
## lowerlayer, upperlayer, mergedlayer 중 각 최상단이 확인
## / 경로 이상을 나가지 못할 뿐, 하나의 컨테이너로 도커 전체에 종료가 이루어질 수 있음
```

- cgroups 와 namespace를 결합하여 애플리케이션을 위한 고립된 환경 제공

  - cgroups  
    프로세스별 자원 할당 관리  
    쓸 수 있는 사용량 제한
  - namespace  
    장치를 격리해서 관리  
    볼 수 있는 범위 제한

- docker run ... 진행시  
  실행한 터미널이 init 프로세스가 됨
  - 컨테이너 하나에 여러 프로세스를 올리는 건 지양
