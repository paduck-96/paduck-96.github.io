---
title: "Kubernetes  서브넷 과 인스턴스"
excerpt: "Kuberentes 서브넷을 생성하고, 이를 활용하는 인스턴스에 대해 학습하고,  
이를 기반으로 로드밸런싱을 진행한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-07
last_modified_at: 2023-02-07

---
```docker
예)
https://github.com/pcmin929/simple_jar
에는 스프링부트로 제작된 간단한 어플리케이션

10.200.0.0 /16 의 CIDR을 갖는 exam-vpc에 
a 와 c 가용영역을 사용하는 두개의 퍼블릭 서브넷(exam-pub-sub1,2)을 만들어서 최소 두개이상의 인스턴스에서 위 어플리케이션을 배포하고, 로드밸런싱 하시오. 단, 커스텀이미지를 통해 생성한 인스턴스여야함.

리눅스에서는 쉘스크립트라는 파일을 통해 여러개의 명령어를 동시에 실행 시킬 수 있는데 아래의 내용을 갖는 파일을 start.sh로 만들어서 스프링부트 파일을 실행시킬 수 있다. jar파일은  /home/ec2-user/simple_jar 에 존재해야하며 내가 현재 있는 경로를 보는 방법은 pwd라는 명령어를 통해 확인 가능

서버 재시작시 아래의 스크립트 파일이 동작을 하도록 하면 스프링부트 파일이 배포될 것이다.

cat start.sh
#!/bin/bash
sudo chmod 755 /home/ec2-user/simple_jar/springbootApp.jar
sudo nohup java -jar /home/ec2-user/simple_jar/springbootApp.jar 1> /dev/null 2>&1 &
echo $! > pid.file

# 인스턴스 생성후

sudo amazon-linux-extras enable corretto8
sudo yum install java-1.8.0-amazon-corretto -y
sudo yum -y install git
sudo git clone https://github.com/pcmin929/simple_jar
sudo chmod 777 simple_jar/springbootApp.jar
sudo vi start.sh
sudo vi /etc/rc.d/rc.local

# 두개의 파일 모두 모든 권한을 줘서 실행될 수 있게끔 만든다.
sudo chmod 777 /etc/rc.d/rc.local
sudo chmod 777 start.sh

#재부팅하여 위와 같이 접속되는지 확인. 
#잘 된다면 중지 후 이미지를 생성한다

# app-2 로 한개 더 생성
각 인스턴스에 설치되어있는 어플리케이션의 동작여부를 8085번 포트로 확인

# 로드 밸런서탭으로 돌아와서 생성한 타겟그룹 지정
#오른쪽에 available이 뜨면 구글 계정으로 연결해서 회원가입 후 결제.

# 오토스케일링이 될 인스턴스 설정
sudo amazon-linux-extras enable corretto8
sudo yum install java-1.8.0-amazon-corretto
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.27/bin/apache-tomcat-10.0.27.zip
sudo yum -y install unzip
sudo unzip apache-tomcat-10.0.27.zip
sudo rm -rf apache-tomcat-10.0.27.zip
sudo chmod 777 -R tomcat
./tomcat/bin/startup.sh
curl localhost:8080

sudo vi tomcat/webapps/ROOT/test.jsp
#아래의 내용 추가
<%@ page contentType="text/html; charset=UTF-8"%>
<html>
    	<head><title>hello world</title></head>
    	<body>
    	<h2>
            	TOMCAT TEST<br><br>
            	time : <%= new java.util.Date()%>
            	<%@ page import="java.net.InetAddress" %><br>
            	<%InetAddress inet= InetAddress.getLocalHost();%>
            	WAS ip : <%=inet.getHostAddress()%>
    	</h2>
 
 
    	</body>
</html>

sudo amazon-linux-extras install epel -y
# 부하를 주기위한 패키지(stress) 설치
sudo yum -y install stress

#재부팅시에 톰캣 동작하도록 내용 추가
sudo vi /etc/rc.d/rc.local

#재부팅후에 잘 동작하는지 반드시 확인.
sudo chmod +x /etc/rc.d/rc.local

#인스턴스 중지 후 이미지 생성.

# 이미지로 시작 템플릿을 생성한다.
로드밸런서 => 타겟그룹 => 오토스케일링 그룹 순으로 만듬.

# <타겟그룹>
#헬스체크 주기가 너무 짧아도 문제. 왜냐하면 잘 동작하는 서버를 문제서버로 판별할수도 있음.
#빈 타겟그룹 생성.

# 아까 생성하던 로드밸런서를 마저 생성.
#감소 정책 생성
#으로 검색
#CPU 사용량이 30% 이하면 인스턴스를 줄이겠다.

# 오토스케일링 그룹에 있는 인스턴스에 접속, 창을 두개 띄워서 아래창에는 top라고 입력하고 위의 창에는 
stress --cpu 1 --timeout 1200

#를 입력한다. cpu 한개만큼의 부하를 1200초동안 주겠다는 뜻.
#CPU 사용량이 100%임을 확인
#CloudWatch 와 오토스케일링 그룹의 인스턴스관리탭을 주시한다.

# 오토스케일링
#삭제시
1.로드밸런서 삭제
2.오토스케일링 그룹 삭제
3.대상그룹 삭제

예제)
1. 로드밸런서 생성
2. 타겟그룹 생성
3. 오토스케일링 그룹
- CPU 사용량이 80% 이상일때 증가
- CPU 사용량이 20% 이하일때 감소
```