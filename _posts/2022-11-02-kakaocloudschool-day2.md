---
title: "HTML 영역과 CSS"
excerpt: "html에서 영역을 나누는 요소인 div, span, iframe에 대하여 학습하고,
시각적인 효과를 부여하기 위한 기술인 css에 대해 학습한다"

categories:
  - Blog
tags:
  - [Blog, kakaocloudschool, develop, html, css]

toc: true
toc_sticky: true

date: 2022-11-02
last_modified_at: 2022-11-12
---

# HTML

1.  영역 태그

    - div  
      블록 요소
      - 기본적으로 좌우에 다른 요소가 배치될 수 없음 - 영역을 만들 때 자주 사용됨
    - span  
      인라인 요소
      - 기본적으로 좌우에 다른 요소가 배치될 수 있음

2.  iframe  
    다른 웹 문서를 가지고 와서 표시하는 요소
    - 속성
      - width
      - height
      - name
      - seamless
      - src: 표시할 문서의 URL

# CSS

HTML 문서에 시각적인 효과를 부여하기 위한 언어

1.  작성 방법

    - 별도의 파일에 작성하고 가져와서 사용(external)
    - HTML 문서 안에 <style></style>을 만들고 작성(internal)
    - 태그 안에 style 속성에 작성(inline)

2.  실행 방식

    - HTML 코드 나 Javascript 코드를 전부 읽어서 구조를 만든 후 적용
    - 스타일시트는 작성 위치에 상관없이 동작

3.  기본 선택자

    - \*
    - tag
    - .class(여러 요소를 그룹화하기 위해서 사용, 대부분 스타일 적용에 사용)
    - #id(문서 내에서 구별하기 위해서 사용, 대부분 자바스크립트에서 사용)

4.  속성 선택자

    - 선택자[속성이름]: 선택자 안에 속성이 존재하는 경우만 적용

    ```
    <a href="https://www.kakao.com">카카오</a>
    <a>카카오</a>
    ```

          a[href]:{}

    - 선택자[속성이름=값]: 선택자 안에 속성의 값까지 일치하는 경우만 적용

      ```
      input[type="text"]{ }
      ```

      - input 태그 중에서 type 이 text 인 경우에만 적용

    - 부분일치
      A^=B: A 속성의 값이 B로 시작하는 경우
      A$=B: A 속성의 값이 B로 끝나는 경우
      A\*=B: A 속성의 값에 B가 포함된 경우

5.  복합 선택자

    - 일치 선택자: 선택자1선택자2...
      공백없이 여러 선택자를 나열하면 모든 선택자가 일치해야 합니다.

      ```
      div.content{ } -> div 태그 중에서 class 속성이 content 인 경우에 적용
      ```

    - 자식 선택자: 선택자1 > 선택자2
      선택자1 에 포함된 선택자2 인데 선택자2가 바로 아래 레벨에 있어야 함

      ```
      <div><span></span></div>
      <div><ul><span></span></ul></div>
      ```

      - div > span 으로 작성하면 **위의 경우**에만 적용

    - 하위 선택자: 선택자1 선택자2
      공백을 이용해서 선택자를 나열
      선택자2 가 선택자1에 포함되어 있으면 적용

      ```
      div span
      ```

      작성하면 위의 모든 경우에 적용

    - 인접 형제 선택자: 선택자1 + 선택자2
      선택자1 과 동일한 레벨에서 **다음에 나오는** 선택자2 에 적용

      ```
      <h1>대제목</h1>
      <h2>제목1</h2>

      <h2>제목2</h2>
      ```

      - h1 + h2 -> 이 경우는 제목1에만 적용

    - 형제 선택자: 선택자1 ~ 선택자2
      h1 ~ h2
      h1 과 **동등한 레벨에 있는 모든** h2에 적용

    - 그룹 선택자: , 로 구분
      , 와 나열을 하게 되면 나열된 선택자 중에 포함되면 적용

