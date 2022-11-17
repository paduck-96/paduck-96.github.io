---
title: "HTML form을 포함한 다양한 태그"
excerpt: "여러 데이터를 받아서 서버에 전송하기 위해 사용하는 html의 form을 학습하고,
다양한 HTML 태그들을 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, html, html form, html tags]

toc: true
toc_sticky: true

date: 2022-11-01
last_modified_at: 2022-11-11
---

# Form

## 1. form

웹 클라이언트에서 여러 데이터를 받아서 서버에게 한 번에 전송하기 위해 사용

1. 기본 형식

```javascript
<form
  action="데이터를 전송받을 서버 URL"
  method="요청 방식"
  enctype="multipart/form-data"
></form>
```

- action은 생략하면 요청한 url  
  현재 웹 브라우저의 url

2. METHOD  
   default는 get

- GET
  - 데이터를 url에 붙여서 전송하는 방식으로 일반적으로 조회할 때 사용  
    `READ`
  - 데이터가 URL에 노출이 되기 때문에 보안성이 떨어지고 데이터의 길이에 제한이 있음
  - 자동 재전송 기능이 있기 때문에 서버 나 네트워크가 일시적으로 사용 중지 되었다가 복구되는 경우 자동으로 전송되는 경우가 있음
- POST
  - 데이터를 헤더에 숨겨서 전송하는 방식으로 예전에는 READ를 제외한 모든 작업에 사용했는데 지금은 삽입에만 사용  
    `CREATE`
  - 숨겨서 전송하기 때문에 보안성이 뛰어나고 URL에 포함시키지 않기 때문에 길이에 제한이 없음
  - 패스워드 나 파일이 존재할 때는 반드시 POST 나 PUT 또는 PATCH를 사용
- PUT
  - 전송 방법은 POST와 동일한데 데이터 전체 수정에 사용  
    `UPDATE`
- PATCH
  - 전송 방법은 POST와 동일한데 데이터의 일부 수정에 사용  
    `UPDATE`
- DELETE
  - 전송 방법은 GET과 동일한데 데이터 삭제에 사용  
    `DELETE`
- OPTIONS
  - 리소스가 지원하고 있는 메서드 획득
- HEAD
  - 서버 리소스의 헤더
- CONNECT

  - 프록시 동작의 터널 접속을 변경

  ENCTYPE

  - 보통 때는 설정하지 않아도 되고 파일을 전송하고자 하는 경우에는  
    multipart/form-data로 설정
    - 기본 값은 application/x-www-form-urlencoded  
      (인코딩해서 전송)

3. 그룹화 및 제목  
   그룹화를 하고자 할 때는 fieldset을 이용하고  
   그룹에 제목을 붙이고자 할 때는 fieldset 안에 legend 태그 이용해 제목 작성

4. label  
   텍스트를 출력할 때 사용

   - for라는 속성에 다른 입력 객체를 연결 시킬 수 있다  
     모바일에서는 필수

5. input  
   사용자로부터 입력을 받거나 이벤트를 받기 위한 용도로 사용하는 태그

   - type 속성은 종류를 선택하는 것인데 HTML 4.01에서는 10가지고 있고  
     HTML5에서는 다양한 type이 있음
     - text  
       한 줄 입력
     - password  
       한 줄을 입력 받는데 텍스트가 마스킹되서 보임
     - radio  
       여러 개 중 하나를 선택하게 할 때 사용
     - checkbox  
       여러 개 중에서 0개 이상을 선택하게 할 때 사용
     - file  
       파일
     - image  
       이미지 버튼
     - submit  
       form의 데이터를 서버에게 전송하는 기본 역할을 수행
     - reset  
       form의 데이터를 모두 삭제하는 기본 역할을 수행
     - button  
       버튼
     - hidden  
       눈에 보이지 않음
   - name 속성은 서버에게 전달할 때 같이 전송되는 이름  
     radio나 checkbox를 하나의 그룹으로 만들 때는 name 속성을 동일하게 해줘야 함
   - size, maxlength, checked, disabled, readonly 등이 있음
   - radio를 만들 때는 되도록이면 하나의 데이터를 선택된 상태로 출력
   - value는 값을 설정하는 것인데 text에서는 보여지는 글자  
     radio나 checkbox에서는 구분하기 위한 값  
     file에는 value 설정이 없음

6. textarea  
   여러 줄의 글을 입력받고자 할 때 사용

- name, value, rows 와 cols 속성을 제공
  value는 처음에는 사용할 수 없고 초기 값은 태그 사이에 작성
- 여는 태그와 닫는 태그가 다른 줄에 있으면 커서가 첫번째 칸에 위치하지 않음

7. select  
   목록 태그

- select 태그 안에 option 태그로 목록을 만듦
- select 에는 name을 설정하고 option에는 value를 설정

8. button  
   button은 form 안에 있으면 submit의 기능을 수행

- ```
  <button>텍스트</button>
  ```
