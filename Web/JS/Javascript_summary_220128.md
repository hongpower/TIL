## Javascript

## 요점 정리

- 식별자 (변수/함수 이름 정할 때 사용하는 이름) : 영문자, 숫자, 언더스코어, 달러 사용 가능. (키워드, 숫자시작 안된다)
- 키워드/예약어 : var같이 미리 예약된 단어들
- 명시적 타입 변환 : parseint(), parsefloat()
- 날짜는 문자, 숫자 둘다 변환 가능한데, getdate()처럼 get은 날짜 중 특정 부분 숫자로 반환



### 배열 :

> array객체로 다뤄진다

- 배열 생성 방법 : 배열리터럴([]), array객체 생성자 이용(Array(,)), new 생성자 활용(객체 초기화까지 가능)
- 만약 new Array(3) 이러면 3개의 요소 가짐
- push()메소드, length 프로퍼티로 추가, 인덱스 활용 추가. 
- 자바스크립트는 arr['하나'] 이런거 불가능. 무조건 인덱스로만
- push(), pop(), shift(첫요소제거), .reverse()
- join()과 slic()는 원본 변경 X. 참조만
- toString(), concat()



### 논리 연산자 :

- &&
- ||
- ! (결과의 반대 반환)



### 프로퍼티 참조 방법:

- 객체이름.프로퍼티
- 객체이름["프로퍼티"]



### 메소드 참조 :

- 객체이름.fuild() => birthday 반환
- 객체이름.fuild => function(){return}... (프로퍼티 그자체 참조)
- 예) var person = {fuild : function(){return this.birthday}}



- prototype :JS의 모든 객체는 prototype이라는 객체가지고 있음
- prototype chain: new Array() 이러면 모든 array 객체들은 같은 프로토타입을 가짐



- prototype property: 새로운 메소드나 프로퍼티 추가 가능

- new Date("02/19/1982"); *// MM/DD/YYYY*

  new Date("1982/02/19"); *// YYYY/MM/DD*

- JS에서 2월은 1월

  - date.setFullYear(1998, 10, 23)
  - date.getFullYear() => 1998
  - date.getMonth() => 10

  

- str.toUpperCase()



### DOM:

- Dom : XML/HTML 문서에 접근하기 위한 인터페이스
- Document객체 : 웹페이지에 존재하는 HTML요소에 접근하고자 할때 연결해줌
  - getElementsByTagName
  - ...byId
  - querySelectorAll(선택자)
  - createElement(HTML요소)
  - document.getElementById(아이디).onclick=function(){}
  - document.getElementsByClassName("odd").style.color = "red"
- 만약 복수라면 for를쓰거나 슬라이싱 해줘야함



HTML의 내용 바꾸기 : innerHTML 속성!!!!!!

- document.getElementById("text").innerHTML = "이 문장으로 바뀜"
- 속성은 이런식으로 접근하는데 그래서 document.getElementById("text").href = "sdffef"



### Node

- node : HTML 문서의 모든 것.

- 문서노드(html문서전체), 요소노드(속성노드가질수잇는노드), 속성노드, 텍스트노드

- 노드 접근 방법

  1. getElementsByTagName() :

  2. 관계 사용 (프로퍼티)

     - parentNode

     - childNodes (근데이거는 텍스트노드까지 저장)

     - childNodes[0] 이렇게 접근가능하겟지? 프로퍼티니까
     - children : 요소만 저장

     - firstChild

     - nextSbiling

