---
title: "반응형 웹의 정의와 자바스크립트의 정의와 규칙, 데이터와 연산자"
excerpt: "반응형 웹이 대한 이론적인 내용을 학습하고,
자바스크립트가 무엇인지 어떤 규칙으로 어떻게 쓰고 어떤 데이터와 연산자를 가지고 있는지 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, responsive web, javascript]

toc: true
toc_sticky: true

date: 2022-11-04
last_modified_at: 2022-11-12
---

# 반응형 웹

`화면의 크기에 따라 콘텐츠의 배치를 다르게 해서 동일한 콘텐츠를 사용할 수 있게 디자인한 웹`

flex와 grid 속성을 사용해 편의성 있게 구현할 수 있다

- 방법

  - css의 media query 사용
    @media () and ()
  - meta 태그의 viewport 속성
    width : device-width ..
  - bootstrap과 같은 외부 라이브러리 사용

# JavaScript

**인터프리터** 형식으로
`줄 단위로 번역`하면서 실행하고
`아래쪽에 에러가 있더라도 그 전까지 실행`

- 웹 페이지의 동적 처리를 향상시키기 위한 목적으로 탄생한 언어 - 넷스케이프 사에서 개발  
  문법적으로는 Java 와 아무런 관련이 없고 Java의 몇 몇 클래스를 가져와서 사용
- 기본적으로는 **웹 브라우저 내에서만 동작**하는 **클라이언트 기반의 언어**  
  컴파일 하지 않고 `웹 브라우저에 있는 JavaScript Interpreter Engine 이 해석`해서 실행
- 유니코드를 사용하고 대소문자를 구별

- **동적 바인딩(Dynamic Binding)**  
   실행할 때 메모리의 크기가 결정됨
  - `이름을 만들 때는 메모리 공간 확보를 하지 않고 실제 데이터가 대입될 때 메모리 공간을 확보`
- **객체 기반 언어**  
  클래스를 만들지 않고 `바로 객체를 생성해서 사용`하는 것이 가능  
  최근에는 클래스를 만드는 문법이 추가 - es6, TypeScript
- HTML5 의 기반 언어
- 많은 라이브러리가 존재
  - jQuery  
    크로스 브라우징(하나의 코드로 여러 브라우저에서 수행되도록)을 지원하기 위해 등장
  - express.js, node.js  
    Application Server를 만들기 위한 라이브러리
  - angular, react, vue  
    SPA(Single Page Applicaiton) 구현을 위한 라이브러리
  - d3js  
    그래프 구현을 위한 라이브러리
  - bootstrap  
    반응형 웹 디자인을 쉽게 해주는 라이브러리
  - react-native, ionic  
    모바일 앱 개발을 위한 라이브러리
  - eletron  
    pc 용 앱 개발

`M(Mongo DB)E(Express.js)A(Angular.js)N(Node.js), MER(React.js)N`

**1. 종류**
ECMA 5 - ES5: 2015년 이전의 자바스크립트
ECMA 2015 ~ : ES6 부터 시작하는데 매년 발표, ESNext
ESNext 에 Type 을 추가한 MS 의 TypeScript

**2. 작성**

- 외부에 작성해서 불러들이는 방식

  ```
  <script src="자바스크립트 파일의 경로"></script>
  ```

  - 자바스크립트 파일의 확장자는 js를 주로 사용
  - 타입스크립트 파일의 확장자는 ts를 주로 사용
  - react에서 컴포넌트 파일의 확장자를 구별하기 위해서 jsx 를 사용

- HTML 파일 내부에 스크립트 영역을 만들어서 사용

  ```
  <script>
  내용
  </script>
  ```

- 태그 안에 사용

  ```
  <태그 이벤트="스크립트 코드" ... />
  ```

  - `가독성이 떨어지기 때문에 사용하는 것을 권장하지 않음`

**3. 규칙**  
 HTML 과 JavaScript는 `위에서부터 읽어서 순차적으로 실행`
