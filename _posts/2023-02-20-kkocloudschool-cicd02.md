---
title: "젠킨스 연결"
excerpt: "빌드한 이미지에 젠킨스 연결을 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-20
last_modified_at: 2023-02-20
---
``` docker
<빌드하여 push>
1. 깃허브에서 소스코드 다운로드
2. 자바 빌드 => app.jar 파일 생성
3. 생성된 app.jar 파일을 삽입하여 도커이미지 빌드
4. 도커이미지를 저장소에 푸쉬

<젠킨스 설치>

<aws>
#관리자권한 획득

<공통>
apt-get update -y
#패키지 업데이트

apt-get install -y openjdk-11-jdk
#openjdk-11 설치

wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -
#키 설치

echo deb http://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list\
#레포리스트 업데이트

sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FCEF32E745F2C3D5
#키값 확인

sudo apt-get -y update
#변경된 레포지토리 업데이트

sudo apt-get -y install jenkins
#젠킨스 설치
##기본포트 = 8080

<포트를 7777번 포트로 변경>
sed -i s/HTTP_PORT=8080/HTTP_PORT=7777/g /etc/default/jenkins
# /etc/default/jenkins 라는 파일의 HTTP_PORT=8080을 HTTP_PORT=7777로 치환

sed -i s/JENKINS_PORT=8080/JENKINS_PORT=7777/g /usr/lib/systemd/system/jenkins.service
# /usr/lib/systemd/system/jenkins.service 라는 파일의 JENKINS_PORT=8080을 JENKINS_PORT=7777로 치환

systemctl daemon-reload
#변경된 프로그램 리로드
systemctl restart jenkins
#재시작
systemctl enable jenkins
#서버 재부팅 후에도 동작
##초기비번을 입력해서 unlock

cat /var/lib/jenkins/secrets/initialAdminPassword
#밑에 나오는 랜덤한 문자열값을 웹에 복붙

1.maven 빌드를 위한 플러그인 설치
# 젠킨스에서 설치 가능한 플러그인 선택 

<설치한 maven을 등록>
#앞으로 빌드를 하게되면 ‘my_maven’이라는 이름을 갖고있는 빌드툴로 빌드

#새로운 프로젝트 생성
https://github.com/<본인아이디>/<레포지토리주소>

#콘솔 아웃풋을 통해 뭐가 문제인지 파악 가능
#깃허브(SCM)의 Jenkinsfile이라는 파이프라인 절차를 통해 프로젝트를 빌드해야하는데 젠킨스 파일이 깃허브 레포지토리상에 없는게 문제.

pipeline {
  agent any
  // any, none, label, node, docker, dockerfile, kubernetes

  stages {
    stage('Example') {
      steps {
        echo 'Hello World'
        }
    }
  }
}


# 서버에서 자신의 레포지토리를 클론 해온 후 
# Jenkinfile을 만든다
#Jenkinsfile 업로드

pipeline {
  agent any
  // any, none, label, node, docker, dockerfile, kubernetes
  tools {
    maven 'my_maven'
  }
  environment {
    gitName = 'pcmin929'
    gitEmail = 'pcmin929@gmail.com'
    githubCredential = 'git_cre'
    dockerHubRegistry = 'oolralra/sbimage'
  }
  stages {
    stage('Checkout Github') {
      steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: githubCredential, url: 'https://github.com/pcmin929/sb_code.git']]])
          }
      post {
        failure {
          echo 'Repository clone failure'
        }
        success {
          echo 'Repository clone success'  
        }
      }
    }

    stage('Maven Build') {
      steps {
          sh 'mvn clean install'
          }
      post {
        failure {
          echo 'Maven jar build failure'
        }
        success {
          echo 'Maven jar build success'  
        }
      }
    }
    stage('Docker Image Build') {
      steps {
          sh "docker build -t ${dockerHubRegistry}:${currentBuild.number} ."
          sh "docker build -t ${dockerHubRegistry}:latest ."
          }
      post {
        failure {
          echo 'Docker image build failure'
        }
        success {
          echo 'Docker image build success'  
        }
      }
    }
  }
}

/var/lib/jenkins/workspace/simple_sb
#프로젝트 파일이 존재하는 디렉토리

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
#도커 이미지 빌드를 위해 도커를 설치

<젠킨스에서 도커 및 관련 명령어를 사용하기 위해 도커 플러그인을 설치>
#jenkins 사용자에게 docker를 동작시킬 권한이 없음.
#도커파일 생성

FROM openjdk:8-alpine
ADD target/springbootApp.jar springbootApp.jar
EXPOSE 8085
ENTRYPOINT ["java", "-jar", "springbootApp.jar"]
# -a : 사용자의 기본 소속그룹을 변경
# -G : 계정의 소속 그룹을 변경.

usermod -g <그룹> <계정>
- jenkins라는 사용자를 docker라는 그룹에 합류

# 아까 보안그룹을 만들때 7979번 포트를 미리 개방

<Docker Hub Push>
1.docker push를 위한 도커 허브 credential을 젠킨스에서 생성해준다.

stage('Docker Image Push') {
      steps {
          // 도커 허브의 크리덴셜
          withDockerRegistry(credentialsId: dockerHubRegistryCredential, url: '') {
          // withDockerRegistry : docker pipeline 플러그인 설치시 사용가능.
          // dockerHubRegistryCredential : environment에서 선언한 docker_cre
            sh "docker push ${dockerHubRegistry}:${currentBuild.number}"
            sh "docker push ${dockerHubRegistry}:latest"
          }  
      }
      post {
      // docker push가 성공하든 실패하든 로컬의 도커이미지는 삭제.
        failure {
          echo 'Docker Image Push failure'
          sh "docker rmi ${dockerHubRegistry}:${currentBuild.number}"
          sh "docker rmi ${dockerHubRegistry}:latest"
        }
        success {
          echo 'Docker Image Push success'
          sh "docker rmi ${dockerHubRegistry}:${currentBuild.number}"
          sh "docker rmi ${dockerHubRegistry}:latest"
        }
      }
    }

<AWS의 인스턴스가 먹통이 되는경우>
인스턴스를 중지
‘중지됨’ 상태가 되면 재시작
인스턴스는 중지가 되면 퍼블릭IP가 변경.
젠킨스에 접속하려면 변경된 IP의 7777번 포트로 접속.
빌드넘버 오른쪽의 x버튼을 눌러서 빌드를 abort.
#여기까지 끝났다면 도커허브에 접속하여 이미지가 push 됐는지를 확인

pipeline {
  agent any
  // any, none, label, node, docker, dockerfile, kubernetes
  tools {
    maven 'my_maven'
    // 플러그인 설치후 글로벌 설정에서 만든 빌드명.
  }
  environment {
    gitName = 'pcmin929'
    gitEmail = 'pcmin929@gmail.com'
    githubCredential = 'git_cre'
    dockerHubRegistry = 'oolralra/sbimage'
    dockerHubRegistryCredential = 'docker_cre'
  }
  stages {
    stage('Checkout Github') {
      steps {
          checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: githubCredential, url: 'https://github.com/pcmin929/sb_code.git']]])
          }
      post {
        failure {
          echo 'Repository clone failure'
        }
        success {
          echo 'Repository clone success'  
        }
      }
    }

    stage('Maven Build') {
      steps {
          sh 'mvn clean install'
          // maven integration , maven invoker 플러그인이 설치되어있어야함.
          }
      post {
        failure {
          echo 'Maven jar build failure'
        }
        success {
          echo 'Repository clone success'  
        }
      }
    }
    stage('Docker Image Build') {
      steps {
          sh "docker build -t ${dockerHubRegistry}:${currentBuild.number} ."
          sh "docker build -t ${dockerHubRegistry}:latest ."
          // dockerHubRegistry 위에서 선언한 변수, 내 저장소.
          // currentBuild.number 젠킨스가 제공하는 변수. 빌드넘버를 받아옴.
          }
      post {
        failure {
          echo 'Docker Image Build failure'
        }
        success {
          echo 'Docker Image Build success'  
        }
      }
    }
    stage('Docker Image Push') {
      steps {
          // 도커 허브의 크리덴셜
          withDockerRegistry(credentialsId: dockerHubRegistryCredential, url: '') {
          // withDockerRegistry : docker pipeline 플러그인 설치시 사용가능.
          // dockerHubRegistryCredential : environment에서 선언한 docker_cre  
            sh "docker push ${dockerHubRegistry}:${currentBuild.number}"
            sh "docker push ${dockerHubRegistry}:latest"
          }  
      }
      post {
        failure {
          echo 'Docker Image Push failure'
          sh "docker rmi ${dockerHubRegistry}:${currentBuild.number}"
          sh "docker rmi ${dockerHubRegistry}:latest"
        }
        success {
          echo 'Docker Image Push success'
          sh "docker rmi ${dockerHubRegistry}:${currentBuild.number}"
          sh "docker rmi ${dockerHubRegistry}:latest"
          slackSend (color: '#0AC9FF', message: "SUCCESS: Docker Image Push '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
      }
    }
    stage('Docker Container Deploy') {
      steps {
          sh "docker rm -f spring"
          sh "docker run -dp 7979:8085 --name spring ${dockerHubRegistry}:${currentBuild.number}"
          }
      post {
        failure {
          echo 'Container Deploy failure'
        }
        success {
          echo 'Container Deploy success'  
        }
      }
    }
  }
}

# vi src/main/resources/templates/index.html 파일에 들어가서 적절하게 내용을 수정한 후(awesome을 삭제) 빌드 해당 주소로 접속하면 업데이트된 내용으로 앱이 배포되는것을 확인할 수 있다.

<slack 연결>
#설치 후 재부팅
#success post 밑에 슬랙 알림을 추가해준다.
slackSend (color: '#0AC9FF', message: "SUCCESS: Docker Image Push '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")

#알림이 정상적으로 오는것을 확인

< github webhook 추가>
#앞에 파란체크가 떠있으면 웹훅이 정상적으로 동작.
#젠킨스에서 새로운 빌드가 트리거됨.

- 로컬 vm에서 와이파이로 사용하는 경우
#공유기에 접속
admin / admin

http://rapa1.iptime.org:30047/github-webhook/

rapa203 와이파이 사용하시는분들은 rapa1.iptime.org 대신 1.223.55.43
```