6.  pseudo-classes selector

    - 선택자 뒤에 :을 하고 작성

      - link - link, visited

        - a:link ->링크에 모두 적용

        - a:visited -> 링크를 한번이라도 누른 경우 적용

    - 동작  
      hover(마우스가 올라오면), active(마우스로 누르면), focus(input에서 포커스가 오면)

    - UI요소 - checked, enabled, disabled

    - 구조 관련 선택자
      root
      first-child, last-child
      nth-last-child  
      nth-child(n)  
      only-child  
      not(선택자)  
      empty  
      first-line, first-letter  
      nth-of-type(n), first-of-type, last-of-type, only-of-type

    - lang(languagecode): 특정 언어 설정에만 적용
      언어코드2자리-국가코드2자리(ko-KR, en-US...)

    - pseudo-element
      ::first-letter
      ::first-line
      ::before
      ::after
      ::selection

7.  상속

    - 하위 요소가 상위 요소의 값을 물려받는지 여부
    - 속성 별로 다르게 적용되는데 폰트 관련된 속성은 대부분 상속됨
    - `어떤 속성이 상속되지 않는 경우` 상속을 받고자 하면 `inherit` 로 값을 설정하면 됩니다.

8.  우선 순위

    - 하나의 요소에 동일한 속성의 값을 2개 이상 적용하는 경우 충돌이 발생
      `마지막에 적용된 요소`가 우선 순위를 갖음
      `태그 안에 작성한 style` 이 **우선 순위**가 가장 높음
      외부 파일에 작성한 것 과 <style> 태그 안에 작성한 것은 나중에 작성한 것이 우선 순위가 높음

    - 동일한 방식으로 작성된 경우는 **특정도**를 가지고 우선 순위를 적용합니다.
      inline - 1000
      id 선택자 - 100
      class 선택자 - 10
      가상 클래스 - 10
      가상 요소 - 1
      태그 선택자 - 1

9.  단위

    - 절대 단위: 불변의 단위

      cm
      mm
      in

    - px: 픽셀로 1/96 인치인데 해상도 같은 것을 표현할 때 사용하는 단위 - 1920 \* 1024 라고 하면 가로로 점을 1920 개 찍을 수 있고 세로로 점을 1024개 출력할 수 있다라는 의미입니다.

      `화면의 확대 축소에 따라 변하기도 하기 때문에 상대 단위라고 하기도 합니다.`

    - pt: 1/72 인치
      pc: 12pt

    - 상대 단위: 화면 크기 나 디바이스 크기에 따라 다르게 적용
      px
      %
      em: font-size 가 기준, 글꼴 크기가 16px 이면 1em 은 16px, rem 은 최상위 요소의 글꼴 크기
      - 주변의 글자 보다 크기가 1.5배가 되도록하고자 할 때는 1.5em
    - vw(화면의 가로 크기를 100으로 설정), vh(화면의 세로 크기를 100), vmin(가로 나 세로 중 작은 것을 100), vmax(가로 나 세로 중 큰 것을 100)
    - ex: 소문자 x 의 높이로 em 의 절반
    - ch: 숫자 0의 너비

    - 각도
      deg
      rad: 라디안

    - 요즘은 디바이스 크기가 다양하기 때문에 화면 출력을 할 때는 상대 단위를 사용하는 것을 권장하고 `인쇄를 할 때`는 절대 단위를 사용하는 것을 권장

