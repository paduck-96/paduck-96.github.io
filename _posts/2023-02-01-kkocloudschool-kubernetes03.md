---
title: "Kubernetes  pod 관리"
excerpt: "Kuberentes pod 관리와 모니터링에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-01
last_modified_at: 2023-02-01

---
```docker
pod의 label은 dev: my-label이며 image는 rapa1.iptime.org:5000/ipnginx를 사용하여.
----
root@master-pcm~# : 
cat replicaset-nginx.yaml 

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myrs
spec:
  replicas: 3
  selector:
    matchLabels:
      dev: my-label
  template:
    metadata:
      name: mypod
      labels:
        dev: my-label
    spec:
      containers:
      - name: mycon
        image: nginx:latest
        ports:
        - containerPort: 80
—------------------------------------------------

# 파드를 실시간 모니터링
kubectl get pod -o wide --watch

# 파드를 삭제하면 레플리카 매니페스트 파일이 정의되어있는 상태로 원상복구를 하기위해 새로운 파드를 한개 더 생성

—-----------------------------
<Deployment>
root@master:~# 
cat deploy-nginx.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deploymnet
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx
    spec:
      containers:
      - name: nginx
        image: rapa1.iptime.org:5000/hnginx
        ports:
        - containerPort: 80

#디플로이먼트 생성.
kubectl apply -f deploy-nginx.yaml

#생성한 디플로이먼트 확인
kubectl get pod -o wide

#생성안되는경우 위와 같이 치시면 됨
#잘 동작하는것을 확인

#파드의 호스트네임을 출력하는 nginx 이미지.
vi deploy-nginx.yaml 

# image의 hnginx를 ipnginx로 변경

# 현재 사설저장소 IP가 192.168.0.195 => 192.168.0.98로 변경

vi /etc/docker/daemon.json

#이미지를 변경후 apply

예제) 아까 띄워놨던 레플리카셋인 myrs의 image를 rapa1.iptime.org:5000/ipnginx로, 레플리카수는 2개로 변경하여 해당 레플리카에 속해있는 pod의 ip를 curl로 출력하여 어떤 변화가 있는지 확인해보세요. 또한 레플리카수가 변경되는지 여부도 확인해보세요.

#변경 전 상태
#nginx의 기본페이지

#매니페스트 파일을 불러와서
vi replicaset-nginx.yaml

#레플리카수를 2로, 이미지를 rapa1.iptime.org:5000/ipnginx로 수정
#레플리카의 수는 2개로 변경
#이미지(app)은 그대로. 레플리카셋으로는 이미지 업데이트가 불가능함.

—-----------------------------------------
# <네임스페이스>
##격리된 환경을 제공하는 오브젝트
kubectl get namespace

#blue-ns라는 네임스페이스를 매니페스트 형태로 저장.
kubectl create namespace blue-ns --dry-run=client -o yaml >blue-ns.yaml

#매니페스트 파일이 생성된것을 확인 가능
vi blue-ns.yaml 로 아래와 같이 편집.

예제) blue-ns라는 네임스페이스에 image는 rapa1.iptime.org:5000/hnginx로 하고, 레플리카는 2개인 deployment를 만들어보세요. 

deployment 이름 : blue-dep
replicaset 이름 : blue-rs
레이블 : app: blue-label

#파일복사하는법
cp deploy-nginx.yaml deply-blue.yaml

—------------------
답)

cat deply-blue.yaml 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: blue-dep
spec:
  replicas: 2
  selector:
    matchLabels:
      app: blue-label
  template:
    metadata:
      name: blue-pod
      labels:
        app: blue-label
    spec:
      containers:
      - name: nginx
        image: rapa1.iptime.org:5000/hnginx
        ports:
        - containerPort: 80

—-----------------------

# 1. 매니페스트 파일을 apply 할때 네임스페이스를 지정.
kubectl apply -f deply-blue.yaml -n blue-ns

# blue-ns에 생성된 파드 확인

# 2.  vi deply-blue.yaml 후 metadata 밑에 namespace를 추가하는 방법

#네임스페이스 삭제시 모든 오브젝트 및 컨트롤러가 삭제
#네임스페이스와 디플로이먼트 동시 생성.
#여러개의 매니페스트 파일을 하나의 파일로 합칠 수 있음

예제) exam이라는 네임스페이스에 rapa1.iptime.org:5000/ipnginx를 이미지로 하는 pod와 rapa1.iptime.org:5000/hnginx를 이미지로 하는 두개의 replicaset, rapa1.iptime.org:5000/nginx를 이미지로 하는 deployment를 하나의 매니페스트 파일로 만들어 apply 해보세요!

답)

cat exam.yaml 
apiVersion: v1
kind: Namespace
metadata:
  name: exam
---
apiVersion: v1
kind: Pod
metadata:
  name: my-nginx-pod
  namespace: exam
spec:
  containers:
  - name: my-nginx-ctn
    image: rapa1.iptime.org:5000/ipnginx
    ports:
    - containerPort: 80
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myrs
  namespace: exam
spec:
  replicas: 2
  selector:
    matchLabels:
      dev: my-label
  template:
    metadata:
      name: mypod
      labels:
        dev: my-label
    spec:
      containers:
      - name: mycon
        image: rapa1.iptime.org:5000/hnginx
        ports:
        - containerPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deploymnet
  namespace: exam
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      name: my-nginx-pod
      labels:
        app: my-nginx
    spec:
      containers:
      - name: nginx
        image: rapa1.iptime.org:5000/nginx
        ports:
        - containerPort: 80

—---------
kubectl apply -f exam.yaml
```