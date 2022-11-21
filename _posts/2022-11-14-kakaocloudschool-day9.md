---
title: "데이터 표현 방법, 서버로 데이터 전달하는 방법 그리고 HTML5"
excerpt: "데이터 표현 방법들에 학습하고
서버로 데이터 전달하는 방법에 학습하며
HTML5에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, javascript, Data Format, Server Data, HTML5]

toc: true
toc_sticky: true

date: 2022-11-14
last_modified_at: 2022-11-21
---

# 데이터 표현 방법

서로 다른 방식의 컴퓨터에서 `데이터를 주고 받으려면 표준적인 포맷`이 필요

- Parsing  
  가져온 `데이터를 해석`하는 과정

  - **서버 입장**에서는 데이터 표현방식을 `데이터를 만들어서 제공`  
    **클라이언트 입장**에서는 데이터를 `파싱해서 출력`하는 것이 중요

- DTD  
  HTML, XML은 브라우저 해석 혹은 설정 이용에 사용하는데  
  설정을 만든 곳에서 해석하는데 그 때의 해석 위치

1. 텍스트  
   일반 문자열

   - **`CSV`**  
     구분 기호를 가지고 구분할 수 있도록 만든 포맷

   - 용량이 작아 변하지 않는 데이터 제공할 때 주로 이용

2. XML  
   태그 형식으로 데이터를 표현  
   해석을 개발자가 하는 것이 HTML 과의 차이

   - 설정이나 데이터 전송에 이용하나  
     다른 방식보다 용량이 커 사용 빈도 수 감소

3. **`JSON`**  
   자바스크립트의 데이터 표현법을 이용하는 방식

   - XML 보다 용량이 작아 데이터 전송에 이용

4. YML(YAML - 야믈)  
   이메일 데이터 표현 방식  
   가독성이 좋고 용량이 작아 설정에 많이 이용

   - 구글 제품, spring 등에서 사용  
     데이터 전송 사용 빈도 수 적음

5. HTML  
   데이터 표현이 아닌 화면 가시화
   - Scraping  
     화면에 보여지나 Open API를 제공하지 않는 경우  
     HTML을 가져와 임의 해석하여 데이터처럼 사용
     - 저작권 확인 필수

# 웹 클라이언트에서 서버로 데이터 전송 방법

1. URL에 포함시키기  
   (최신식)

   - 데이터 1개일 때 주로 이용
   - Bloter.net이나 Tistory의 상세보기가 그 예

2. 요청 방식에 따른 데이터 전달

- **`GET`**  
  데이터를 요청할 때 사용하는 방식

  - URL 뒤에 \? 추가하고 `이름 = 값 의 형태`로 데이터 전송

    - 데이터 부분을 parameter || query string 이라고 함
      - **한글이 포함될 경우 인코딩 필수**
      - **파일은 인코딩 X**

  - `history에 남아 보안성은 떨어지나 캐싱이 가능해 재전송이 가능`
  - `데이터 길이 제한`

  조회에는 GET을 사용하고 삭제에는 DELETE를 사용

- **`POST`**  
  삽입, 갱신, ~~삭제~~ 를 할 때 사용하는 방식
  - **HTTP BODY**에 데이터를 넣어서 전송
  - `보안성이 우수하고, 데이터 길이 제한 없음`
  - `캐싱이 안되기 때문에 자동 재전송 되지 않음`
    - 삽입할 때만 사용 권장  
      갱신은 PUT 등 권장

3. **헤더 설정**을 이용해서 데이터 전달

- 주로 인증 정보 전송할 때 이용

# 다른 방식을 이용한 Ajax

**1. Promise를 이용하는 방식**

- 콜백 안에서 콜백을 호출하는 경우 Promise가 가독성이 더 좋음

- promiseajax.html 파일 생성

**2. Fetch API**  
서버의 데이터를 가져오는 로직을 단순화한 ajax보다 새로운 API

- 기본적으로 Promise를 리턴하기 때문에 `별도 콜백 생성 불필요`

요청 과 응답 등의 요소를 Javascript 에서 접근하고 조작할 수 있는 인터페이스 제공

- fetch( ) 전역 함수를 이용해 `네트워크의 리소스를 비동기적`으로 가져올 수 있음
  - XMLHttpRequest 대신 사용
  - fetch(요청 URL, 옵션)의 형태로 작성하는데 결과는 Promise 객체로 리턴  
    then 과 catch 를 이용해서 처리
    - 옵션: https://developer.mozilla.org/en-US/docs/Web/API/fetch

`요청에 `**성공**`했을 때 전달되는 객체는 `**Response**

- Response 속성

  - status  
    상태 코드 값을 담은 정수
  - statusText  
    상태 코드 메시지
  - ok  
    성공 여부 판단

- Response 메서드

  - arrayBuffer()  
    바이트 배열
  - **blob()**  
    파일의 내용
  - `formData()`  
    폼의 데이터
    - form의 형태로 응답하는 경우
  - `json()`  
    JSON 파싱한 결과
    - JSON으로 응답하는 경우
  - text()  
    결과 데이터 문자열
    - 문자열로 응답하는 경우