10. Typography  
     문자 나 기호에 적용

    1. font-family

       - font-family: 폰트 나열
         폰트가 없을 때 다른 폰트를 적용하기 위해서 나열
         font-family: 돋움, sans-serif
         돋움이 없으면 sans-serif를 적용

    2. @font-face

       - 폰트가 없을 때 다운로드를 받을 수 있도록 해주는 속성

       ```
       @font-face{
       font-family:글꼴 이름
       src:url(글꼴 파일의 경로) format(파일 유형)
       }
       ```

    3. font-size

       - 글꼴 크기

       - 키워드(xx-small, x-small, small, medium, large, x-large, xx-large, smaller, larger..)로 설정할 수 있고 직접 단위 설정 가능

         - `em 단위로 설정하는 것을 권장`

    4. font-weight

    - 글자 두께

    - 100 부터 900 까지 100 단위로 설정 가능하고 normal, bold, bolder, lighter 와 같은 키워드 설정 가능

    5.  font-style

        - italic을 설정하면 기울임

    6.  font-variant

        - small-caps 를 설정하면 소문자를 작은 대문자로 변형

    7.  font

        - `앞의 6가지를 한꺼번에 적용`하기 위해서 사용

        font: weight style variant size line-height font-family
        다른 모든 속성은 생략이 가능하지만 `font-size 와 font-family는 생략 불가`

    8.  color

        - 요소의 전경 색상 - 대부분 글자에만 적용됨
        - 키워드로 설정할 수 있고 **#16진수 6자리**, **rgb(0-255까지의 숫자 3개 나열)**, **3개 숫자 대신에 백분율**로 설정할 수 있고 **rgba를 사용하면 투명도 설정** 가능하고 **hsl** 도 있음
        - 색상 키워드: https://www.learningwebdesign.com/colornames.html
        - 색상 추출: https://www.webfx.com/web-design/color-picker

          - vscode 에서는 색상 추출 기능을 코드 센스로 제공

    9.  text-decoration

        - 밑줄이나 취소선 등의 효과를 설정
        - none, underline, overline, line-through, blink 등으로 설정
        - a 태그를 이용해서 버튼 효과를 나타낼 때 none 으로 설정하는 경우가 있음

        요즈음은 a 태그에 밑줄을 긋는 것 보다는 `색이나 두께를 변경해서 알려주는 것을 권장`
        일반 텍스트에는 underline을 하지 않고 강조를 하고자 하면 `italic으로 기울임을 설정`하는 것을 권장

    10. text-transform

        - 영문의 대소문자 변환을 설정
        - none, uppercase, lowercase, capitalize 등을 이용

    11. white-space

        - 공백 문자 설정  
          normal: 여러 개의 공백을 하나로 처리  
          nowrap: 여러 개의 공백을 하나로 처리하고 영역 너비를 넘어가면 줄 바꿈하지 않고 한 줄로 표시  
          pre: 여러 개의 공백을 그대로 처리하고 영역 너비를 넘어가면 줄 바꿈하지 않고 한 줄로 표시  
          pre-wrap: 여러 개의 공백을 그대로 처리하고 영역 너비를 넘어가면 줄 바꿈해서 표시  
          pre-line: 여러 개의 공백을 하나로 처리하고 영역 너비를 넘어가면 줄 바꿈해서 표시

11. Paragraph  
     문단 관련 속성

    1. text-align
       문단의 가로 정렬

       - 셀이나 인라인 요소에 적용할 때는 `내용보다 너비가 더 커야` 설정됩니다.

         - start, end, left, center, right, justify(문단의 시작을 왼쪽에 끝을 오른쪽에 맞추고 여백을 조정)

    2. text-justify

       - text-align 에 `justify를 적용했을 때` 공백 조절
         auto: 웹 브라우저가 조절
         none: 정렬하지 않음
         inter-word: 단어 사이의 공백을 조절
         distribute: 글자 사이의 공백을 조절

    3. text-indent
       첫 줄 들여쓰기

       - 양수를 설정하면 들여쓰기 이고 음수를 설정하면 내어쓰기

    4. letter-spacing
       문자 사이의 간격

    5. line-height
       문단의 행 사이의 간격

    6. word-break
       줄바꿈 옵션

       - keep-all 을 설정하면 단어 단위 줄바꿈을 적용

    7. direction

       - rtl을 설정하면 오른쪽에서 왼쪽으로 출력

    8. vertical-align  
       인라인 요소끼리의 세로 위치를 설정

       - sub, super, top, text-top, middle, bottom, text-bottom
         `이미지 주위에 텍스트를 배치`할 때 많이 이용

    9. text-shadow

    글자에 그림자 효과

    - css3에서 추가된 속성이라서 구형 브라우저에서는 적용이 안됨
    - 수평 오프셋, 수직 오프셋, 흐릿해지는 반경, 색상 순으로 설정

      - 수평 오프셋 과 수직 오프셋이 일치하면 하나만 설정

    `속성을 나열한 후 ,를 하고 다시 설정하면 여러 개 적용이 가능`

12. list

    1. list-style-type  
       목록의 마커 설정

       - none, disc, circle, square, decimal, decimal-leading-zero, upper-alpha, lower-alpha, upper-roman, lower-roman, upper-latin, lower-latin, lower-greek, armenian, georgian, katakana, hiragana

    2. list-style-image

       - 이미지 파일을 마커로 사용
         url(이미지 파일의 경로)

    3. list-style-position
       - 마커의 위치  
         inside 와 outside 를 설정할 수 있음

