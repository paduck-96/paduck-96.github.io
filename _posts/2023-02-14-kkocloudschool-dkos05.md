---
title: "Kubernetes  Auth - 카카오 특강"
excerpt: "Kubernets의 인증에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-14
last_modified_at: 2023-02-14
---
# Kubernetes Auth
- `Kubernets에는 유저 저장소가 존재하지 않음`
## 인증
- 실제로 신고된 내용인지를 판단하는 과정
  - 비행기 표를 예매한 사람
  - 영화 표를 예매한 본인
  - 구매 하기로 한 당사자가 맞는지 등
## 인가
- 특정 리소스 / 기능에 액세스할 수 있는 권한 부여
  - 특정 목적지에 갈 수 있는 비행기 표
  - 특정 영화를 볼 수 있는 영화표
  - 그 물건을 살 수 있는 권한

`쿠버네티스는 내부 인증체계의 종속을 피하고 외부에 의존`
- X.509, HTTP Auth, Proxy Authentication 등...
- 일반적인 웹 서비스 인증은  
IP, PW를 저장해 비교
## kubernetes 인증 기본 방법
- X.509
  - PKI(Public Key Infrastructure) 기술 중 널리 알려진 표준 포맷
- kubernetes `내부 component간 통신`에 사용
  - api-server, controller-manager, etcd, scheduler, kubelet 등
- 사용자가` api-server에 접근하기 위해 사용하는 kubeconfig의 기본 설정`
```docker
# ssh 접속
#해당 키-페어가 있는 위치에서
ssh -i 인스턴스 접속 명령어

# 해당 인스턴스에서 kubeconfig 파일 확인 가능
```
## PKI(Public Key Infrastructure)
- 목적
  - 네트워크를 통해 주고받는 데이터를 누군가 훔쳐볼 수 없게 암호화
### 단방향 암호화
- 한번 암호화되면 복호화를 할 수 없다
- `같은 방법으로 암호화해서 일치 여부`를 판단
- 사용자 패스워드 관리에 적합
### 양방향 암호화, 대칭키
- 하나의 key로 암/복호화가 가능하다
- 송수신자가 `key를 사전에 공유`한다
- M:N 통신 또는 주기적 key의 변경의 고려가 필요하면 안전한 키 공유 방법 마련이 필수적
- 제한된 사설 네트워크 통신(e.g. 회사간, 1:1)에서 사용하기 적합
### 양방향 암호화, 비대칭키
- 송수신자가 각각 키 pair(public, private)를 갖는다
- Private key는 공개하지 않고, `public key를 공개`한다 (교환이 아님)
- public key로 암호화한 데이터는 `private key로 복호화가 가능`하다 (반대도 가능)
- 공개된 네트워크 통신(e.g. 인터넷)에서 사용하기 적합
### 요구사항
- Private Key가 유출되지 않았다는 것을 전제로 한다
- Private Key로 암호화한 데이터는 Public Key로 복호화 할 수 있다
- Public Key는 누구나 쉽게 구할 수 있다. 
- Public Key로 복호화 가능하면 신용할 수 있는 데이터다
### 인증서
- 유효기간이 있는 신분증 (e.g. 주민등록증, 운전면허증)
- 위조되지 않았다는 것만 확실히 할 수 있다면, 그 사람(혹은 사물)이 맞다
- 주민등록증 발급기  
private key  
신분증 감별기  
public key (만 19세라 주장하는 이 신분증이 위조된 것이 아닌가?)  
지문  
인증서 주요 정보를 sha256으로 Hash(단방향 암호화)한 값 (Fingerprint)  
서명  
지문을 private key로 (양방향) 암호화한 것 (Digital Signature)
### RootCA
- RootCA
  - 최상위 인증 기관 
  - RootCA의 Private Key는 절대 털리지 않는다
### 생성과 검증
- 생성
  - kakao가 인증서를 만들고 싶다는 의뢰서(CSR1))를 작성
  - DigiCert의 Private Key로 지문 암호화한 서명을 생성한다
  - kakao가 DigiCert에게 인증서를 받는다
- 검증
  - DigiCert의 Public Key로 서명이 복호화 된다
    - 인증서는 신뢰할 수 있다
  - 인증서 지문과 서명을 복호화한 것이 일치한다
    - 인증서는 변조되지 않았다

## PKI 와 HTTPS
- HTTPS란?
  - HTTP에 인증서를 이용한 상호 신뢰 검증과 암호화 통신을 더한 것
  - TLS(Transport Layer Security)

- PKI 통신을 위한 보안 연결 순서
  - #1 TLS handshake
  - #2 대칭키 암호화 통신
### TLS HandShake
- 클라이언트와 서버는 DigiCert의 private key로 서명된 인증서와 DigiCert의 public key를 보유
- Client Hello  
클라이언트가 서버에 접속 요청과 random data#1를 보냄
- Server Hello  
서버는 자신의 인증서와 random data#2를 보냄
- Server Certificate  
클라이언트가 서버 인증서의 서명을 DigiCert의 public key로 복호화해본다
- Certificate request  
서버는 설정에 따라 통신 과정에 클라이언트 인증서를 요구할 수 있음
- Certificate  
클라이언트가 자신의 인증서를 보냄
- Server hello done  
DigiCert의 public key로 서명을 복호화하여 지문이 일치하는지 확인한다
- Client Key Exchange  
Pre-Master Secret을 생성하고 서버의 public key로 암호화해서 보냄
- Client Key Exchange  
pre-master secret에 randomdata를 salt로 써서 대칭키를 각각 만듬
- 키교환이 끝났으므로 인증서 없이 대칭키만으로 암호화 통신을 한다
## TLS 인증서 - 쿠버네티스
### k8s 인증서 목록
- RootCA  
  - Cluster 안에서만 먹히는 CA 인증서
- 무엇을 더 생성해야 하는가?
  - kubernetes admin  
클라이언트 인증서 및 kubeconfig
  - etcd  
클라이언트/서버 인증서
  - kube-api-server  
클라이언트/서버 인증서
  - kube-controller-manager  
클라이언트 인증서 및 kubeconfig
  - kube-scheduler  
클라이언트 인증서 및 kubeconfig
  - kubelet  
클라리언트/서버 인증서 및 kubeconfig
### 생성 방법
- kubeadm
  - 인증서 생성을 알아서 해줌
  - 단, CA 인증서 유효기간은 10년, CA Signed된 인증서는 유효기간 1년
    - brute force attack으로 TLS는 1년이면 뚫린다
- kubespray
  - 구 버전은 kubespray가 인증서를 스크립트로 생성
  - release-2.11부터 kubeadm을 내부적으로 사용
- DKOS
  - DKOS는 kubespray를 사용
  - 2021년 1분기까지는 6개월마다 인증서 갱신을 해왔음
  - kubeadm을 다시 빌드하여 100년 인증서 생성하고 있음
  - 23년부터 kubespray/kubeadm를 사용하지 않음
