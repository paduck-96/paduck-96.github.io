---
title: "Event, Event handler, 동기/비동기 그리고 Ajax에 관한 학습"
excerpt: "Event와 Event handler에 대해 학습하고,
동기/비동기 방식들에 대해 학습하며,
Ajax과 파싱에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, javascript, Event, Event handler, Async, Await, Promise, callback, ajax]

toc: true
toc_sticky: true

date: 2022-11-11
last_modified_at: 2022-11-18
---

# DOM(Document Object Model)

..이어서

3. DOM 객체의 공통 속성과 메서드

   - style 속성  
     스타일 지정하기 위한 속성
   - innerHTML 속성  
     **태그와 태그 사이의** 텍스트와 관련된 속성
     ```
     <태그></태그>
     ```
   - value 속성  
     **입력된 데이터 확인**  
     이벤트와 함께 많이 사용

   `DOM 객체마다 속성은 다르다`

# Event Handling

1. Event & Event Handler
   - Event  
     상태 변화
   - 사용자가 발생시키는 것(버튼 클릭이나 마우스 hover),  
     시스템이 발생시키는 것(타이머, 데이터 다운) 등
   - Event Handling  
     Event가 발생했을 때 수행할 동작을 설정

**`이러한 Event Handling을 수행하는 객체를 Event Handler`**  
**`또는 `Listener`라고 합니다`**

2. JavaScript 이벤트 처리 방법

   - inline  
     태그 안에서 이벤트 속성에 스크립트 코드를 직접 작성  
     `권장하지 않음`

   - 고전적 이벤트 처리 모델  
     객체.이벤트이름 = 함수 (이름이나 코드)

   - 표준 이벤트 모델

     객체.addEventListener("이벤트 이름", 함수 이름이나 코드)

     `이벤트 이름 앞에 on을 붙이지 않는다`  
     `하나의 이벤트에 여러 함수 연결 가능`

     - removeEventListener로 이벤트 제거 가능

3. 이벤트 객체

   이벤트를 처리하는 함수에는 `event 객체 1개`가 전달된다

   - 이 객체에 이벤트와 관련된 정보가 전달된다  
     이벤트 객체의 속성은 이벤트마다 다르다
     - IE 하위 버전에서는 이벤트 처리 함수에서 이벤트 객체를 받을 수 없어  
       window.event 로 이벤트 객체 가져온다
       ```
       (e)=>{let event = e || window.event}
       ```
   - **이벤트 처리 함수 내**에서 `this` 는 이벤트가 발생한 객체
     - 최근에는 `화살표 함수 사용의 증가로 잘 이용되지는 않음`
     - 대신, Document 객체를 이용해서 DOM을 찾아와 사용한다
   - 속성

     - **`data`**  
       Drag&Drop 할 때 Drop된 객체들의 url을 문자열 배열로 반환
     - height
     - layerX, layerY
     - modifier  
       같이 누른 조합키(alt, ctrl, shift ...)

     - pageX, pageY
     - screenX, screenY
     - **`target`**  
       이벤트가 전달된 객체
     - type  
       이벤트의 타입
     - **`which`**  
       마우스 버튼의 ASCII 코드 값이나 키보드의 키값  
       (IE는 window.event.keyCode)
     - width
     - x
     - y

4. Event Trigger  
   이벤트를 강제로 발생시키는 것

   - 객체.이벤트이름( )

**`5. Default Event Handler`**  
 특정한 태그들에는 기본적인 이벤트 처리 코드가 포함되어 있다.

- a 태그는 다른 url이나 책갈피로 이동
- input type이 submit 이면 form의 action으로 요청 전송
- input type이 reset 이면 form clear
- button이 form 안에 존재하면 submit 기능 수행
- keydown이 발생하면 누른 키를 input에 출력
  - submit과 reset은 버튼을 누르지만  
    실제로는 form의 이벤트

`기본 기능을 수행하지 않게 할 때는 event 객체의 `**preventDefault()**` 호출`