`list-style에 3가지를 동시에 설정 가능한데 이 경우는 type, position, image 순으로 작성`

14. background

    1. background-color

       - 배경색으로 color 와 같은 방식으로 설정

    2. background-image

       - 배경 이미지를 설정하는 것으로 url(경로)
       - ,를 이용해서 여러 개 적용이 가능한데 순서대로 적용이 됩니다.

    3. background-repeat

       - 이미지의 반복을 설정
       - 이미지가 배경보다 작을 때 적용
       - repeat, repeat-x, repeat-y, no-repeat 설정 가능

    4. background-position

    - left, right, center, top, bottom, 직접 숫자 입력 가능

    5. background-attachment

       - 스크롤 할 때 이미지의 이동 여부
       - fixed를 설정하면 이미지 고정이 되고 scroll을 설정하면 배경 이미지도 스크롤

    6. background-size

       - 배경 이미지 크기  
         auto: 원본 이미지 크기 그대로 출력  
         숫자 2개: 너비 와 높이  
         숫자 1개: 너비 설정이고 높이는 auto  
         **cover**: 너비 와 높이 비율을 맞추어서 확대하거나 축소하는데 큰 값을 적용  
         **contain**: 너비 와 높이 비율을 맞추어서 확대하거나 축소하는데 작은 값을 적용

    7. background-clip

       - 적용 범위  
         border-box: 테두리까지 적용  
         padding- box: 테두리 제외  
         content- box: content 에만 적용

    8. background

       - color, image, 반복여부, position, attachment를 한꺼번에 적용하기 위한 속성

