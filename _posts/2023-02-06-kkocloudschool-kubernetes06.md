---
title: "Kubernetes  서브넷 활용"
excerpt: "Kuberentes 서브넷을 활용해 외부 접속을 허용하는 것에 대해 학습한다"

categories:

- Blog
  tags:
- [Blog, kakaocloudschool, develop, java, kubernetes]

toc: true
toc_sticky: true

date: 2023-02-06
last_modified_at: 2023-02-06

---
```docker
# 서브넷 생성
# 해당 서브넷에서 인스턴스를 생성할때 
#퍼블릭 아이피(외부와 통신이 가능한 IP)를 자동으로 할당
# 기본적으로 인스턴스를 생성할때 사설IP와 공인IP
#이렇게 두개의 아이피가 할당 가능

# 공인(퍼블릭)IP VS 사설(프라이빗)IP
#대역으로 구분이 가능.
#일반적으로 공인IP를 사용하면 공인네트워크
#사실IP를 사용하면 사설네트워크로 구분

#VPC에는 퍼블릭 서브넷과 프라이빗 서브넷이라는 개념
# 퍼블릭 서브넷은 외부와 통신이 가능한(IN,OUT) 가능한 서브넷 (ex. rapa-vpc-pub-sub1)
# 프라이빗 서브넷은 내부에서만 통신이 가능한 서브넷.

# 통신이 안되는경우 확인해봐야할것들.
#1. 인스턴스가 퍼블릭 서브넷에 존재(퍼블릭IP)
#2. 보안그룹(출발지 및 포트[22])
#3. 키페어,사용자명(ec2-user)

예제) rapa-vpc 에 ‘rapa-pub-sub2’라는 퍼블릭 서브넷을 생성하시오.
대역은 10.10.2.0 /24로 하고, 인스턴스 생성시 자동으로 퍼블릭IP를 할당 받아야 한다.
생성 후 해당 서브넷에 인스턴스 생성후 xshell로 ssh 접속

# EC2 - 인스턴스 시작
# RDS를 배치하기 위한 DB서브넷(프라이빗 서브넷) 생성
# 해당 서브넷에 RDS를 생성
# rapa vpc 내에 있는 자원들만 접속 가능한 RDS 생성.

예제) 
1.rapa 퍼블릭 서브넷에 인스턴스를 하나 생성. 
2.rapa 서브넷 그룹에 rds를 하나 생성
3.생성한 인스턴스에서 rds에 접속을 해보세요!

# RDS에 접속하기 위한 EC2 인스턴스  생성
# xshell로 인스턴스 접속후
sudo yum -y install mysql
mysql -u admin -ptest1234 -h database-1.c901gurflnsy.ap-northeast-2.rds.amazonaws.com -P 33306
# -u : db계정(RDS 생성시 마스터계정)
# -p :패스워드(마스터암호)
# -h : 주소(엔드포인트의 주소)
# -P : 포트지정(데이터베이스 포트)

# 인스턴스에 간단한 스프링부트앱 배포
# 저장공간 활성화
sudo amazon-linux-extras enable corretto8

#패키지 다운로드
sudo yum install java-1.8.0-amazon-corretto

#깃 설치.
sudo yum -y install git

#jar 파일이 있는 깃허브레포지토리
git clone https://github.com/pcmin929/simple_jar
cd simple_jar
java -jar springbootApp.jar

# 보안그룹에서 8085번 개방 후 접속

# 톰캣 서버 설치
https://tomcat.apache.org/download-10.cgi

sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.0.27/bin/apache-tomcat-10.0.27.zip
sudo yum -y install unzip
sudo unzip apache-tomcat-10.0.27.zip
sudo mv apache-tomcat-10.0.27 ~/tomcat
cd ~

# tomcat의 하위디렉토리 포함 모든 파일에 대해 읽고,쓰고,실행할 권한을 주겠다.
sudo chmod 777 -R tomcat
cd tomcat
# 톰캣 실행
./bin/startup.sh

#톰캣 샘플 어플리케이션 경로.
https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war
cd ~

# 샘플 어플리케이션 다운로드
wget https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war

# sample.war 파일을 tomcat/webapps 경로에 복사
cp sample.war tomcat/webapps/
# WAR 파일의 압축이 자동으로 풀려 배포되고 있는 것을 확인할 수 있다.

# <RDS와 연동 확인>
cd tomcat/lib/
wget https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.23/mysql-connector-java-8.0.23.jar

# 라이브러리를 반영시키기 위해 tomcat 중지
~/tomcat/bin/shutdown.sh

# tomcat 시작
~/tomcat/bin/startup.sh
cd ..
pwd
/home/ec2-user/tomcat
cd webapps/ROOT/

sudo vi dbtest.jsp
    <%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
    <%@ page import="java.sql.*"%>
    <h1>DB</h2>
    <%
            Connection conn=null;
            try{
                    String Url="jdbc:mysql://<rds의 엔드포인트>/<초기데이터베이스이름>";
                    String Id="<마스터계정>";
                    String Pass="<마스터암호>";
    
                    Class.forName("com.mysql.jdbc.Driver");
                    conn=DriverManager.getConnection(Url,Id,Pass);
                    out.println("was-db Connection Success!");
            }catch(Exception e) {
                    e.printStackTrace();
    }
    %>
#엔드포인트 끝에 포트번호를 명시

# 서버 부팅시 실행할 명령어를 명시
 sudo vi /etc/rc.d/rc.local

# 해당 파일에 실행 권한 부여
sudo chmod +x /etc/rc.d/rc.local

# 서버를 재부팅 후 접속하여 정상적으로 tomcat이 동작을 하는지 확인

# <인스턴스로 커스텀 AMI 만들기>
AMI : Amazon Machine Image

#인스턴스 부팅후 접속하여
sudo vi tomcat/webapps/ROOT/test.jsp
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
#중지후 이미지 생성
#기존의 이미지 삭제후 새로운 이미지
이름 : new_test-img  , 설명 : new_test-img

# 두번째 인스턴스는 이름은 test-2
# 서브넷은 rapa-pub-sub2 로 하고 나머지는 다 동일하게 구성
#각 인스턴스가 잘 동작하는지 테스트

# <두개의 인스턴스에 순차접속하기 위한 로드밸런서를 생성>
#1.로드밸런서 생성
#2.타겟그룹(test-1 및 test2)

# ALB 선택
# 외부에서 찾아오기위해선 internet-facing 선택
# 밑으로 서버 두대가 내려옴
# 로드밸런서를 설정하던 탭으로 돌아와서 타겟그룹의 새로고침을 한번 누른다.
# 로드밸런서를 통해 각 인스턴스에 순차접속되는것을 확인할 수 있다.
#DB와의 연동도 잘 되는것을 확인
#ELB 삭제
#타겟 그룹 삭제

예제) 남은시간동안

1. 인스턴스 두개에 하나는 nginx , 하나는 httpd를 설치한다. 각 인스턴스는 pub1, pub2 서브넷에 둔다
2. 두 인스턴스를 타겟그룹으로 하는 로드밸런서를 생성한다
3. 로드밸런싱이 되는것은 확인한다.
```