---
title: "Kubernetes 실습? - 카카오 특강"
excerpt: "어떻게 Kubernets를 사용해 배포와 CI/CD를 진행할 수 있는지 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-13
last_modified_at: 2023-02-13
---
# 1. Dockerfile 생성
## Dockerfile 생성하기
```docker
FROM # 사용할 부모 이미지
COPY [복사할 파일] /usr/share/[복사할 컨테이너 위치] # 파일 복사 
```
- 해당 과정을 통해 부모 이미지에 사용자 정의 파일 추가
## 이를 통해 Build
```docker
docker build -t [태그할 이미지 이름]
```
## 실행
```docker
docker run -d -p 컨테이너포트/호스트포트 --name [컨테이너 이름] [이미지 이름]
```
## 설정 포트로 실행 확인
## 종료 후 삭제
```docker
docker stop $(docker ps -aqf "name=컨테이너 이름") &&
docker rm $(docker ps -aqf "name=컨테이너 이름")
```

# 2. Docker Image 생성
# 3. Docker Hub 에 저장
## Dockerfile 를 통해 이미지 빌드
```docker
docker build -t Hub 사용자 이름/이미지 이름 .
```
## 이미지를 Hub 에 Push
```docker
docker push 이미지 이름
```
## docker run 명령으로 실행
## docker stop / rmi 로 중지 및 삭제

# 4. 저장된 Image 를 활용해 배포
## 배포 파일
- 배포의 단위인 **쿠버네티스 디플로이먼트**
```docker
kubectl create deployment 디플로이먼트 이름 --image=사용할 이미지
```
## 사용자 접근 파일
- 사용자가 접근할 수 있는 단위인 **쿠버네티스 서비스**
```docker
kubectl expose deployment 디플로이먼트 이름 --type=LoadBalancer --port=컨테이너 포트 번호 --target-port=호스트 포트 번호
```
## 서비스 정보 확인
```docker
kubectl get services
```
- 사용할 서비스의 EXTERNAL-IP 를 통해 IP로 접속 가능
## 배포 정보 변경
```docker
kubectl set image deployment/디플로이먼트 이름 기존 이미지 이름=새 이미지 이름
```
## 업데이트 과정 모니터링
```docker
kubectl rollout status deployment/디플로이먼트 이름
```
## 종료 및 삭제
```docker
kubectl delete deployment 디플로이먼트 이름 kubectl delete service 서비스 이름
```
# 5. CI / CD 파이프라인 구축
- 어떤 CI / CD 파이프라인을 사용하는 것은 개인의 자유
## Github Actions 워크 플로우
- 프로젝트 루트 레포지토리에 .github 디렉토리 생성
- .github > Workflow 디렉토리 생성
- 워크플로우를 정의하는 ci.yml 파일 생성

## 워크 플로우 정의하기
```docker
name : # 워크플로우 이름 작성
on: # 트리거할 이벤트 후술
    push:
        branches:
            - 브랜치 이름

jobs: # 워크플로우의 실행 작업 후술
    build:
        runs-on: ubuntu-latest # 구동 환경

        steps:
        - name: # 실행 단계 이름
           uses: actions/checkout@v2 # 사용할 액션
        
        - name: # 실행 단계 이름
           uses: docker/build-push-action@v2 # 사용할 액션
           with:
                context: . # 액션을 처리할 내용 // 현재 위치 기반
                push: true # docker 레지스트리로 push 함을 명시
                tags: # 사용자이름/repository:${{ env.TAG_NAME}} # docker 이미지 레포지토리의 이름과 태그 지정

    deploy:
        needs: build
        runs-on: ubuntu-latest # 구동 환경

        steps:
        - name: # 실행 단계 이름
           uses: aws-actions/configure-aws-credentials@v1 # 액션
           with:
                aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }} # github에 저장된 시크릿으로 설정
                aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }} # github에 저장된 시크릿으로 설정
                aws-region: us-west-2 # EKS 클러스터 위치한 AWS 리전

        - name: Deploy to EKS # 실행 단계 이름
           uses: aws-actions/eks-deploy@v1 # 액션
           with:
                cluster-name: my-eks-cluster # EKS 클러스터 이름
                namespace: default # 배포 생성하는 쿠버네티스 네임스페이스
                kubeconfig: ${{ secrets.KUBECONFIG }} # 쿠버네티스 구성 파일 경로
                images: | # 배포해야하는 docker 이미지, 태그
                    my-image:username/repository:${{ env.TAG_NAME }}

```
- 일련의 과정을 통해 변경 사항이 레포에 push 될 때마다,  
docker 이미지를 빌드하고 클러스터에 배포