15. gradation  
    css3 에서 지원하는 것으로 여러 색상을 혼합해서 사용하는 속성

    1. vendor-prefix

       - css3 의 기능 중에는 `표준으로 채택되지 않아서` 브라우저 별로 별도로 기능을 제공할 때 사용하는 기호  
         사파리나 크롬은 -webkit- 을 추가  
         파이어폭스는 -moz- 를 추가  
         MS의 브라우저는 -ms-  
         오페라 브라우저는 -o- 를 추가
       - css3 는 현재도 표준을 계속 추가하고 있기 때문에 vendor prefix를 이용하는 기능 중에는 최신 브라우저를 사용하면 vendor-prefix를 생략해도 되는 경우가 있음

    2. linear-gradation  
       선형 그라데이션 - linear-gradient(각도, 색상값을 나열) - vendor prefix 이용

    3. radial-gradation  
       원형 그라이데이션

       - radial-gradient(시작점의 위치, circle의 모양, 색상 나열

`그라데이션을 직접 생성하는 것은 쉽지 않아서 http://www.colorzilla.com/gradient-editor 에서 모양을 만든 후 코드를 복사해서 사용하는 것이 편리`

16. Box Model  
     영역에 대한 설정

          - width: 콘텐츠 영역의 너비
            height: 콘텐츠 영역의 높이
            border: 경계선
            padding: 경계선 과 콘텐츠 사이의 여백
            margin: 영역 과 영역 사이의 여백

    1. IE 의 호환 모드
       - IE 의 하위 버전들은 padding을 width 와 height 에 포함시켜서 계산
         width: 100px
         height: 100px
         padding: 30px

`일반 브라우저들은 160px을 확보해서 표시하지만 IE 하위 버전은 100px 만을 확보해서 표시`

    2. margin collapsing(마진 겹침)

      - 연속배치된 요소들에 마진을 전부 설정하면 마진은 중복 적용되지 않고 큰 값 1개만 적용

    3. box-sizing

      - box 의 크기를 설정할 때 기준을 정하는 것
      - content-box를 설정하면 내용을 기준으로 box의 크기를 설정하고 border-box를 설정하면 경계선을 기준으로 함

    4. 크기 설정

      - width 와 height를 가지고 설정
      - max-height, min-height, max-width, min-width를 이용해서 최대 및 최소 크기를 설정하는 것도 가능

    5. 여백 설정

      - padding 과 margin으로 설정
      - padding 이나 margin에 하나의 숫자를 설정하면 좌우상하에 동일하게 적용
      - 상하좌우 여백을 따로 설정하고자 하면 \-left, \-right, \1-top, \-bottom을 이용해서 설정

    6. visibility

      - `요소의 내용을 숨기거나 표시하고자 할 때 사용하는 속성`
      - visible, hidden, collapse 로 설정

    7. overflow

      - 내용이 영역보다 큰 경우의 옵션 설정으로 visible, hidden, scroll, auto 가 있음

    8. text-overflow

       - `텍스트가 영역보다 긴 경우`의 옵션 설정으로 clip(내용을 잘라서 보이지 않음) 과 ellipse(... 을 표시) 를 설정할 수 있음
       최근에는 텍스트가 영역보다 큰 경우 **더보기 버튼** 같은 것들을 만들어서 출력하는 경우가 많습니다.
       페이스북 앱은 내용이 많은 경우 ... 과 더보기 기능을 제공해서 더보기를 누르면 아래로 펼쳐지는 형태로 디자인을 하는데 이러한 디자인을 **카드형 레이아웃**이라고 부름

    9. floating

      - 블록 요소 주위에는 다른 요소가 배치될 수 없습니다.
      - `블록 요소 주위에 다른 요소를 배치`하고자 할 때는 **블록 요소를 inline 요소**로 변경하거나 **float 속성**을 이용해서 영역은 차지하지만 공중에 떠 있는 형태로 만들어서 할 수도 있습니다.
      - float 속성에는 left, right, none을 설정할 수 있습니다.
      인라인 요소에도 float 속성을 설정할 수 있는데 인라인 요소에 float을 적용할 때는 width 와 height를 설정하는 것이 좋은데 설정하지 않으면 콘텐츠를 표시하는 영역이 최대한 확장이 되버림
      - float 속성이 적용된 상태에서 해제
        - clear 속성에 none, left, right, both를 설정해서 해제
        블록 요소에서만 가능
        - overflow 속성에 auto 나 hidden을 설정해서 해제
        부모 요소에 설정
        - float 속성을 부모 요소에 설정해서 해제

    10. 크기 조절

      - resize 속성에 horizontal 이나 vertical, both를 설정해서 크기 조절이 가능하도록 할 수 있음

    11. 그림자 설정

      - box-shadow: 수평오프셋 수직오프셋 흐릿함 과 확산 정도 색상

`수평 오프셋 앞에 inset 을 설정하면 그림자가 내용 안으로 출력됩니다.`

    12. 경계선

      - border-style
      경계선의 모양으로 none, dotted, dashed, solid, double, groove, ridge, inset, outset 을 설정
      그냥 사용하면 상하좌우 경계선이 모두 동일한 모양이 되고 방향 별로 다르게 설정하고자 하면 border-left-style, border-right-style, border-top-style, border-bottom-style로 설정

      - border-width
      경계선의 두께로 thin, medium, thick 으로 설정할 수 있고 값 과 단위를 직접 지정할 수 있습니다.
      방향 별로 설정할 수 있음

      - border-color
      경계선의 색상

      - border: 두께 종류 색상을 한꺼번에 설정

      - border-radius
      경계선에 라운드 효과를 설정하는 속성
      css3에서 추가된 속성
      값을 하나만 설정해서 상하좌우 모두 동일한 값으로 적용할 수 있고 숫자 4개를 설정해서 상하좌우를 다르게 설정하는 것도 가능하고 ,로 구분하고 다시 설정해서 여러 개의 값을 적용하는 것도 가능

      - 이미지 보더
      보더에 이미지 설정 가능
        - border-image-source:url(이미지 파일 경로) 로 설정
        border-image-slice: 값 4개 로 잘라내기 설정
        border-image-repeat: stretch, repeat, round, space 중 하나의 값을 설정해서 반복 여부를 설정
        border-image-width:숫자 로 두께 설정
        border-image-outset: 숫자 로 경계선 과의 거리를 조정

      - display
      박스의 보기 모드를 변경
        - block: 블록 요소로 만들어 짐
        - inline: 인라인 요소로 만들어지는데 height 가 무시됨
        - inline-block: 인라인 요소로 만드는데 주위에 콘텐츠가 배치되지 않고 height가 적용됨

      - opacity
      불투명도 설정으로 0 과 1 사이의 소수로 설정하는데 1이면 100% 불투명이 되고 0이면 투명

      - position
      요소의 배치 방법을 설정하는 것으로 기본값은 static

        - static: 위치 기준이 없는 것으로 순서대로 배치하는 것인데 left 나 top을 사용할 수 없음
        - relative: 이전에 출력된 내용과의 관계를 이용해서 출력되는데 이 경우에는 left 나 top을 이용해서 이전 내용과의 거리를 설정
        - absolute: 부모의 왼쪽 위를 기준으로 해서 배치되는 것으로 left 나 top을 이용해서 부모에서의 위치를 설정
        - fixed: 웹 브라우저의 스크린 기준으로 배치되는 방식
        - sticky: 스크롤 영역을 기준으로 배치

`absolute 나 fixed 를 설정하면 그 요소의 display는 block으로 자동 변경됨`

      - 위치
      left, right, top, bottom 을 이용해서 위치를 설정

      - 겹쳐서 출력할 때 순서 설정
      z-index 속성을 이용하는데 숫자가 클수록 위에 배치가 됩니다.

17. display:flex  
    하나의 컨테이너를 생성해서 요소들을 가로나 세로 방향으로 배치하는 레이아웃

    - `모바일 웹에서 가로 방향으로 요소들을 배치해서 가득 채우고자 할 때 사용`
    - 정렬 방법이나 크기 방법등을 설정해서 사용

18. 다단

    - 가로 화면을 여러 개로 분할해서 콘텐츠를 배치하는 것으로 **모바일에서는 가독성 때문에 잘 사용하지 않습니다**.
      모바일은 가로는 가득 채우고 세로로 스크롤 할 수 있도록 만들기 때문에 내용이 많으면 `세로 방향으로 스크롤` 하도록 해서 출력합니다.

19. Table

    1. Table 관련 display 속성

       - table, inline-table, table-row, table-row-group, table-header-group, table-footer-group, table-column, table-column-group, table-cell, table-caption, list-item 등이 있습니다.
       - 예전에 서버가 데이터를 xml로 주는 경우가 많았는데 `xml로 전송된 데이터를 표 형태로 출력`을 할려고 하면 `데이터를 파싱해서 객체로 변환`한 후 **table 태그**를 이용해서 출력을 했는데 이를 편리하게 하고자 할 때 **display 속성**을 이용했습니다.

    2. table-layout

       - fixed를 설정하면 셀의 너비가 고정되고 auto를 설정하면 셀의 너비가 셀의 내용에 따라 변경됨

    3. caption-side

       - 캡션의 위치를 설정하는 것으로 top 과 bottom 을 설정할 수 있습니다.

    4. border-collapse

       - 테두리 관련 속성으로 기본값은 **separate** 인데 이 값은 표 와 셀의 테두리가 구분되는 것이고 **collapse** 로 설정하면 표의 테두리 와 셀의 테두리가 한 묶음으로 만들어짐

    5. border-spacing

       - 테두리의 간격으로 border-collapse 가 separate 일 때 적용

    6. 셀 안에서의 정렬

       - 상하 정렬은 vertical-align 속성에 baseline, top, middle, bottom 으로 설정
       - 좌우 정렬은 text-align 속성에 left, right, center, justify 로 설정

    7. empty-cells

       - 빈 셀의 표시 여부로 show 로 설정하면 빈 셀이 보이고 hide로 설정하면 빈 셀이 보이지 않음

20. cursor  
    커서 모양을 변경하는 것으로 url(이미지 파일 경로)로 설정할 수 있고 keyword를 이용해서 설정하는 것도 가능

21. outline  
    외곽선 속성으로 border 와 동일한 방식으로 설정 가능한데 color 에 invert 를 이용해서 반전 효과를 만들 수 있음

22. 변환(Transform - 행렬을 이용) 과 애니메이션

    1. 2D 변환

       - 이동: 덧셈
         translate(x 축 이동값, y축 이동값)
         translatex(x 축 이동값)
         translatey(y 축 이동값)

       - 크기 변환: 곱하기
         scale(가로 비율, 세로 비율)
         scalex(가로 비율)
         scaley(세로 비율)

       - 회전
         rotate(각도 deg)

       - 비틀기
         skew(x축, y축)
         skewx(x축)
         skewy(y축)

       - 한꺼번에 적용: matrix(scaleX, skewX, skewY, scaleY, translateX, translateY)
       - 변환 기준점 설정: transform-origin