HTML에 작성된 태그들을 JavaScript에서 사용을 할 때는 **메모리에 로드가 되고 난 이후에 사용**해야 합니다.
`태그가 등장한 다음`이나 `window 의 load 이벤트`가 발생하고 난 후 - 자바스크립트는 줄 단위로 읽어서 번역하기 때문에 한 번에 실행되어야 하는 코드를 한 줄에 작성해야하는데  
 한 줄에 2개 이상의 실행문이 올 때는 이를 구분하기 위해서 ;으로 구분

- 한 줄에 하나의 명령문만 오는 경우는 ;을 하지 않아도 되는데 가독성 과 관습때문에 ;을 하는 것이 일반적

- 주석  
  한 줄 주석은 // 뒤에 작성  
  여러 줄 주석은 /_ 내용 _/ 안에 작성

- 있는 그대로 해석
  ```
  <![CDATA[ ]]>
  ```
  [ ] 안에 작성한 내용은 해석하지 말고 하나의 문자열로 판단
  - 코드 쓸 때 주로 사용

**4. 구성 요소**

- Keyword(예약어)  
  JavaScript에서 의미를 부여한 단어  
  의미 변경이 안됨

- Control Character(제어문자): 프로그래밍 언어에서 \ 다음에 영문자 하나를 추가해서 기능을 부여한 문자

- \n  
  줄 바꿈
- \t  
  탭
- \'  
  작은 따옴표
- \"  
  큰 따옴표
- \\  
  \
- \0  
  null

- Identifier  
  사용자가 의미를 부여한 단어

  - Keyword는 사용하면 안됨
  - 숫자로 시작하면 안되고 특수문자는 \_ 와 $ 만 가능하고 중간 공백 안됨

- Data

  - Literal  
    사용자가 직접 입력하는 데이터

    - 숫자의 경우는 정수 와 실수 형태로 작성

      - 19, 28.4
      - 10진수 형태로 작성

    - 문자열의 경우는 작은 따옴표 나 큰 따옴표 안에 기재  
      'a', "a"...

      - `자바스크립트는 문자 와 문자열을 구분하지 않음`

    - Boolean 의 경우는 true 와 false

    - null  
      가리키는 것이 없음

- Variable(Identifier)  
  데이터에 붙인 이름

  - 일반적인 데이터: 값이나 값들의 모임
  - 함수: 기능을 수행하는 코드의 모임
  - 객체: 데이터 나 함수의 모임

  - 이름을 생성하고 데이터를 대입  
    var(let, const) 이름 = 데이터;

**5. 출력**

- 브라우저 화면에 출력

  - document.write(출력할 내용)  
    **버퍼에 모아서 출력**
  - document.writeln(출력할 내용)  
    바로 바로 출력, 줄바꿈도 해줌
    - HTML에서는 태그를 이용해서 줄 바꿈

- 대화상자에 출력  
  alert(출력할 내용)

- 디버그 콘솔에 출력
  - console.log(출력할 내용)  
    콘솔에 출력하면 브라우저 화면에는 보이지 않고 브라우저의 console 창이나 IDE의 console 창에 보임

**6. Data 의 분류**

- **Immutable Data**(변경 불가능한 데이터 - 여러 곳에서 같이 사용하거나 옵션) 와  
  **Mutable Data**(변경 가능한 데이터)

- **Scala Data**(1개의 데이터) 와  
  **Vector Data**(0개 이상의 데이터 - List 와 Map)

  - 이름에는 하나의 데이터만 매핑할 수 있습니다.  
    `Scala Data 의 경우는 이름이 데이터를 의미`하지만  
    `Vector Data 같은 경우는 이름 이외의 별도의 무엇인가가 추가되어야 데이터를 의미`합니다.

    - [인덱스] 또는 .이름

- 데이터의 모양에 따른 분류

  - 정형 데이터  
    모든 데이터의 모양이 일정
    - class, RDBMS
  - 비정형 데이터  
    데이터의 모양이 일정하지 않은 것
    - Map, NoSQL
  - 반정형 데이터  
    모양은 일정하지 않은 것 처럼 보이지만 정형으로 만들 수 있는 데이터

    - xml, json 등