- 노드에 대한 정보

  - nodeName !!!!!!! 이 프로퍼티는 항상 태그 이름을 대문자로 저장
    - 텍스트노드의 nodeName property( #text) 
    - 속성노드의 nodeName은 속성이름
    - 요소노드의 nodeName은 태그 이름(대문자)
    - 문서노드의 nodeNmae은 #document
  - nodeValue
    - 요소노드의 nodeValue 는 undefined
    - 속성노드의 nodeValue는 속성값
    - 텍스트노드의 nodeValue는 텍스트 문자열

- 노드 리스트

  - getElementsByTagName()
  - childNodes 
  - 이 윗 두가지 방법으로 반환되는 객체임

- 노드의 추가

  - 부모.appendChild() : 해당 자식의 맨 마지막에 추가
  - 부모.insertBefore(새로운자식노드, 기준자식노드) : 부모 아래의 지정 자식의 직전에
  - insertData() : 텍스트노드의 텍스트 데이터에 새로운 데이터 추가 (바꾸는거아님)

- 노드 생성

  - createElement()
  - createAttribute() : 이미 있는 속성은 대체
    - var newAttribute = document.createAttribute("style")
    - newAttribute.value = "color:red;"
    - text.setAttributeNode(newAttribute)
  - createTextNode()

- 요소 노드의 텍스트는 요소노드의 자식 노드일 뿐이므로 변경하고 싶다면 요소 노드에 포함된 텍스트 노드에 접근

  - var para = document.get....
  - para.firstChild.nodeValue = ''

- 속성 노드값 변경

  - nodeValue 프로퍼티 말고 setAttribute()함수 사용가능
  - setAttribute("속성","값")

- 요소 노드 교체

  - 부모노드.replaceChild("새로운자식노드","기존")
  - replaceData는 텍스트 노드 변경

  
  
  
  
- append는 문자열 추가 가능. but appendChild는 element만 가능
  
  - 즉, element는 create로 막 만들고 지지고 볶은거로 appenChild는 노드만 삽입 가능
  
  

## 오답풀이

- getElementsBy.. => 복수가 나오므로, 무조건 인덱스로 지정 해주기
- JS에서 split은 무조건 (" ") 이런식으로 줘야지 공백 기준으로 나눌 수 있다
- trim() : strip과 유사
- textContent VS innerText:
  - textContent = visible text , 모든 Node object에 의해 정의가능
  - innerText = full text, HTML 요소 오브젝트에 의해서만 정의가 가능
  - 예를 들어, `<span>앞 span<span display="None">뒤 span</span></span>`인 경우 textContent는 '앞 span'출력, innerText는 '앞 span뒤 span'출력



## JQuery

## 요점정리

### JQuery 기본

- 문자열로 응답되고 있는 파일(독타입은 xml)을 XML객체로 만들어주는게 responseXML인데 Jq에서는 dataType속성이 정해줌 ( 즉 받아온 xml파일도 string형식으로 가져오는데 이걸 xml로 변환)

- $() => HTML요소를 제이쿼리에서 이용하게해주는데, 이렇게 생성된 요소는 제이쿼리 객체라고 칭함

- 선택자 css외에도 ("P")나 뭐 ("P:조건") 이런건 제이쿼리선택자임

  - 선택자중에 :checked도 있음, option은 :selected

  

- wrap vs wrapall은 wrap은 선택한 요소고 wrapall은 선택한 모든요소

- detach()는 remove()랑 다르게 연관된 제이쿼리 데이터나 이벤트는 삭제 X

- empty()는 detach와 remove()와다르게 그 자체는 삭제 X

  unwrap도 삭제의 일부. 근데 부모요소



### 형제, 자식 요소 선택

- 형제요소 선택: siblings(나제외), .next():형제요소 다음
- 자식 : children()얘는 자식요소만, find도 지정한 자식요소만 찾음



- add랑 each랑 구분.



- 여러개 쓸때는 ({checked : true, }) 이렇게고 하나면 (checked, true)

- CSS에서 처럼 background**-**color JQuery에서 가능, but JS에서는 불가능, 하이폰 빼고 Camel로 바꾼다



- Attr VS Property : Attr은 HTML 문서에 존재하고 값이 변하지 않으며, 프로퍼티는 HTML DOM 트리에 존재하고 그 값이 변할 수 있다.



### Ajax

- 그다음에, $.get() => 이게 get방식의 HTTP요청을 구현한 것임
- $.ajax(url주소, 옵션들) 메소드 : url주소는 클라이언트가 HTTP요청을 보낼 서버의 주소임.



- innerHTML, innerText 등이거는 변경도 가능하고 읽기도 가능.





## 오답풀이



- $("div:eq(1)") == $("div").eq(1)
- prop("checked",false) == removeAttr("checked") ==> attribute로 HTML에 속성이 정의되어있는 경우에만
- odd, even 이나 특정 인덱스의 자식을 부르고 싶을 때는 **nth-child(even)**
- 만약 자식이 없는데 .children()을 쓴다면 조건에서 제외된다.
  - `$(#isChild).children().children().parent().remove()` => isChild의 자식들 중 자식이 있는 자식을 삭제한다

- contains + string은 `contains:문자` 이렇게 따옴표 없이 표현
  - `$().children(":contains(bold)").css("font-weight","bold")`