**`6. Event Bubbling`**  
 `부모와 자식, 양쪽에 동일한 이벤트에 대한 핸들러가 존재시,`  
 `자식 태그 이벤트가 발생하면 `**자식 태그의 핸들러를 수행**`하고 `**부모 핸들러도 수행**`

- 이벤트 버블링을 막고자 한다면  
  이벤트 객체의 stopPropagation 메서드 호출

7. 여러 종류의 이벤트

   - click, dbclick  
     누른 좌표는 screenX 와 screenY

   - keydown, keypress, keyup  
     which 속성을 이용해서 누른 키의 ASCII 코드 값을 찾아올 수 있는데,  
     상수 형태로 가져오거나, key로 문자로 가져올 수 있다
   - mousemove, mouseout, mouseover, mouseup  
     마우스의 움직임
   - focusin(focus), focusout(blur)  
     focusout 이벤트에서 유효성 검사를 하기도 한다
   - **`load`**  
     메모리에 적재되면..  
      `window 에서는 body에 있는 태그를 전부 읽어서 메모리에 적재하면`
     `img/file 의 경우에는 내용을 전부 읽었을 때`  
      `ajax 에서는 서버에서 응답을 받았을 때`  
      호출되는 이벤트
   - **`beforeunload`**  
     브라우저의 내용이 사라지기 직전
     - 세션을 이용해서 로그인 처리를 하는 경우  
       `로그아웃을 할 때 세션을 초기화`하는데 브라우저 창을 닫아버리면  
       세션 초기화를 하지 못하는데, 이 이벤트를 이용해서  
       **브라우저가 닫히는 시점을 찾아 세션 초기화 가능**
   - change  
     값이 변경될 때
     - 비밀번호 같은 것을 변경할 때 메시지를 출력하는 형태로 이용
   - form
     - submit과 reset 이벤트가 있음
   - scroll  
     모바일에는 터치 이벤트와 방향 전환 이벤트가 있음

# 비동기 처리

1. 동기(Synchronous) 와 비동기(Asynchronous)

   - 동기  
     순차적으로 처리
     - 하나의 처리가 끝나야 다음을 처리하는 형태
   - **비동기**  
     하나의 처리가 끝나기 전에 다른 처리를 할 수 있는 형태

     - `작업을 수행하다가 쉬는 시간이 생기거나(sleep)`
     - `일정한 시간 이상 작업을 하거나(time sharing)`
     - `프로세서를 사용하지 않는 작업(입출력/디크스, 네트워크에서 데이터 받아오기)`

     이와 유사한 형태가 `스레드를 만들어서 처리`하는 것

     - `시간이 오래 걸리는 작업`의 경우  
       작업을 순차 처리하면 뒤의 작업이 너무 오래 기다려야 하고  
       동일한 자원을 사용하지 않는 작업을 순서대로 처리하게 되면 자원 낭비 발생

     `알림을 어떻게 줄 것인가를 고민해야 한다`  
     비동기로 동작한 작업이 끝나고 난 후 작업을 어떻게 할 것인가?

     - interupt / notification  
       (네트워크 - 입출력 장치의 interupt)

   - Callback  
     상태 변화가 생기면 호출되는 함수
     - 비동기 처리는 언제 작업이 종료될 지 알 수 없기 때문에  
       `종료되면 다른 작업을 수행`할 수 있도록 콜백 함수 등록  
       `모든 이벤트 처리는 비동기 방식`
       - 연속된 경우 콜백 안에 콜백 계속 삽입

2. 비동기 처리를 수행해야 하는 경우

   - 입출력 작업
     파일에 읽고 쓰기  
     서버에 요청하고 응답을 받는 것;
     - 느리기 때문
   - 암호화 / 복호화 작업  
     오랜 시간이 걸리기 때문에 비동기적으로 처리하는 것을 기본

3. Promise  
   **연속된 콜백** 사용할 경우의 가독성 문제와 콜백 메서드 안에서의 예외 처리 해결을 위해

   - 비동기 처리를 승핸한 후 다음 작업 진행시 콜백 요청
   - 가독성 떨어질 수 있음  
     callback()을 계속 만들어 줄 수 있음
   - 예에와 처리의 한계

4. Promise 사용

```javascript
const promise = new Promise((resolve, reject)=>{
 //비동기 작업 수행
 if(비동기 작업 성공했다면);
 resolve;
}else{
  rejectl
}
```

- chainning(연쇄적으로 동작) 가능
  then 함수에 성공했을 때 수행할 작업을 연속 지정 가능

```javascript
Promise.then(수행 작업).then(수행 작업).,...catch(에러 수행 작업)
```

- then의 개수는 무제한이지만 catch는 1번만 가능
  앞에서 수행한 작업의 리턴되는 데이터가 운동 수행할 작업의 매개변수로 전달

5. async/await  
   가장 최근에 등장한 콜백 처리 방식으로 가독성을 높일 목적으로 추가

   ```javascript
   async function 함수이름(){
    변수 = await 비동기로 동작할 코드
    비동기 코드가 작업을 완료하면 수행할 코드
    }
   ```

   `async와 await는 반드시 쌍으로 등장해야 한다`

# Communication

최근의 프론트 앤드 변화

- 모바일에서 접속을 많이 하고 사용자에게 가까운 쪽에서 많은 것을 하게 함
- 모바일은 `언제든지 네트워크에 변화가 생겨서 `**서버와의 연결**`이 끊어질 수 있다.`
  1. 화면 전환 최소화  
     Single Page Application이 대안으로 떠오름  
     하나의 화면에 2개 이상의 컨텐츠를 보여 주어야 한다
  2. 로컬에 저장해서 출력  
     예를 들면, 메일

이러한 변화에 맞추어서

1. Ajax(Asynchronous Javascript XML)

   - XML => eXtensible Markup Language

   HTML은 마크업 형태로 작성을 해서 브라우저가 해석  
   HTML에서는 태그의 기능이 고정

   XML은 확장 마크업으로, 태그의 기능을 개발자가 정의하는 것  
   XML의 목적은 서로 간에 데이터를 교환하는 포맷

   `Ajax는 비동기적으로 xml을 주고받기 위한 자바스크립트 기술`  
   **`자바스크립트를 이용해서 비동기적으로 (무슨) 데이터이든 받아오는 것`**

   js에서는 ajax를 `XMLHttpRequest 객체를 이용`해 구현

   - ajax는 예전에 나와 가독성이 떨어져, **Fetch API 추가**

   - HTML5에는 **Server Sent Events(Web Push)**와 **Web Socket API 추가**

   기본적으로 클라이언트와 서버의 통신 방식은  
   클라이언트가 서버에게 요청을 보내면 서버가 응답  
   Push는 클라이언트의 요청이 없어도 서버가 클라이언트에게 데이터 전송

   HTTP나 HTTPS는 데이터 이 외에 `헤더 정보를 같이 전송`하고  
   `한 번 응답이 오면 연결이 끊어짐`  
   그래서, 짧은 메시지를 보낼 때 HTTP나 HTTPS를 사용하면 `오버헤드가 커짐`

   - 오버헤드 => 정말로 하려고 하는 일을 제외한 모두

   상태 유지가 안되기 때문에 `쿠키 와 세션`을 이용해서 상태 유지를 했는데  
   이를 위해, 연결형 서비스이고 `오버헤드가 작은 Web Socket을 추가`함

   **작업 순서**  
   XMLHttpRequest 객체 생성

   서버에게 보낼 데이터를 준비

   서버에게서 응답이 왔을 때 처리할 콜백 함수를 등록하고 그 안에 코드 작성

   open 메서드를 호출해서 연결 요청 준비

   send 메서드를 호출해서 요청을 전송

   **XMLHttpRequest 의 속성**

   readyState

   - 상태를 나타내는 속성으로  
     0이면 객체 생성 직후  
     1이면 open()을 호출  
     2이면 send()를 호출  
     3이면 서버에서 응답이 오기 시작한 상태  
     4이면 응답이 완료된 상태

   **`status`**

   - 서버에서 응답을 보냈을 때 응답의 상태  
     100번대이면 처리 중  
     200번대이면 정상 응답  
     300번대이면 redirect 중
     - 처리는 끝났고 응답을 전송하고 있는 경우
       400번대이면 클라이언트 오류 - 401-권한 없음
     - 402 - 요청한 url을 처리할 수 없음
       500번대이면 서버 오류

   responseXML  
    서버가 XML로 전송한 경우 XML 데이터

   - XML 파싱 해서 사용

   responseText  
    서버가 XML 이외의 형식으로 전송한 경우의 문자열

   - JSON 데이터 일 경우 JSON 파싱 해서 사용

   **XMLHttpRequest 의 메서드**

   - abort()  
     요청 취소

   - open(요청 방식, 요청 url, 비동기 전송 여부)  
     연결 요청 준비

   - send(데이터)  
     요청 전송

   - setRequestHeader(인자, 값)  
     헤더 설정하는 것으로 인증(로그인)에서 중요

   - sendAsBinary(데이터)  
     요청 전송하는 것으로 파일 업로드할 때

   **XMLHttpRequest 의 이벤트**

   - abort  
     취소할 때

   - error  
     에러가 발생했을 때

   - load  
     응답이 전부 왔을 때

   **SOP(Same Origin Policy - 동일 출처 정책)**  
    동일한 출처(**등록된 도메인이 같아야 하는데 포트번호까지 확인**)에서  
    불러온 데이터만 사용할 수 있게 하는 브라우저 보안 정책

   - ajax 와 Fetch API에는 적용이 되고  
     img, link, script, video, audio, object, embed 등에는 적용 안 됌  
     `ajax와 Fetch API에서는 동일한 도메인의 데이터만 사용 가능`

   다른 출처의 데이터를 ajax와 Fetch API에서 사용하려면  
    서버에서 **CORS(교차 출처 리소스 공유)** 설정을 하거나

   - 다른 도메인에서 자원을 사용할 수 있도록 해주는 것

   클라이언트 쪽에서 **Proxy**를 이용해야 한다

   - 웹에서 데이터를 받아올 수 있는 언어의 프로그래밍 이용
   - 내부에 데이터를 요청하는 형태로 작성하지만  
     `Application 서버에서 외부로 나가서 데이터를 가져와서 전달하는 방식`
     - react / vue에서는 단순 설정으로 가능

# 데이터 읽어서 파싱하기

# XML 파싱

RSS(Rich Site Summary)에 많이 이용

- 신문사 등에서 실시간으로 변경되는 데이터를 제공하는 용도로 주로 이용

- 태그 형태로 데이터 구성

- 파싱

  - XML 데이터 = ajax객체.responseXML;

- 필요한 태그 추출

```
  태그들 = XML데이터.getElementsByTagName("태그 이름");
```

- 태그 순회하며 작업

```javascript
for (var i = 0; i < 태그들.length; i++) {
  //태그 1개 가져오기
  var 태그 = 태그들[i].childNodes[0].nodeValue;
  //태그 사용
}
```
