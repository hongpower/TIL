# Java Script(JS)

> 실행과 제어 등 여러 기능을 구현하기 위해서 사용하는 프로그래밍 언어다. 

- html과 css는 만들어지면 끝이라서 정적인 느낌이 있지만, java script를 통해서 동적인 기능까지 구현 가능하다.



## 기초 :

- head에 :
  - `    <script type="text/javascript">`    
  - 스크립트 이렇게 열고 여기에 내부 작성 방식인 경우에는 함수작성
  - ``</script>`
  - 외부 호출 방식이라면 또 다른 script열고
  - `<script type="text/javascript" src="경로"></script>`
  
- `<li onclick="">` 

- onclick = "alert('inline방식');" 클릭하면 "inline방식"이라는 alert를 준다
  - onclick="javascript : " 구글링 하다가 이렇게 작성되어있으면 옛날 버전의 코드라는 뜻

- 파이썬의 def => javascript의 function

- 파이썬의  : =>  javascript의 {} ; scope(영역)작성

- 변수 선언 : __var__ 변수명
  - var doc = document.getElementsByTagNameNS("li")[1];
  - doc변수는 li의 1번째 인덱스(0부터 시작)의 element를 가져온다
  
  

## 변수

- JS는 global 선언이 필요없다 ; 지역변수가 함수내에 없으면 자동으로 전역 불러옴

#### 전역변수

- script구문이 여러개고, head는 body든 어디에 위치하더라도 모든 script구문 내에서 전역변수는 사용가능하다 ; 물리적으로 script구문들이 떨어져있어도 연결되어있다고 생각하자!

#### 지역변수

- 함수내에서 정의한 지역변수라면 새로운 script에서 작성하면 실행 불가능이다.
- 즉 지역변수가 있는 함수가 호출되어야 그 지역변수가 생성되어서 닿을 수 있다

- new 함수() : new는 해당 함수의 객체이다
- JS에서는 function도 data type중 하나이다.

- JS는 변수에 값(변수 = 값)이 들어오면 알아서 받고 무슨 타입이라고 '타입추론'을해서 그 정보를 갖고 있다. 
  - typeof() 실행하면 추론한 타입을 뱉어준다?



## Alert

- windows 객체 메소드들: alert, confirm, prompt
- 메소드를 작성할 때 windows는 생략 가능하다
- alert는 window객체가 가지고 있는 함수기 때문에 만약 `function alert()` 를 작성하면 함수를 정의하는게 아니라 윈도우 객체 alert()라 인식해서 alert의 앞이나 뒤에 다른 이름을 붙여줘서 컴퓨터가 구분할 수 있게해주자.

#### if문

```html
<body>
if (조건구문) {
    alert("실행문 예시")
    } else if (조건구문) {
    실행문
    } else if (조건구문) {
    실행문
    } else {
    }
</body>
```

- 파이썬과는 다른 점은 : 이거대신 {}이거 쓰고, 조건구문에 ()가 들어간다는 점, elif대신 else if쓴다는 것

- confirm()

  - 확인 -> true return
  - 취소 -> false return

- prompt()

  - prompt(a, b):  b="3" 작성하면 디폴트로 3이 작성되어있음
  - 확인 -> 텍스트 return
  - 취소 -> null return

  



