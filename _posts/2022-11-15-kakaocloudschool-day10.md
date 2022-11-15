# HTML5

...이어서

1. **Geo Location**

- 개요  
  디바이스의 물리적 위치 정보를 파악하기 위한 JavaScript API
- 방법

  - GPS 같은 위성 정보를 이용
  - 가까운 라우터의 위치
  - 기지국의 위치

  `사용에 있어서, 위치 정보 사용 허용 필수`

- 위치 정보 사용 가능 여부 확인
  ```javascript
  navigator.geolocation; // 값 확인
  ```
- 위치 정보 가져와서 한 번만 사용하기

  ```javascript
  navigator.geolocation.getCurrentPosition(성공했을 때 함수, 실패했을 때 함수, 옵션)
  ```

  - 성공했을 때 호출되는 함수  
     매개변수로 위치 정보와 관련된 객체 전달

  `이 정보는 Javascript 뿐 아니라 모바일 API에서도 동일`

          1) coords
            - latitude
              위도
            - longitude
              경도
            - altitude
              고도(GPS가 아니면 없음)
            - accuracy
              정확도
            - altitudeAccuracy
              고도의 정확도
            - heading
              방향
            - speed
              속도
          2) timestamp
            위치 정보를 가져온 시간

  - 실패했을 때 호출되는 함수  
    매개변수로 에러 객체 전달되고, 속성에 실패한 이유 포함

- 위치 정보를 계속 가져와서 사용하기
  ```javascript
  let 변수 = navigator.geolocation.watchPosition(성공시 호출 함수, 실패시 호출 함수, 옵션) //위치 정보 지속 확인
  clearWatch(변수) // 위치 정보 가져오기 중지
  ```
- 옵션  
  객체 형태로 대입

  ```javascript
  {
    enableHighAccuracy; // 정확도가 높은 위치 정보 사용
    timeout; //일정 시간이 지넨 데이터 폐기 > 밀리초 단위
    maximumAge; // 0을 설정하면 항상 최신의 데이터 > 위치 정보 갱신 주기
  }
  ```

  `스마트폰 이용 시 옵션 설정이 굉장히 중요`

  - 블루투스 사용 과 GPS 가 가장 배터리를 많이 먹기 때문

- 웹 화면에 현재 위체에 해당하는 카카오 맵 출력
  - KaKao Open API  
    https://developers.kakao.com
  - Key 발급  
    애플리케이션 생성하고 js키 복사
    - 키는 플랫폼 등록 후 사용 가능  
      네이티브 앱은 패키지 이름 등록 후 사용 가능  
      웹은 도메인 등록 후 사용 가능

2. **File API**

- 개요  
  파일을 읽고 쓰기 위한 API
- 방법  
  input type="file"에 multiple 속성이 추가되어 여러 개 파일 선택 가능

  - 일반 파일을 읽을 때  
    `이미지 미리보기`
    ```javascript
    FileReader 객체 생성
    reader.readASdATAURL(파일 객체) 호출
    load 이벤트 //전부 읽었을 때 FilerReader 객체의 result에 저장
    error 이벤트 //읽기에 실패햇을 때 실패 이유를 저장한 객체 리턴
    ```
  - `텍스트 파일을 읽을 때는 인코딩 설정 주의`

3. **Drag And Drop API**

- 개요  
  브라우저 내에서 사용할 수도 있고 외부 프로그램 과 브라우저 사이에서도 사용
  - 외부 프로그램과 사용  
    `외부 프로그램에서 드래그를 하고 브라우저에서 드랍`
    - 파일 첨부
    - 메일 작성 등의 특수 사항

4. 브라우저에 데이터 저장

- 이유

  - 불필요한 트래픽 감소
    - 메일 앱 접속시 매번 서버의 데이터를 받아오는 건 자원의 낭비
    ```javascript
    if(파일의 존재 여부 확인){
    맨 처음 접속
      데이터 다운로드
    }else{//맨 처음 접속이 아닐 경우
      //파일마다 업데이트 시각 작성 필수
      //마지막 업데이트된 시간을 비교
     if(양쪽의 시간이 다르면) 데이터 다운로드
     if(양쪽의 시간이 같으면) 다운로드 X
    }
    ```
  - offline 상태에서도 데이터 사용 가능

- 브라우저 데이터 저장 방법

  - **Web Storage**  
    Map의 형태로 저장
  - **Web SQL**  
    관계형 데이터베이스(SQLite3) 이용
    - 외부 접속 불가한 저용량 DB
  - **Indexed DB**  
    자바스크립트 객체 형태로 저장
    - NoSQL과 유사

기존에는 Cookie를 사용  
문자열만 저장할 수 있고, 전송 여부를 클라이언트가 결정 불가(매번 서버 전송)

