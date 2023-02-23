---
title: "쿠버네티스에서 was 와 db 연동"
excerpt: "쿠버네티스를 통해 was 와 db를 연동하는 방법에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-22
last_modified_at: 2023-02-22
---
```docker

<k8s에서 WAS 와 DB 연동>
- DB deployment의 svc를 통해 WAS에서 찾아갈 수 있도록 간단한 테스트파일을 통해 구성

cat Dockerfile 

    FROM mariadb
    ENV MYSQL_ROOT_PASSWORD=test123
    ENV MYSQL_DATABASE=db
    ENV MYSQL_USER=admin
    ENV MYSQL_PASSWORD=test123

docker build -t sql:1.0 .
# 이미지 빌드
docker run -d --name sql sql:1.0
# 컨테이너 실행 (잘 동작하는지 테스트)
docker exec -it sql /bin/bash
#컨테이너 내부에 진입

mysql -u admin -ptest123
# 데이터베이스 서버로 진입 사용자 admin 암호 test123

docker tag sql:1.0 <도커허브계정>/sql:1.0
# 도커허브에 업로드하기위한 tag 생성

docker login
docker push <이미지 태그>

DB이름 : db
DB사용자명 : admin
DB사용자암호 : test123

#tom 컨테이너의 Dockerfile
cat Dockerfile 

    FROM tomcat
    ADD mysql-connector-java-8.0.23.jar /usr/local/tomcat/lib
    ADD dbtest.jsp /usr/local/tomcat/webapps/ROOT/dbtest.jsp
    ENTRYPOINT ["catalina.sh","run"]
-----
<Dockerfile에 ADD한 파일1 >

mysql-connector-java-8.0.23.jar
# DB와 연동하기 위한 커넥터

https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.23
- 위 링크에서 jar 파일을 Dockerfile이 있는곳에서 wget
wget https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.23/mysql-connector-java-8.0.23.jar
#jdbc 드라이버 다운로드

—-------------------------------------------

<Dockerfile에 ADD한 파일2 >
dbtest.jsp
#DB와의 연동을 테스트 하기 위한 파일, 

    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
    <%@ page import="java.sql.*"%>
    <h1>DB</h2>
    <%
            Connection conn=null;
            try{
                    String Url="jdbc:mysql://<k8s의 db deployment의 svc명>/<DB이름>";
                    String Id="<DB사용자>";
                    String Pass="<DB사용자암호>";

                    Class.forName("com.mysql.jdbc.Driver");
                    conn=DriverManager.getConnection(Url,Id,Pass);
                    out.println("was-db Connection Success!");
            }catch(Exception e) {
                    e.printStackTrace(); 
    }
    %>
#3개의 파일이 존재, Dockerfile, dbtest.jsp,  jdbc라이브러리 파일


docker build -t <도커허브계정>/tom:1.0 .
docker push <이미지 이름>

docker run -d --name sql-svc oolralra/sql:1.0
#DB컨테이너 생성
docker run -dp 7878:8080 --name tom --link sql-svc oolralra/tom:1.0
#tomcat 컨테이너 생성
# curl localhost:7878/dbtest.jsp 를 쳤을때 
# 밑에 ‘was-db connection success가뜨면 was 와 db가 연동이 잘 된것이다.
#DB컨테이너를 삭제후에 다시 접속해보면( DB연동이 제대로 안되면) 그냥 DB라는 문구만 뜬다.
—---------------------------------------------------------------------------

<k8s 에서 svc로 연동>

vi tom-deploy.yml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: tom-deploy
    spec:
    replicas: 2
    selector:
        matchLabels:
        app: mytom
    template:
        metadata:
        labels:
            app: mytom
        spec:
        containers:
            - image: <도커허브계정>/tom:1.0
            name: mytom
------
    apiVersion: v1
    kind: Service
    metadata:
    name: tom-svc
    spec:
    ports:
        - port: 80
        protocol: TCP
        targetPort: 8080
    selector:
        app: mytom
    type: LoadBalancer

—------------------------------------
vi sql-deploy.yml


    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: sql-deploy
    spec:
    replicas: 1
    selector:
        matchLabels:
        app: mysql
    template:
        metadata:
        labels:
            app: mysql
        spec:
        containers:
            - image: <도커허브 계정>/sql:1.0
            name: mysql
---
    apiVersion: v1
    kind: Service
    metadata:
    name: sql-svc
    spec:
    ports:
        - port: 3306
        protocol: TCP
        targetPort: 3306
    selector:
        app: mysql
--------

vi /etc/netplan/탭

netplan apply

hostnamectl set-hostname master-1

sudo swapoff /swap.img
sudo sed -i -e '/swap.img/d' /etc/fstab
 
sudo groupadd docker
sudo gpasswd -a $USER docker
sudo usermod -aG docker $USER
sudo newgrp docker
 
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
 
sudo systemctl enable --now docker && sudo systemctl status docker --no-pager
  
설치
 
git clone https://github.com/Mirantis/cri-dockerd.git
 
wget https://storage.googleapis.com/golang/getgo/installer_linux
chmod +x ./installer_linux
./installer_linux
source ~/.bash_profile
cd cri-dockerd
mkdir bin
go build -o bin/cri-dockerd
mkdir -p /usr/local/bin
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
cp -a packaging/systemd/* /etc/systemd/system
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
systemctl daemon-reload
systemctl enable cri-docker.service
systemctl enable --now cri-docker.socket
sudo systemctl restart docker && sudo systemctl restart cri-docker
sudo systemctl status cri-docker.socket --no-pager
 
 
# Docker cgroup Change Require to Systemd
 
cat <<EOF | sudo tee /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
	"max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

sudo systemctl restart docker && sudo systemctl restart cri-docker

sudo docker info | grep Cgroup
 
# Kernel Forwarding , kube-proxy(= 파드의 통신, 오버레이 네트워크를 담당) 설정.

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
 
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
 
sudo apt-get update
 
sudo apt-get install -y apt-transport-https ca-certificates curl unzip
 
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
 
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
 
# Update 해야 패키지 인식함
sudo apt-get update
 
# k8s 설치
sudo apt-get install -y kubelet kubeadm kubectl
 
# 버전 확인하기
kubectl version --short
 
# 버전 고정하기
sudo apt-mark hold kubelet kubeadm kubectl
 
<<<마스터만>>>
sudo kubeadm config images pull --cri-socket unix:///run/cri-dockerd.sock
 
sudo kubeadm init --pod-network-cidr=10.10.0.0/16 --cri-socket /var/run/cri-dockerd.sock

mkdir -p $HOME/.kube

sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

sudo chown $(id -u):$(id -g) $HOME/.kube/config
 
<<<<워커 노드만>>>>
kubeadm join <마스터노드 주소>:6443 --token rjb3jo.pimbre1cedmts6jz \
--discovery-token-ca-cert-hash sha256:3a671267bf18f46cd9c74d024853296ac6576fc644302485c2d02a4bfc7a9c2d --cri-socket /var/run/cri-dockerd.sock
마스터노드에 뜬 본인껄로 하세요!!
 

<마스터 노드..>

CNI(Container Network Interface) - calico
 
curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/calico.yaml -O
 
sed -i -e 's?192.168.0.0/16?10.10.0.0/16?g' calico.yaml
 
kubectl apply -f calico.yaml
 
#쿠버네티스는 자동완성
#쿠버네티스 사이트에서 'bash completion' 검색
 
apt-get install bash-completion ## 없으면 설치하는데 원래 되어있음.
 
source /usr/share/bash-completion/bash_completion
 
echo 'source <(kubectl completion bash)' >>~/.bashrc
echo 'source <(kubeadm completion bash)' >>~/.bashrc
 
kubectl completion bash >/etc/bash_completion.d/kubectl
 
## 이제 kubectl get nodes --all-n 하고 탭치면 자동완성.

사설저장소 구축.

사설 저장소 주소 : 150


vi /etc/docker/daemon.json

"insecure-registries": ["192.168.2.1:5000"]
```