---
title: "젠킨스 연결과 CD"
excerpt: "빌드한 이미지에 젠킨스 연결을 학습하고,
CD 툴을 사용해 파이프라인을 형성한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-21
last_modified_at: 2023-02-21
---

```docker
sudo -i
#인스턴스 생성 후, 관리자권한 획득하여 접속

cat jenkins.sh

#!/bin/bash
apt-get update -y
apt-get install -y openjdk-11-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FCEF32E745F2C3D5
sudo apt-get -y update
sudo apt-get -y install jenkins
sed -i s/HTTP_PORT=8080/HTTP_PORT=7777/g /etc/default/jenkins
sed -i s/JENKINS_PORT=8080/JENKINS_PORT=7777/g /usr/lib/systemd/system/jenkins.service
systemctl daemon-reload
systemctl restart jenkins
systemctl enable jenkins
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

#vi 편집기로 열어서 위 내용을 넣어준다.
#실행 권한을 주고 젠킨스 설치 스크립트를 실행

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
#도커 설치

<플러그인 설치>

<크리덴셜>

# docker_cre, git_cre 생성.
#docker_cre
#git_cre
#credential이 3개 존재.

sudo usermod -aG docker jenkins
systemctl restart jenkins
#젠킨스가 도커를 실행할 수 있도록 권한 부여.

<AWS CLI 설치>
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
# cli 환경 설치를 위한 압축파일 다운
apt-get -y install unzip
# 압축 파일 풀기위한 패키지 설치
unzip awscliv2.zip
# 다운받은 압축파일을 풀고
sudo ./aws/install
# 설치

# aws라는 명령어가 생김.

<aws에 cli로 로그인을 하기위한 액세스키(계정) 생성>

#클릭

<EKS 클러스터 구성을 위한 eksctl 명령어 설치>

curl --silent --location \
"https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
#설치파일 다운로드

sudo mv /tmp/eksctl /usr/local/bin
# 명령어를 내 서버로 이동
eksctl version
# 버전 확인

<kubectl 명령어 설치>

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
# 명령어 다운
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
# 설치
kubectl version --client



<eksctl 을 통한 EKS 클러스터 구성>

eksctl create cluster --vpc-public-subnets <퍼블릭서브넷ID 1>,<퍼블릭서브넷ID 2> --name <eks 클러스터 이름> --region ap-northeast-2 --version 1.24 --nodegroup-name <eks 노드그룹 이름> --node-type t2.small --nodes 2 --nodes-min 2 --nodes-max 5

eks클러스터 구성하는데 시간당 0.1달러 + EC2 인스턴스의 사용요금은 별도 + 기타 등등..

<퍼블릭서브넷ID 1>,<퍼블릭서브넷ID 2>
: 여러분들의 VPC에 존재하는 퍼블릭 서브넷의 ID, 반드시 jenkins 인스턴스와 같은 vpc에 존재해야함.

<eks 클러스터 이름> , <eks 노드그룹 이름>
: 임의로 정하는 값, 중복되면 안됨.
<eks 클러스터 이름> = eks-cluster
<eks 노드그룹 이름> = eks-nodegroup


# cloud formation 인프라 프로비저닝 서비스, 만약에 클러스터를 다시 설치할때 안되는경우 cloud formation에 가서 찌꺼기를 삭제해준다.

docker image pull oolralra/hnginx
docker image pull oolralra/ipnginx
#실습을위해 이미지를 pull 해온다

docker tag oolralra/ipnginx <여러분들의 도커허브계정>/ipnginx
docker tag oolralra/hnginx <여러분들의 도커허브계정>/hnginx
#받은 이미지를 새로 태깅

#도커 허브에 로그인

docker push <여러분들의 도커허브계정>/ipnginx
docker push <여러분들의 도커허브계정>/hnginx
#여러분들의 도커허브에 push

—------
eks 클러스터 구성이 잘 됐다면 아래 명령어로 확인

kubectl get pod -n kube-system

<간단한 deployment 배포>

apiVersion: apps/v1
kind: Deployment
metadata:
  name: h-deploy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hnginx
  template:
    metadata:
      labels:
        app: hnginx
    spec:
      containers:
        - name: hnginx
          image: <본인의 도커허브 계정>/hnginx

kubectl apply -f h-deploy.yml
kubectl get pod -o wide

—-------------------
디플로이먼트를 외부로 서비스하기위한 service 매니페스트 파일 정의

k8s# vi h-svc.yml


apiVersion: v1
kind: Service
metadata:
  name: np-rep-svc
spec:
  selector:
    app: hnginx
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80

<서비스타입을 변경하는 방법>
1.svc매니페스트 파일을 직접 변경 후 apply를 다시 해주는 방법
2.kubectl patch 명령을 쓰는방법

kubectl patch svc np-rep-svc -p '{"spec": {"type": "LoadBalancer"}}'
# svc앞에 로드밸런서를 붙여준다.

# 웹브라우저에서 DNS이름으로 접속을 하거나
# curl 명령으로 로드밸런싱이 잘 되는것을 확인할 수 있다.

kubectl delete -f h-svc.yml -f h-deploy.yml
#서비스 및 deploy 삭제

# 서비스를 삭제하면 로드밸런서도 같이 삭제가 된다.


<argoCD : k8s의 배포툴>
- 깃허브 레포지토리에 있는 매니페스트파일을 불러와서 자동으로 배포. 시각화 및 롤백 같은 기능을 제공한다.

1.깃허브에서 배포할 매니페스트파일 업로드
# 파일을 생성하면서 / 를 하면 폴더가 생성된다.

<argoCD 설치>

kubectl create namespace argocd
#argoCD를 설치하기 위한 네임스페이스 생성

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# argocd 설치

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
# argocd의 서비스 타입을 로드밸런서로 구성

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
# 초기비밀번호 조회

#패스워드 변경

#깃허브의 레포지토리를 등록

#해당 레포지토리를 기반으로 하는 앱 생성

##SYNC POLICY를 Automatic으로 해야 자동으로 깃허브 레포지토리의 변경된 점을 자동으로 인식

#깃허브 레포지토리의 deploy라는 폴더에 매니페스트 파일 존재 필요


- 각 오브젝트들 및 상태를 시각적으로 볼 수 있다
- 서비스를 클릭해서 들어가면 외부 접속주소를 확인 가능

#deployment의 이미지 변경하여 자동으로 배포가 되는지 확인

# 서비스 포트 변경
# argocd가 수분내에 이를 인지하여 배포

---
        git credentialsId: githubCredential,
            url: 'https://github.com/<본인레포지토리주소>/sb_code.git',
            branch: 'main'

        // 이미지 태그 변경 후 메인 브랜치에 푸시
        sh "git config --global user.email ${gitEmail}"
        sh "git config --global user.name ${gitName}"
        sh "sed -i 's/sbimage:.*/sbimage:${currentBuild.number}/g' deploy/sb-deploy.yml"
        sh "git add ."
        sh "git commit -m 'fix:${dockerHubRegistry} ${currentBuild.number} image versioning'"
        sh "git branch -M main"
        sh "git remote remove origin"
        sh "git remote add origin git@github.com:<본인레포지토리 주소>/sb_code.git"
        sh "git push -u origin main"

---

-초기, 깃허브에 ssh로 push할 수 있는 권한이 없음
# ssh로 push를 하기위해서는 키페어를 생성하고 깃허브에는 퍼블릭키,젠킨스 서버에는 프라이빗키를 등록

#젠킨스 사용자로 접속

cd /var/lib/jenkins/.ssh/
ls

#젠킨스에 등록할 프라이빗키를 복사해서 manage credential

#깃허브로 이동하여 퍼블릭키(id_rsa.pub)의 내용을 복붙

#정상적으로 등록된 퍼블릭키

# 신뢰할 수 있는 사이트 목록에 깃허브가 존재하지 않음.

ssh -vT git@github.com
# yes를 입력

# 신뢰할 수 있는 사이트에 등록

#에러메세지가 사라짐. = ssh로 push가 가능

#known_hosts 라는 파일이 생성
#재빌드

- argoCD상에서도 pod의 이미지가 업데이트 된것을 확인 가능하다
eksctl delete cluster --name eks-cluster

```
