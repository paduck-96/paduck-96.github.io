---
title: "Kubernetes란? - 카카오 특강"
excerpt: "Kubernetes에 대한 전반적인 내용을 카카오 개발자를 통해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-10
last_modified_at: 2023-02-10
---

# 쿠버네티스

## 이론

- k8s라고도 불리기도 함
- 컨테이너 배포, 자동 복구, 확장 등 컨테이너화된 app 관리를 위한 시스템
- GKE, EKS, AKS 등 있음

## 기능

### 스케쥴링

- 다수의 컨테이너를 다수의 호스트(클러스터)에 적절히 분산

### 상태 관리

- 원하는 상태 로 실행상태를 유지

### 배포 / 스케일링

- 서비스 중단 없이 자동화 배포를 통해 스케일 확장/축소

### 서비스 디스커버리

- 새로 뜨거나 옮겨간 컨테이너에 접근 방법 제공

### Resoure 연결 관리

- 컨테이너 라이프 사이클에 맞춰 관련 자원을 연결

## 동작 원리

### Master 노드

- `kube-apiserver`
  - 워커 노드와 소통하며, etcd에 저장
- `kube-controller-manager`
  - 실제 리소스 관리를 수행하는 컨트롤러의 집합
- `etcd`
  - 모든 오브젝트의 상태를 key/value 로 저장
- `kube-scheduler`
  - 새 pod 의 생성을 관찰해 스케쥴링

### Worker 노드

- `kubelet`
  - 노드 와 파드의 상태를 파악, 실행, 보고

### 핵심

`Spec 과 Status 필드를 지속적으로 확인해 비교해가면서 관리`

---

# DKOS

- 카카오에서 사용하고 있는 Kubernetes 오케스트레이션 툴

---

# 쿠버네티스의 구조

[구조 이미지]total-image-url
total-image-url:https://www.figma.com/file/djLa3AdAxMrBLqa9zRbNfW/Untitled?node-id=0%3A1&t=pwGpaw2IficackJD-0

## 컨트롤러

- Deployments
  - 배포용, `ReplicaSet을 만들어 배포`
- DeamonSet
  - `노드마다 1개씩` 띄울 수 있음
- StatefulSets
  - 순서대로 뜨고 내려가고, `특정 스토리지나 네트워크` 필요시
- CronJob
  - `주기적으로 실행될 잡`

---

# 클라우드 네이티브 패턴

- 대규모 컨테이너 마이크로 서비스를 자동화하기 위한 패턴

## 컨테이너 필요 사항

- Log는 Docker `자체에서 stdout / stderr 로 뿌려주기`
  - 로그로 인한 호스트 메모리가 꽉 차는 것을 방지
- 컨테이너 헬스체크와 트래픽 유무 확인
  - `liveness`  
    fail시 컨테이너가 비정상으로 인지하고 kill
  - `readness`  
    ok 하면 컨테이너가 준비된 것으로 인지하고 traffic 전달
- 플랫폼에서 오는 `이벤트에 반응하여 준수`하는 방법
  - **SIGTERM** 받았을 때 task 마무리 후 종료처리  
    무중단 배포를 위한 필수 사항
  - SIGTERM 이후 SIGKILL을 통해 죽이는데,  
    SIGTERM에 대한 처리가 없으면 작업 중인 일을 냅두고 죽음
- `컨테이너 1개에는 가급적 단일 목적 1개 프로세스`
  - 성능 병목이나 이슈 파악의 용이
- `모든 배포 환경에 사용되는 이미지는 동일`
  - 개발 환경과 실행 환경의 동일성
  - 동적인 정보는 config, secret, storage로 전달
- 적절한 `resource request/limit` 설정
  - 서버 부하가 생길 경우 미설정 시 후순위로 밀려 문제 발생
- 컨테이너는 임시적이고 대체될 준비가 되어야 함
  - 개별 컨테이너에 상태 저장 지양
  - 컨테이너는 언제든 재시작 가능

# 서비스 설계하기

## SLA

- 서비스 수준 정하기
  - 가용성 관리
  - 용량 관리
  - 연속성 관리
  - 보안성 관리

## 가용성

- Master Node의 갯수
  - etcd 등을 외부 분리해 설치
- 노드 컨디션에 대해 상세한 확인 진행
- Pod의 가용성 관리
  - liveness / readiness 프로브