일반적인 처리 방식

- fetch(url, {옵션이름:옵션값...})  
  .then((response)=>response.메서드)  
  .then((data)=>{파싱한 결과를 가지고 수행할 내용})  
  .catch((error)=>{에러가 발생했을 때 수행할 내용})

- fetch 옵션 설정

  - method
  - mode
  - cache
  - credentials
  - headers
  - redirect
  - referrerPolicy
  - body

# HTML5

1. 정의와 특징

   - XHTML4에서 진화한 마크업 언어
   - 디자인 과 내용의 분리  
     특별한 경우가 아니면 디자인은 CSS를 이용
   - 자바 Applet, Active X, 플래시와 같은 `외부 플러그인 최소화`
     - 자바의 플렉스 나 .Net 의 실버라이트 배제
   - **Semantic**  
     문서 구조의 의미나 문서 안에 삽입된 데이터의 의미 등을 명확하기 위한 사항 추가
   - 마크 업 언어에 웹 애플리케이션을 만들 수 있는 API를 포함

2. 태그의 변화

   - 추가된 태그
     - 문서 구조와 관련된 태그
     - 외부 콘텐츠 사용
     - 폼 관련 태그
     - 텍스트 관련 태그
   - 변경된 태그
     - hr, menu, small, strong
   - 태그에 추가된 속성
     - input  
       모바일을 위한 속성 과 유호성 검사 관련 속성 이 추가
   - 없어진 요소
     - bgsound
     - applet 등

3. API 변화

   - Cookie가 아닌 `로컬 저장소` 출현
     - **Web Storage**
     - Web SQL
     - Indexed DB
   - Drag And Drop API
   - **GeoLocation**  
     위치 정보 사용
   - **Canvas**  
     그리기 API
     - 그래프 나 이미지 출력을 외부 라이브러리 도움 없이 구현 가능
   - Web Worker  
     Thread 사용
   - Math ML  
     수식 표현
     - FireFox만 적용
   - Communication API
     - ajax level 2
     - Server Sent Events(Web Push)
     - **Web Socket API**

4. 권장 사항

`Cookie 보다는 Local Storage 를 이용하는 것을 권장`

- Cookie  
  서버에게 요청할 때 마다 전송되고 문자열만 저장 가능
- Local Storage  
  자바스크립트 객체를 저장할 수 있고 필요할 때 서버에게 전송 가능

`Cache 보다는 로컬 데이터베이스를 사용하는 것을 권장`

`자바스크립트 애니메이션 대신 CSS의 전환 효과를 이용`

`부담을 많이 주는 작업은 웹 워커 사용을 권장`

- 자바스크립트를 이용해서 플러그인 이나 PC 직접 동작 앱 개발

5.  기본적인 문서 구조

    - DOCTYPE - DTD

    ```
    <!Doctype html>
    ```

    - 인코딩 설정

    ```
    <meta charset="인코딩 방식">
    ```

    - 문서 검증  
      http://html5.validator.nu

6.  HTML5를 지원하지 않는 브라우저를 위한 코딩

    - 적용 범위 확인  
      http://caniuse.com/#index

7.  Mark up  
    명확한 의미 전달을 대신 할 수 있는 요소 추가

        - header
        - section
        - article
        - aside
        - nav
        - footer

    - div와 동일한 역할을 수행

      - 브라우저의 내용을 인간이 아닌  
        **robot 이 읽었을 때** 명확하게 의미 전달하기 위해

    - figure, figcaption  
      이미지 나 그래프 또는 예제 코드 등을 작성할 때  
      문서 안에 삽입해서 의미를 전달하기 위해 사용
      - 음성 부라우저에서 그림에 대한 설명 읽기
    - ruby  
      글자 위에 조그마한 글자를 표시하는 기능
      - 한자 위에 독음 표시용

8.  https://github.com/itggangpae/html5.git

9.  멀티미디어

    - video  
      비디오 재생을 위한 태그  
      자바스크립트 객체로 추가됨

      ```
      <video id="video"></video>
      document.getElementById("video")
      ```

      외부 플러그인 없이 동영상을 제어할 수 있음

      - 브라우저 별로 코덱이 다르기 때문에  
        모든 동영상이 전부 재생되진 않음

      `동일한 동영상을 여러 포맷으로 만들어 제공하는 편을 권장`

      ```
      <video src="동영상 url"></video>
      <video>
        <source src="동영상 url" />
        <source src="동영상 url" />
      </video>
      ```

      - 재생할 수 있는 source를 재생

    - audio  
      video 태그 와 사용법이 같음