- Web Storage

  - LocalStorage  
    브라우저에 저장해서 지우지 않는 한 삭제가 되지 않는 저장소

    ```javascript
    전역 변수 localStorage
    ```

    `보안이 중요하지 않은 많지 않은 양의 데이터 저장에 용이`

    - 장바구니 / 아이디 저장 등에 유용
    - 동일한 패턴의 데이터가 많은 경우 Web SQL 이나 Indexed DB 권장

  - SessionStorage  
    현재 접속 중인 브라우저에 해당하는 저장소
    - 접속 종료 시 소멸
    ```
    sessionStorage
    ```

  ```javascript
  //데이터 `저장` 과 `가져오기` 그리고 `삭제`
  //저장
    스토리지.키이름 = 데이터;
    스토리지["키이름"] = 데이터;
    스토리지.setItem("키이름", 데이터);

  //가져오기
    스토리지.키이름
    스토리지["키이름"] = 데이터;
    스토리지.getItem("키이름");
  //삭제
    delete 스토리지.키이름 = 데이터;
    delete 스토리지["키이름"] = 데이터;
    스토리지.removeItem("키이름");
  ```

  `저장소에 데이터가 변경되면 window 객체에 storage 이벤트가 발생`

  - 이벤트 객체에는 key, oldValue, newValue, url, storageArea 같은 속성 생성
  - `Application > 저장소 항목`에서 저장된 내역 확인 가능

5. Web Worker  
   Javascript를 이용한 백그라운드 처리
   > 현재 작업을 멈추고 다른 작업을 수행할 수 있음
   - `Thread 라는 표현 대신에 Worker 라는 표현을 사용`

- HTML 과 함께 있는 Javascript 코드에서 긴 작업을 수행하게 되면  
  작업이 끝날 때까지 다른 작업을 수행할 수 없음 (UI 아무것도 못함)
- UI 변경, DOM 객체 제어는 못하지만, localStorage 와 XMLHttpRequest 가능
- 생성
  ```javascript
  let 변수 = new Worker("자바스크립트 파일 경로");
  //별도 스크립트 파일에 만들어야 함
  ```
- 워커 와 브라우저 사이의 메시지 전송
  ```javascript
  //워커파일에서
  postMessage("메시지"); // 워커 변수에 message 발생
  //js에서
  워커변수.postMessage("메시지"); // 워커에서는 message 이벤트 발생
  워커변수.onmessage = (event) => {
    event.data;
  }; //워커가 전송한 데이터 사용
  ```
  - sendMessage  
    바로 처리해달라는 요청
  - postMessage  
    다른 작업이 없으면 처리해달라는 요청
- message 이벤트가 발생하면 매개변수에 data 와 error 가진 객체 전달
  - data  
    데이터
  - error  
    발생한 에러에 대한 정보를 가진 객체
- 워커 중지
  ```
  terminate( )
  ```
  - 계산 작업을 대신 수행해주는 워커 생성  
    `별도의 js 파일 생성 필요`
  - 워커 파일 생성

6. Application Cache  
   리소스의 일부분을 로컬에 저장하기 위한 기능

   - 오프라인 브라우징 가능, 리소스 빠르게 로드, 서버 부하 감소
   - CSS 나 JS 그리고 이미지 파일 등을 캐싱
     - CACHE
     - NETWORK
     - FALLBACK

7. Web Push  
   Server Sent Events

   - 클라이언트의 요청이 없어도 서버가 클라이언트에게 메시지를 전송
     - 알림
       - APNS(Apple Push Notification Service)  
         Apple Server 가 보내는 Push
       - FCM(Firebase Cloud Message)  
         Google Server 가 보내는 Push

8. Web Socket  
   Web 에서의 TCP 통신을 위한 API
   - 일반적인 Web 요청 처리 방식
     ```javascript
     Client -> Server 하나의 reqeust 전송
     처리 후 response 를 Client 에게 전송 후 접속 종료
     ```
   - 연속되는 작업 처리 방식  
     Cookie(클라이언트의 브라우저에 저장) Session(서버에 저장) 사용
   - 일반적인 Web 요청(HTTP, HTTPS)에는 헤더 정보 필요
     - 작은 사이즈의 데이터에는 오버헤드 발생
   - Web Socket에는 헤더가 거의 없기 때문에 오버헤드 최소화
     - 작은 양의 메시지가 빈번하면 ajax, Fetch API 보다 권장

# 마무리

XHTML, CSS, JAVASCRIPT, HTML5  
웹 브라우저에 무엇인가를 출력하고 제어하기 위한 기본 기술

- 화면 출력
  XHTML, HTML5
- 출력 화면 디자인  
  CSS
- 출력 화면 요소 동적 제어  
  JavaScript
- 서버 데이터  
  JavaScript 의 Ajax , Fetch API

웹의 모든 화면을 만들 수 있지만, 길고 비효율적이게 코딩할 수 있는 문제 있음  
대부분 효율이 보장된 간결한 코드로 할 수 있는 프레임워크 / 라이브러리 사용

- jQuery  
  속도 느리나, 쉬운 크로스 브라우징, 다양한 플러그인으로 UI 쉽게 구현
- bootstrap  
  반응형 웹 구현 라이브러리로 모바일 웹 구현에 좋음
- react, vue  
  플로그인으로 다양한 기능 제공 및 쉬운 SPA 구현 도와주는 라이브러리
- axios  
  서버에서 데이터 받아오기 쉽게 구현