**7. 변수**  
데이터에 붙이는 이름

- 분류

  - Local Variable(지역 변수)  
    **자신의 영역**(자바스크립트 나 자바에서는 { }을 영역으로 간주) 내에서만 사용이 가능
  - Member Variable(멤버 변수)  
    누군가(클래스 나 인스턴스)에 **소속된** 변수
  - Global Variable(전역 변수)  
    **영역 외부에서 선언**해서 모든 곳에서 사용이 가능

- 선언(생성) 방법

  - 이름;  
    이렇게 만들면 전역 변수
  - var(let 또는 const) 이름;  
    지역 변수 나 멤버 변수

  - 함수 내에서 var 나 let, const 없이 이름을 만들면 함수가 호출된 이후에는 어디서든지 사용
  - var 나 let 이나 const 와 함께 만들어지면 `자신의 영역 내에서만 사용`  
    최근에는 var 나 let 이나 const 없이 이름 만드는 것을 금기시

- 데이터의 참조를 대입  
  이름 = 데이터;

  - 데이터는 리터럴 이나 다른 변수 또는 코드 가능

- 변수를 선언하면서 대입  
  var(let 또는 const) 이름 = 데이터;

  - var 나 let 또는 const 라는 단어없이 새로운 이름을 만드는 것도 가능

- `이름을 만들 때는 기억하기 좋아야 하고 이름만으로 데이터를 예측할 수 있어야 함`

- let  
   var 키워드의 문제점을 해결하기 위해서 등장

  - 동일한 scope 안에서 동일한 이름의 let 변수를 선언한 수 없고 사용만 해야 함

            var x = 1000;
            1000 이라는 데이터의 참조를 x 에 대입

            x =  100;
            100이라는 데이터의 참조를 x에 대입(수정)

              - 동일한 이름을 계속해서 생성하는 것이 가능
                - 실제로는 수정
              - var x = 2000;
                2000 이라는 데이터의 참조를 x 에 대입

            let y = 1000;
            1000 이라는 데이터의 참조를 y 에 대입

            y =  100;
            100이라는 데이터의 참조를 y에 대입(수정)

              - 동일한 이름을 계속해서 생성하는 것이 불가능
              - var y = 2000;
                2000 이라는 데이터의 참조를 y 에 대입

- **hosting이 되지 않음**  
  `이름을 만들기 전에 사용하는 것`

  - var 는 hosting이 가능

- undefined  
  **존재하지 않는 이름**으로 `null 과는 다름`

  - 이름을 만들고 데이터를 대입하지 않거나 이름이 존재하지 않는 경우

        //x 는 var 로 선언되서 만들기 전에 호출하면 undefined
        console.log(x);
        var x = 100;
        console.log(x);

        //y 는 let으로 선언되서 만들기 전에 호출하면 error
        console.log(y);
        let y = 100;
        console.log(y);

- 동일한 블럭에서 let을 이용해서 두 번 생성하는 것은 안되지만 블록이 다르면 가능
  블럭이 변경되거나 새로 만들어지면 이름 공간이 새로 만들어짐

          let data = 10;
          data = 20;
          //기존 데이터가 참조하고 있는 데이터를 수정

          {
              //기존 데이터의 값을 변경하는 것이 아니고 새로운 영역에
              //data를 만드는 것.
              //여러 블럭에 동일한 이름이 여러 개 존재하면
              //자신의 블럭에서 가까운 블럭에 만든 것을 우선
              let data = 2022;
              console.log("data:" + data);//가까운 곳에서 만든 2022
          }
          console.log("data:" + data); //이전 내부 블럭이 없다고 보면 20
          //let 대신에 var를 사용하면 내부 블럭에서 외부 블럭의 데이터를 호출 할 수 있음

**7. const**  
constant 의 약자로 일반적으로 상수라고 함

