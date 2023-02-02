---
title: "Kubernetes 로드밸런싱"
excerpt: "로컬 클러스터에는 Kubernetes 로드밸런싱 구현이 필요하여 방법에 대해 학습한다."

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-02
last_modified_at: 2023-02-02

---

```docker
cat deploy.yaml

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
          image: rapa1.iptime.org:5000/hnginx

—--------------------------------

cat svc.yaml

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
      nodePort: 32767
      port: 80
      targetPort: 80
  type: NodePort

kubectl apply -f deploy.yaml -f svc.yaml

# 웹브라우저에서 <워커노드의 주소>:<노드포트> 로 접속
# 외부로 배포 중

# 브라우저에서는 로드밸런싱 여부가 잘 확인이 되지 않으므로 curl 명령으로 각 파드에 순차접속되는것을 확인.

##예제) ipnginx 이미지로 ip-deploy라는 파드를 레플리카수 3개로 생성. 레이블은 test: svc
##ip-svc라는 이름을 갖는 Service를 통해 ip-deploy라는 Deployment를 30100 노드포트로 배포

# ip-deploy.yaml
cat ip-deploy.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: ip-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      test: svc
  template:
    metadata:
      labels:
        test: svc
    spec:
      containers:
        - name: ipnginx
          image: rapa1.iptime.org:5000/ipnginx

cat ip-svc.yaml

apiVersion: v1
kind: Service
metadata:
  name: ip-svc
spec:
  selector:
    test: svc
  ports:
    - name: http
      protocol: TCP
      nodePort: 30100
      port: 80
      targetPort: 80
  type: NodePort

# 로드밸런싱 여부를 확인.

—------------------
<ingress>
# MSA 구조를 구현할 수 있는 컨트롤러
#가령 rapa.com:30100 으로 접속했을때는 메인페이지로
#rapa.com:30100/signup 으로 접속시엔 회원가입 페이지로
#rapa.com:30100/board 로 접속시엔 게시판으로 경로설정(라우팅) 가능

# 구글에서 ingress k8s로 검색 후 첫번째 페이지
#인그레스 컨트롤러 링크 클릭
#인그리스 컨트롤러중 nginx 선택
#해당 주소로 접속하여 주소만 복사하여 서버에서 wget 으로 다운로드
https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.5.1/deploy/static/provider/baremetal/deploy.yaml

vi deploy.yaml
# deploy.yaml 파일에 노드포트를 추가

# 동작중인 ingress-controller를 확인

#서비스와 디플로이먼트 파일을 하나로 합친후 ingress 폴더에 복사

~/test/ingress# cat ingress.yaml

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress
  namespace: ingress-nginx
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: rapa.com
      http:
        paths:
          - path: /h
            pathType: Prefix
            backend:
              service:
                name: h-svc
                port:
                  number: 80
          - path: /ip
            pathType: Prefix
            backend:
              service:
                name: ip-svc
                port:
                  number: 80

# kubectl get ingress로 해당 인그리스 규칙이 설정된 노드의 아이피를 확인
#설정이 잘 되어야만 아이피가 생성

# vi /etc/hosts 폴더에 rapa.com이라는 영문주소에 대한 아이피를 설정
#본인의 노드 아이피를 넣어야함.

예제)
test.com:30500/hnginx 접속시 hnginx로 접속.
test.com:30500/ipnginx 접속시 ipnginx로 접속

되도록 ingress를 구성해보세요!


# cat metal.yaml
apiVersion: v1
kind: Namespace
metadata:
  labels:
    app: metallb
  name: metallb-system
---
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    app: metallb
  name: controller
  namespace: metallb-system
---
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    app: metallb
  name: speaker
  namespace: metallb-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    app: metallb
  name: metallb-system:controller
rules:
- apiGroups:
  - ''
  resources:
  - services
  verbs:
  - get
  - list
  - watch
  - update
- apiGroups:
  - ''
  resources:
  - services/status
  verbs:
  - update
- apiGroups:
  - ''
  resources:
  - events
  verbs:
  - create
  - patch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    app: metallb
  name: metallb-system:speaker
rules:
- apiGroups:
  - ''
  resources:
  - services
  - endpoints
  - nodes
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ''
  resources:
  - events
  verbs:
  - create
  - patch
- apiGroups:
  - extensions
  resourceNames:
  - speaker
  resources:
  - podsecuritypolicies
  verbs:
  - use
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  labels:
    app: metallb
  name: config-watcher
  namespace: metallb-system
rules:
- apiGroups:
  - ''
  resources:
  - configmaps
  verbs:
  - get
  - list
  - watch
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  labels:
    app: metallb
  name: metallb-system:controller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: metallb-system:controller
subjects:
- kind: ServiceAccount
  name: controller
  namespace: metallb-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  labels:
    app: metallb
  name: metallb-system:speaker
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: metallb-system:speaker
subjects:
- kind: ServiceAccount
  name: speaker
  namespace: metallb-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  labels:
    app: metallb
  name: config-watcher
  namespace: metallb-system
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: config-watcher
subjects:
- kind: ServiceAccount
  name: controller
- kind: ServiceAccount
  name: speaker
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  labels:
    app: metallb
    component: speaker
  name: speaker
  namespace: metallb-system
spec:
  selector:
    matchLabels:
      app: metallb
      component: speaker
  template:
    metadata:
      annotations:
        prometheus.io/port: '7472'
        prometheus.io/scrape: 'true'
      labels:
        app: metallb
        component: speaker
    spec:
      containers:
      - args:
        - --port=7472
        - --config=config
        env:
        - name: METALLB_NODE_NAME
          valueFrom:
            fieldRef:
              fieldPath: spec.nodeName
        - name: METALLB_HOST
          valueFrom:
            fieldRef:
              fieldPath: status.hostIP
        image: metallb/speaker:v0.8.2
        imagePullPolicy: IfNotPresent
        name: speaker
        ports:
        - containerPort: 7472
          name: monitoring
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            add:
            - NET_ADMIN
            - NET_RAW
            - SYS_ADMIN
            drop:
            - ALL
          readOnlyRootFilesystem: true
      hostNetwork: true
      nodeSelector:
        beta.kubernetes.io/os: linux
      serviceAccountName: speaker
      terminationGracePeriodSeconds: 0
      tolerations:
      - effect: NoSchedule
        key: node-role.kubernetes.io/master
---
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: metallb
    component: controller
  name: controller
  namespace: metallb-system
spec:
  revisionHistoryLimit: 3
  selector:
    matchLabels:
      app: metallb
      component: controller
  template:
    metadata:
      annotations:
        prometheus.io/port: '7472'
        prometheus.io/scrape: 'true'
      labels:
        app: metallb
        component: controller
    spec:
      containers:
      - args:
        - --port=7472
        - --config=config
        image: metallb/controller:v0.8.2
        imagePullPolicy: IfNotPresent
        name: controller
        ports:
        - containerPort: 7472
          name: monitoring
        resources:
          limits:
            cpu: 100m
            memory: 100Mi
        securityContext:
          allowPrivilegeEscalation: false
          capabilities:
            drop:
            - all
          readOnlyRootFilesystem: true
      nodeSelector:
        beta.kubernetes.io/os: linux
      securityContext:
        runAsNonRoot: true
        runAsUser: 65534
      serviceAccountName: controller
      terminationGracePeriodSeconds: 0

# metalcm.yaml
#환경변수 생성
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
	address-pools:
  	- name: nginx-ip-range
    	protocol: layer2
    	addresses:
      	- 192.168.243.10-192.168.243.99
# configmap을 통해 로드밸런서가 받아올 IP 범위 지정.

vi h-deploy.yaml
#type을 LoadBalancer로 지정

# HPA
##Horizontal Pod Auto-scaling
##원하는 시간과 상황에 파드 조절

wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

vi components.yaml
...-args:
    - --kublet-insecure-tls

kubectl edit deploy h-deploy
resoureces:
    requests:
        cpu: "10m"
    limit:
        cpu: "50m"

kubectl autoscale deployment h-deploy --min=1 --max=5 --cpu-percent=50
# h-deploy라는 디플로먼트를 오토스케일링 설정 할건데, 최소 파드는 1개, 최대 파드는 5개로 설정. cpu 사용량이 50% 초과하면 파드를 늘리겠다.

i=0;while true;do echo $((i++)) `curl --silent 192.168.210.11`;sleep 1;done
#부하


```
