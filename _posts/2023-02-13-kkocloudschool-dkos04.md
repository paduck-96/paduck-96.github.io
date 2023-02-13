---
title: "Kubernetes 리소스 - 카카오 특강"
excerpt: "Kubernets의 리소스 학습"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-13
last_modified_at: 2023-02-13
---
## kubectl
- 쿠버네티스 클러스터에 여러 리소스들을 생성하고 관리할 수 있는 CLI 툴
- https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/
## pod
- 하나 또는 여러 개 애플리케이션 컨테이너의 그룹을 나타내는 k8s 의 추상적인 개념
- 쿠버네티스에서 생성/관리/배포 가능한 가장 작은 컴퓨팅 단위  
![pod 이미지](../../img/pod.png)
## deployment
- pod 찍어내는 템플릿
  - 어떤 앱인지 / 몇개를 띄울 것인지 / hc 어떻게 할지 / 업데이트 방법 등
- 생성시 자동으로 Replica Set / Pod 생성
  - 지정된 파드 개수만큼 실행될 수 있게 관리하는 것이 **Replica Set**    

![deployment 이미지](../../img/deployment.png)
## 서비스 (Service)
  - pod의 생명 주기는 매우 짧아, pod IP를 사용할 수 없음
- 고정된 단일 엔드포인트를 제공
  - 여러개의 pod 를 하나의 엔드포인트 접근하게 도와주는 것
- `같은 label을 가진 pod 들`을 하나의 ip로 접근 가능하게 도와줌
- Application Stack Layer 간에 Service 라는 추상화 계층을 통해 부하 분산 및 네트워크 접근이 편리

![service 이미지](../../img/service.png)
### 서비스 종류
- ClusterIP (default) 
- NodePort
- LoadBalancer
- ExternalName
### 서비스 IP 종류
- Cluster IP
  - 클러스터 내부에서만 사용 가능 ( pod / node 안에서만 가능 )
- External IP
  - 클러스터 외부에서 사용 가능  
  LoadBalancer Type 서비스
## Ingress
- 클러스터 내부 서비스에 대한 외부 접근을 관리하는 오브젝트
- L7 Proxy 를 쿠버네티스 네이티브한 리소스로 관리
  - 다양한 라우팅 룰 설정 가능
  - SSL 인증서 적용 가능
  - Rate Limit 적용 가능
- `반드시 “인그레스 컨트롤러” 도 같이 배포`

![ingress 이미지](../../img/ingress.png)

![loadbalancer 이미지](../../img/loadbalancer.png)

## 명령어
![cheetsheet 이미지](../../img/cheetsheet.png)