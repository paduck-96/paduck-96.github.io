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

```docker
# PV를 위한 nfs 서버(공유폴더) 설치
mkdir /shared
# /shared 폴더를 누구나 사용 가능하도록 권한 변경
chmod 777 /shared

#nfs서버 설치
apt-get -y install nfs-server

# nfs설정파일을 열어서 다음의 내용을 추가
vi /etc/exports

/shared         *(rw,no_root_squash,sync)

# 설정을 적용시키기 위해 해당 데몬(프로그램) 재시작
systemctl restart nfs-server

#마스터노드에 각 워커노드들 및 파드들이 접근가능한 공유폴더를 생성.
#각 워커노드에서는
apt-get -y install nfs-common

# 공유폴더에 접근 가능하도록 nfs-common 폴더를 설치.
cat pod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: nfs-pod
spec:
  containers:
  - name: nfs-mount-container
    image: nginx
    volumeMounts:
    - name: nfs-volume
      mountPath: /mnt
  volumes:
  - name: nfs-volume
    nfs:
      path: /shared
      server: 192.168.0.210 #자신의 마스터노드의 IP를 입력.
#서로 동기화가 되어있다고 봐도 무방.

# 서버에 파일을 생성, 생성된 파일은 pod에서도 볼 수 있음
touch /shared/test.txt

#test.txt이 보이는 것을 확인가능.
#반대로도 가능(pod에서 파일 생성 => 마스터노드에서 확인)

# PV생성
cat pv1.yaml

apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain #유지
  nfs:
    path: /shared/pv1
    server: 192.168.0.210 #본인 마스터노드
    #(/shared폴더가 있는 서버)의 주소
    readOnly: false

cat pvc-pod.yaml

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-nfs-pvc
spec:
  storageClassName: ""
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 1Gi
---
apiVersion: v1
kind: Pod
metadata:
  name: pvc-pod
spec:
  containers:
  - name: nfs-mount-container
    image: nginx
    volumeMounts:
    - name: nfs-volume
      mountPath: /mnt
  volumes:
  - name: nfs-volume
    persistentVolumeClaim:
      claimName: my-nfs-pvc

# pvc및 해당 pvc를 사용할 pod 생성
kubectl apply -f pvc-pod.yaml

# pod,pvc를 삭제하면 데이터는 /shared/pv1에 유지되고 pv는 더이상 사용할 수 없는 상태가 된다.

예제) exam-pvc라는 pvc를 생성하고 이 pvc를 사용하는 exam-pod를 생성하여 파드내에 /remote라는 공간을 생성하여 해당 공간을 pv에 영구적으로 보관해보세요.
pod내에 kubectl exec exam-pod -- touch /remote/pv.test 를 통해 pod내에 파일을 생성후 해당 데이터가 마스터노드내에 잘 저장되는지 여부도 확인해보세요.

# cp pv1.yaml pv2.yaml
# cp pvc-pod.yaml pvc-exam.yaml
vi pv2.yaml
##nfs:
    ##path: 실 경로 입력

vi pvc-exam.yaml
##volumes:
    ##claimName: 위에서 실제 생성한 PVC 입력

kubectl apply -f pvc-exam.yaml

# cat mysql.yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress
    tier: mysql
  clusterIP: None
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: mysql
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: wordpress
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-pass
              key: password
        ports:
        - containerPort: 3306
          name: mysql
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
      volumes:
      - name: mysql-persistent-storage
        persistentVolumeClaim:
          claimName: mysql-pv-claim


# cat wp.yaml
apiVersion: v1
kind: Service
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  ports:
    - port: 80
  selector:
    app: wordpress
    tier: frontend
  type: LoadBalancer
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: wp-pv-claim
  labels:
    app: wordpress
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: frontend
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: wordpress
        tier: frontend
    spec:
      containers:
      - image: wordpress:4.8-apache
        name: wordpress
        env:
        - name: WORDPRESS_DB_HOST
          value: wordpress-mysql
        - name: WORDPRESS_DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-pass
              key: password
        ports:
        - containerPort: 80
          name: wordpress
        volumeMounts:
        - name: wordpress-persistent-storage
          mountPath: /var/www/html
      volumes:
      - name: wordpress-persistent-storage
        persistentVolumeClaim:
          claimName: wp-pv-claim


—--------------------------------
# cat pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv1
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain #유지
  nfs:
    path: /shared/pv1
    server: 192.168.0.210 #본인 마스터노드(/shared폴더가 있는 서버)의 주소
    readOnly: false
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv2
spec:
  capacity:
    storage: 1Gi
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain #유지
  nfs:
    path: /shared/pv2
    server: 192.168.0.210 #본인 마스터노드(/shared폴더가 있는 서버)의 주소
    readOnly: false


—-------------
# 두개의 yaml파일에서 위의 내용을 수정.
cat <<EOF >./kustomization.yaml
secretGenerator:
- name: mysql-pass
  literals:
  - password=YOUR_PASSWORD
EOF


cat <<EOF >>./kustomization.yaml
resources:
  - mysql.yaml
  - wp.yaml
EOF

# 커스터마이징 파일에 정의되어 있는 두개의 매니페스트파일을 apply
kubectl apply -k .
#각각 mysql 파드와 wordpress 파드의 내용들이 정상적으로 마운트 된것을 확인

```