10. canvas  
     `그래픽을 자유롭게 출력할 수 있는 html5에서 추가된 API`

    - 2D 그래픽을 지원
    - 3D 그래픽은 Web GL을 이용해서 구현
    - 그래픽 가속 기술  
      Open GL(windows는 Direct X)
      - 쉽게 사용하게 만든 그래픽 엔진이  
        Unity 3D, Unreal, Open GL ES 등
    - 웹 그래픽 가속  
      Web Gl
    - 오디오 가속  
      Open AL
    - 그래픽 편집  
      Open CV
    - 태그로는 아무 일도 하지 못하고, 자바스크립트로 작업

      ```javascript
      <canvas id="아이디" width="너비" height="높이"></canvas>

      let 캔버스변수 = document.getElementById("아이디");
      let context변수 = 캔버스변수.getContext("2d");

      context 변수로 그리기 작업
      context변수.save() // 정보 저장
      context변수.restore() // 정보 복원
      ```

    - Context  
      어떤 작업을 하기 위해 필요한 부가 정보

      - 선 색상, 면 색상, 선 두께 등의 정보가 필요  
        설정할 게 너무 많아, context에 저장해두고 필요한 부분만 수정해서 사용

    - ImageSprite  
      여러 개의 이미지를 하나의 파일로 만들고 이미지를 적절히 잘라 사용

      - 보조 기억 장치에 있는 파일을 읽어 오는게 시간이 많이 소모되는 작업이기 때문에  
        `이미지를 하나로 묶어서 나누어 출력`하는 것이 효율적

    - 각 픽셀의 값을 제어 가능
      - 원형 UI 클릭  
        배경 투명인 상태로 원을 그리고 클릭했을 때 그 영역의 색상 값 추출  
        알파 값이 0이면 동작을 하지 않고, 1이면 동작하도록 제작

11. SVG(Scalable Vector Graphics)

- XML을 이용해서 벡터 그래픽을 표시하는 API

  - 웹 표준

- canvas는 Pixel 단위로 그림을 그리지만 SVG는 벡터 이미지

`svg와 canvas 비교`

- 실제 그래프 사용할 경우 직접 작성도 물론 가능하지만  
  d3js 나 jqplot 같은 자바스크립트 라이브러리를 사용

10. FORM

- 기능 변화
  - 여러가지 타입 추가
  - 유효성 검사 기능 추가
  - 진행 상황을 표시해주는 progress 와 meter 추가
- input 의 type 추가  
  모바일을 위한 속성

  - 기존 type  
    text, password, checkbox, radio, file, button, image, submit, reset, hidden
  - 추가된 type  
    search, tel, url, email, number, range, color, date, month, week, time, datetime-local
  - 네트워크 맞추었을 때 스마트 폰에서 접속하는 방법
    - http://127.0.0.1:5500/day9/html5input.html 를  
      http://ip주소:5500/day9/html5input.html로 변경
  - 다른 컴퓨터에서 접속 가능하게 하는 방법
    - 방화벽 해제
    - 로컬 포트포워딩 설정
  - jQuery를 왜 사용했는지
    - HTML5는 브라우저 별로 지원 범위가 다름  
      따라서 추가 기능 사용하려면 브라우저 별로 다른 설정 필요
      - jQuery는 크로스 브라우징을 쉽게 해주는 js 라이브러리
        - 국내에서는 IE 비중이 낮아졌고, Edge는 HTML5를 지원  
          jQuery 사용 빈도 매우 감소

- progress 와 meter  
  진행률을 보여주고자 할 때 사용하는 요소

  - 자바스크립트를 통해 제어
  - 기본적으로 max value는 100으로 설정  
    value 속성을 이용해서 진행률 표시
    - 최대값, 최소값은 min 과 max 속성 값 변경으로 가능

- 추가된 속성

  - file  
    multiple 속성이 추가되서 속성을 설정하면 한 번에 여러 개 파일 선택 가능
  - autocomplete

    자동 완성 기능 사용 여부로 on / off 설정

  - list 속성과 datalist 속성을 이용하면 입력 값 선택하게끔 가능
    ```
    <input type="tel" list="telephonelist">
    <datalist id="telephonelist">
        <option value = "010-1234-1234" />
        <option value = "010-4321-4321" />
    </datalist>
    ```
  - placeholder  
    입력할 때 설명을 위한 텍스트 설정
  - autofocuse  
    첫 번째 포커스 설정

- 유효성 검사(Validation Check)
  - required  
    필수 입력
  - maxlength  
    최대 글자 수
  - min, max  
    최소, 최대값
    - 숫자 나 날짜에 사용
  - pattern  
    정규 표현식 설정 가능

---

WebClient(브라우저 또는 애플리케이션) <->

Web Server <->

Application Server(controller -service - repository) <->

Data Server(database server, file server)

- Web Server 부터 Data Server 까지는 `애플리케이션과 다른 곳에 위치`
  - `Web Client 와 Server 간에 통신에는 트래픽(비용)이 발생`
    - client->server로 `전송하기 전에 유효성 검사` 필수
    - server 가 client 로 부터 `data를 받으면 유효성 검사` 필수