- **데이터를 변경하지 못하도록**(immutable) 할 때 사용하는 키워드
- 데이터를 변경하지 못한다는 것만 제외하고는 let 과 동일

  - const 이름 = 데이터;  
    이름이 참조하는 데이터를 변경할 수 없음

          const con = 1000;
          con = 2000;
          //constant variable 의 데이터를 변경할 수 없어서 에러

**8. Naming 방법**

- Camel 표기법

  - 일반적인 변수 와 함수는 소문자로 시작
  - 2개 단어 이상의 조합이면 두번째 단어의 시작은 대문자로 시작
  - 클래스 이름은 대문자로 시작

- Snake 표기법

  - 상수는 모두 대문자로 표기  
    const CON = 1000;

- 헝거리언 표기법

  - 이름에 자료형을 표현
    - byte 나 boolean  
      b
    - 정수  
      n
    - 문자  
      ch
    - 실수  
      f
    - 문자열  
      str
      - 성적을 정수로 저장하는 변수: nScore

**9. 데이터 타입(Data Type - 자료형)**  
 `데이터를 어떻게 얼마만큼의 공간에 저장하고 어떻게 읽은 것 인가 하는 문제`

- 숫자(Number)

  - 정수 와 실수를 2진수로 변환해서 저장
  - 실수를 가지고 연산을 할 때는 주의  
    console.log((1.0-0.8) == 0.2); //false

          var sum = 0;
          for(var i=0; i<1000; i++){
              sum = sum + 0.1;
          }
          console.log(sum); //99.999...86

- 문자열(String)

  - 생성  
    ' 나 " 사이에 문자를 나열하거나 new String(' 나 " 사이에 문자 나열)
  - 문자열 내에서 줄 바꿈을 하고자 하는 경우는 \n 을 이용
  - 문자열을 다른 데이터 와 + 연산을 하면 그 데이터의 toString 이라는 메서드를 호출  
    이후 문자열로 변환한 후 결합

- Boolean  
  true 또는 false

  - new Boolean(true 또는 false)

- undefined  
  `이름은 존재하는데 아직 데이터가 설정되지 않았거나 이름이 존재하지 않는 경우`

- null  
  `이름은 존재하는데 가리키는 데이터가 없다`

  - undefined 와 null 은 == 로 비교하면 동일하다고 결과가 리턴

- 데이터의 자료형 확인
  ```
  typeof 데이터
  ```
- 배열(array - list)  
   동일한 자료형의 연속적인 모임

  - `문법적으로는 자료형만 동일하면 되지만 실제로는 비교가 가능한 데이터만 묶어야 함`
    자바스크립트에서는 모든 데이터가 데이터의 참조를 기억하기 때문에 동일한 자료형이라 볼 수 있음

  - 생성
    var(let 또는 const) 배열이름 = [값을 나열];
    var(let 또는 const) 배열이름 = new Array(값을 나열);
    var(let 또는 const) 배열이름 = new Array(정수 1개); //정수 개수 만큼 데이터의 참조를 저장할 수 있는 공간을 확보

  - 배열의 각 요소에 접근

    ```
    배열이름[인덱스]
    ```

    - `인덱스는 0부터 시작해서 데이터개수 - 1까지`

  - 배열이름.length 는 데이터의 개수를 리턴

  - 배열이름.toString() 은 각 요소의 toString()을 해서 **결과를 하나의 문자열로 리턴**

    - 출력하는 메서드에 배열이름을 대입하면 자동으로 toString을 호출

  - valueOf() 나 join(구분문자열)을 이용해서도 내부 데이터를 확인할 수 있습니다.

  - 배열 생성

    ```
    var ar = ["카리나", "지젤", "윈터", "닝닝"];
    console.log("데이터 개수:" + ar.length);
    //모든 데이터를 빠르게 확인
    console.log(ar.toString());
    //출력하는 메서드에 이름을 대입하면 toString()을 호출해서 결과를 하나의
    //문자열로 출력
    console.log(ar);
    //하나씩 출력
    console.log(ar[0]);
    //console.log(ar[4]); //잘못된 인덱스 대입 - undefined
    ```

