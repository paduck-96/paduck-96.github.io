---
title: "HTML Form부터 여러 <tag>에 대해서"
excerpt: "여러 데이터를 받아서 서버에 전송하기 위해 사용하는 html의 form. 다양한 상황에 쓰이는 태그들까지"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, html, css]

toc: true
toc_sticky: true

date: 2022-11-02
last_modified_at: 2022-11-02
---

form
=>웹 클라이언트에서 여러 데이터를 받아서 서버에게 한 번에 전송하기 위해서 사용 1.기본 형식

<form action="데이터를 전송받을 서버 URL" method="요청 방식" enctype="multipart/form-data">

</form>

=>action 은 생략하면 요청한 URL(현재 웹 브라우저의 URL)
=>method 는 생략하면 get
GET: 데이터를 URL에 붙여서 전송하는 방식으로 일반적으로 조회할 때 사용 - READ
데이터가 URL에 노출이 되기 때문에 보안성이 떨어지고 데이터의 길이에 제한이 있음
자동 재전송 기능이 있기 때문에 서버 나 네트워크가 일시적으로 사용 중지 되었다가 복구되는 경우 자동으로 전송되는 경우가 있습니다.

POST: 데이터를 헤더에 숨겨서 전송하는 방식으로 예전에는 READ를 제외한 모든 작업에 사용했는데 지금은 삽입에만 사용하는 것을 권장 - CREATE
숨겨서 전송하기 때문에 보안성이 뛰어나고 URL에 포함시키지 않기 때문에 길이에 제한이 없음
패스워드 나 파일이 존재할 때는 반드시 POST 나 PUT 또는 PATCH를 사용해야 합니다.

PUT: 전송 방법은 POST 와 동일한데 데이터 전체 수정에 사용 - UPDATE
PATCH: 전송 방법은 POST 와 동일한데 데이터의 일부분 수정에 사용 - UPDATE

DELETE전송 방법은 GET 과 동일한데 데이터 삭제에 사용 - DELETE

OPTIONS: 리소스가 지원하고 있는 메서드 취득
HEAD: 서버 리소스의 헤더
CONNECT: 프록시 동작의 터널 접속을 변경

=>enctype: 보통 때는 설정하지 않아도 되고 파일을 전송하고자 하는 경우에만 multipart/form-data 로 설정해주면 됩니다.
기본값은 application/x-www-form-urlencoded(인코딩해서 전송) 입니다.

2.그룹화 및 제목
=>그룹화를 하고자 할 때는 fieldset 을 이용하고 그룹에 제목을 붙이고자 할 때는 fieldset 안에 legend 태그를 이용해서 제목을 작성

3.label
=>텍스트를 출력할 때 사용
=>for 라는 속성에 다른 입력 객체를 연결시킬 수 있습니다.
모바일에서는 필수

4.input
=>사용자로부터 입력을 받거나 이벤트를 받기 위한 용도로 사용하는 태그
=>type 속성은 종류를 선택하는 것인데 HTML4.01 에서는 10가지가 있고 HTML5에서는 다양한 type이 추가됨
text: 한 줄 입력
password: 한 줄을 입력받는데 텍스트가 마스킹되서 보임
radio: 여러 개 중 하나를 선택하게 할 때 사용
checkbox: 여러 개 중에서 0개 이상을 선택하게 할 때 사용
file: 파일
image: 이미지 버튼
submit: form 의 데이터를 서버에게 전송하는 기본 역할을 수행
reset: form 의 데이터를 모두 삭제하는 기본 역할을 수행
button: 버튼
hidden: 눈에 보이지 않음

=>name 속성은 서버에게 전달할 때 같이 전송되는 이름
radio 나 checkbox를 하나의 그룹으로 만들 때는 name 속성을 동일하게 해주면 됩니다.
=>size, maxlength, checked, disabled, readonly 등이 있음
=>radio를 만들 때는 되도록이면 하나의 데이터를 선택된 상태로 출력되도록 해주는 것이 좋습니다.
=>value는 값을 설정하는 것인데 text 에서는 보여지는 글자가 되고 radio 나 checkbox에서는 구분하기 위한 값
file 에는 value 설정이 안됨

5.textarea
=>여러 줄의 글을 입력받고자 할 때 사용
=>name, value, rows 와 cols 속성을 제공
value는 처음에는 사용할 수 없고 초기값은 태그 사이에 작성해야 함
=>여는 태그 와 닫는 태그가 다른 줄에 있으면 커서가 첫번째 칸에 위치하지 않음

6.select
=>목록 태그
=>select 태그 안에 option 태그로 목록을 만듭니다.
=>select 에는 name을 설정하고 option에는 value를 설정합니다.

7.button
=>button 은 form 안에 있으면 submit의 기능을 수행
<button>텍스트</button>