**10. 데이터의 자료형 변환**

- 문자열을 숫자 나 Boolean으로 변환할 때는 Number(문자열) 또는 Boolean(문자열)  
  다른 종류의 데이터를 문자열로 변경할 때는 toString()을 호출하면 됩니다.

  - 출력을 위해서 문자열로 변환
  - 숫자 나 Boolean 으로 변환하는 이유는 읽어온 데이터가 숫자 나 Boolean 이 아닌 문자열로  
    연산이 안되는 경우 연산을 수행하기 위해

              console.log("23" + "76");
              console.log(Number("23") + Number("76"));

              var input = prompt("숫자를 입력하세요");
              console.log(typeof input);
              console.log(input + 4);

**11. Operator(연산자)**  
연산을 수행해주는 부호 나 기호 및 명령어

- 연산자의 개수에 따른 분류

  - Unary(단항) 연산자  
    데이터(피연산자)가 1개만 있으면 수행되는 연산자
  - Binary(이항) 연산자  
    데이터(피연산자)가 2개 있으면 수행되는 연산자
  - Ternary(삼항) 연산자  
    데이터(피연산자)가 3개 있으면 수행되는 연산자

- 연산의 결과에 따른 분류

  - 산술 연산  
    숫자 자체를 가지고 연산해서 결과를 숫자 형태로 리턴하는 연산자
  - 논리 연산  
    true 와 false 형태로 연산해서 결과를 Boolean 형태로 리턴하는 연산자

- 할당 연산자

  - =  
    왼쪽에는 이름(변수)이 와야 하고  
    오른쪽에는 데이터(리터럴, 변수 나 상수, 계산식, 함수 호출, 함수 생성 구문 등)가 옴
    - 오른쪽 데이터의 참조를 왼쪽의 이름이 가리키도록 해주는 연산자

- 증감 연산자  
  단항 연산자로 **정수 변수**에만 사용이 가능한 연산자

  - ++  
    정수 변수의 데이터를 1증가시키는 연산자
  - \--  
    정수 변수의 데이터를 1감소시키는 연산자  
    연산자의 위치가 데이터의 앞도 가능하고 뒤도 가능

    - 명령문 안에서  
      `앞에 붙은 경우는 증감을 먼저하고 명령에 이용`  
      `뒤에 붙은 경우는 데이터를 명령에 사용하고 증감`

                var data = 20;
                data ++;
                console.log(data); //21
                console.log(data++); //++ 가 뒤에 붙어서 data를 명령문에 사용하고 1증가
                console.log(++data); //++ 가 앞에 붙어서 data를 1증가시키고 명령문에 사용

- 사칙 연산자
  +, - , \*, /

  - %  
    나머지 구해주는 연산자

    - `일정한 패턴을 갖는 작업이나 코드의 유효성을 빠르게 검사하고자 할 때 사용`
      ```
      setInterval(function(){
      for(var i = 0; i<100; i++){
      if(i % 3 == 0)
      document.write("빨강<br/>");
      if(i % 3 == 1)
      document.write("파랑<br/>");
      if(i % 3 == 2)
      document.write("노랑<br/>");
       }
      }, 1000);
      ```

  - \*\*  
    거듭 제곱

- 비교 연산

  - \> , >= , <, <=  
    크기를 비교해서 Boolean으로 리턴, 문자열도 가능

  - ==  
    같다 인데 `자료형은 확인하지 않음`
  - !=  
    다르다 인데 `자료형은 확인하지 않음`
  - ===  
    `자료형까지 일치`해야만 true
  - !==  
    `값이나 자료형 둘 중에 하나만이라도` 다르면 true

    ```
    console.log(1 > 2);
    console.log("ABC" > "BCD"); //문자열의 크기 비교 가능
    console.log(1 == 2);
    console.log(1 == "1"); // == 은 자료형을 확인하지 않기 때문에 true
    console.log(1 === "1"); // === 은 자료형까지 확인하기 때문에 false
    ```
