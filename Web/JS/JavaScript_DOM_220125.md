# JavaScript

## Element 생성

- element(tag) _객체_ 생성 : 

  - var 변수 = document.__createElement__("태그이름")
    - 예) var span = document.createElement("span")

- 생성한 element(tag)에 속성 설정 :

  - 방법 1
    - var 변수 = document.__createAttribute__("style") => style=""
    - 변수.__nodeValue__ = "color : blue border : 2px solid" => style= "color : blue border : 2px solid"
    - span.__setAttributeNode__(변수) => \<span style="color : blue border: 2px solid">\</span>
  - 방법2 (간편한 방법)
    - span.__setAttribute__("style","color : blue border: 2px solid")

- text node _객체_ 생성 :

  - var 변수 = document.__createTextNode__("text")
    - 예) var text = document.__createTextNode__("createTextNode활용해서 텍스트 노드 생성")

- 자식으로 (즉 span 태그 사이의 text로) 추가 :

  - span.appendChild(text)

- body 에 추가 :

  - __document__.body.appendChild(span)

  :heavy_exclamation_mark: script에서 그냥 노드명/태그명.appendChild는 불가능하다 (createElement로 생성했을 때 제외) , getelements.. 이걸로 가져와야했었음. => 그래서 body앞에 document 작성.

최종 결과물 : 

```html
<body>
	<span style="color : blue border: 2px solid">createTextNode활용해서 텍스트 노드 생성</span>`
</body>
```

:heavy_check_mark: create로 시작하는 명령어는 document가 앞에 붙고, set으로 시작하는 명령어는 앞에 속성을 바꾸고 싶은 node의 메소드로 작성



- replaceChild(바꿀값, 기존값)
  - 예) span.replaceChild(img, chd) : span자식 노드 중 하나인 chd를 img로 바꾼다 ( 참고로 chd와 img는 querySelector와 createElement..로 정의 되어있음)
- 만약 imgview라는 id를 가진 span 태그 사이에 또 다른 자식 태그가 있는데:
  - document.getElementyById("imgview").innerHTML = "\<img src=''>"
    - 이거는 덮어 씌우기
  - document.querySelctor("#imgview > img").src = ""
    - 이거는 값을 바꿈. 
- .getAttribute("속성이름") : 해당 속성의 값을 가져옴
- .setAttribute("속성이름", "값") : 해당 속성의 값을 지정 



- .appendChild(태그) : 함수 내에서 생성한 태그를 마지막 자식 노드로 추가

  - ```javascript
    var fieldset = document.getElementsbyTagName("fieldset")
    var p = document.createElement("p")
    
    fieldset.appendChild(p)
    ```

  - 만약 이미 있는 태그를 appendChild하면 기존 위치에서 삭제되고 지정한 위치에서 새로 생긴다:

  - ```javascript
    var div = document.getElementsbyTagName("div")
    
    fieldset.appendChild(div) 
    // 이미 body내 어딘 가에 있던 div태그의 위치를 fieldset아래로 이동 시킨다
    ```

    

- .insertBefore( 새로운 태그, 기준 태그) : 기준 태그의 앞에 새로운 태그 insert

- __.textContent__ : 함수 내에서 createElement로 생성한 태그(변수) 사이에 text를 끼워넣을 수 있다. __함수, 메서드가 아니라 속성__이다!!!!

  - ```javascript
    var td = document.createElement("td")
    
    td.textContent = "저는 td입니다"
    ```

:ballot_box_with_check: __textContent VS innerHTML__

- innerHTML :

  - element의 속성으로 해당 element의 HTML, XML을 읽어오거나 설정 가능.
  - 태그 내용까지 다 가져온다
  - 예를 들어서 div의 innerHTML을 읽어와서 변수에 담으면, div밑의 텍스트 뿐만 아니라 스페이스,  line break, 태그 등 그냥 그대로 다 가져온다.
  - 참고로 불러온 값에 < 이런 것들이 들어있으면 &lt 이런식으로 불러옴

- textContent : 

  - node의 속성으로 해당 노드가 가지고 있는 텍스트 값을 그대로 읽는다 (스크립트와 style도)
  - 모두 text이기 때문에 innerHTML과는 달리 태그들은 가져오지 x

  ```html
  // innerHTML
  >> 나는 배고파 특히 <strong>마라탕</strong>이 먹고 싶       어 
  
  // textContent
  >> 나는 배고파 특히 마라탕이 먹고 싶       어 
  ```

  

- document.forms : 문서에 있는 form 태그들을 가져온다
  - document.forms\[0][1] : 문서에 있는 첫번째 form 태그 안에 있는 __form 요소들 중의 2번째 요소를 __ 가져온다. 
  
  - form 요소들이란 ? 
  
  -  form element : 유저들과 교류하고 정보를 주고 받기 위해 디자인 된 컨트롤들. input태그는 여기에 해당. td나 tl은 form element가 아니기 때문에 document.forms\[0][1] 이렇게 하면 안가져옴.
  
  - forms로 가져온 form 요소들을 값을 가져오는 법:
    - 인덱스 활용 : forms\[0][1].value
    - 이름 활용 : forms[0].name.value : 해당 이름을 가진 form 요소의 value를 가져와준다
    
    
  
- .haschildNodes() : boolean값 반환. 메소드

- .removeChild(지울값) : 해당 태그의 자식 중 "지울값" 삭제
  - document.getElementById("addtr").removeChild(document.getElementById("addtr").__lastChild__)  : 해당 태그의 last child 삭제.

  
  
- nodename = tag이름
  
- parentNode : Node object로 결과 출력 ( HTML에서는 document자체가 HTML의 모든 요소들의 ParentNode임.) 
  
- children VS Child

  - childNodes : non  element노드 인 text나 코멘트까지 포함, 공백이있으면 텍스트노드(#text)가 포함됨. 그래서 개발자 도구에서 #text 눌러보면 \n 개행문자 들어있음 
  - child : 요소 노드만 잡아줌. (주석, 텍스트노드 제외)




## Ajax

> Asynchronous Javascript And Xml(Ajax)는 웹 페이지 전체를 다시 로딩 할 필요 없이 일부분만을 갱신할 수 있다. Ajax는 비동기통신의 특성을 가지고 있다. 비동기 통신은 클라이언트와 서버가 동기화 되지 않았다는 것이다. 
>
> 이전까지는 client가 요청을 서버로 보내면 서버가 응답을 해주는 방식이었지만, 비동기 통신에서는 페이지 내의 일부분만을 바꿀 때 까지 기다리지 않고 다른 작업을 계속 수행한다. 즉 서버에게 요청을 하고 응답을 기다리지 않은 상태로 계속 진행한다는 뜻이다. 이러한 특징이 오히려 왜곡되어서 콜백 지옥을 겪는 경우도 있다. 서버한테 요청한 것을 응답받기도 전에 밑에 코드들을 수행하려해서 에러가 뜨는 것을 말한다ㅣ.
>
> 비동기 통신의 또다른 특징은 서버에 무엇을 요청해도 서버는 클라이언트에 대한 정보가 이전보다 부족하게 된다. 이전까지는 새로운 창으로 이동하거나 동작을 수행하면 주소창에 입력이 되는데, 일부분만을 바꾸는 비동기 통신의 특징때문에 주소값은 그대로 유지된다. 
>
> Ajax는 웹 페이지 전체를 다시 로딩하지 않고도, 웹 페이지의 일부분만을 갱신할 수 있습니다.

- callback : 클라이언트가 call할 때까지 기다리다가, 요청을 하면 백을 하는 함수. 즉 사용자가 원할 때만 함수를 실행하게끔할 때. onload, onclick도 콜백 함수에 해당.

- ajax를 활용하려면 일단 객체 먼저 생성해야함:

  - `var xhr = new XMLHttpRequest()` 

- 변수 xhr의 상태를 확인하면서, 원하는 상태가 되었을 때 원하는 함수를 실행하게 하는 법 :
  - onreadystatechange 활용 ! __거의 유일하게 다 소문자로 작성__
  - xhr.onreadystatechange = function(){} => state가 변할 때마다 뒤에 함수를 실행하게 된다. state는 총 5개가 있으니 총 5번 함수를 실행하게 됨. 그러므로 콜백함수!
  - xhr.readyState == n (__이거는 대문자__)
    - 숫자 4는 요청이 완료 되었다는 뜻으로 최종 단계
  - xhr.status == 200 
    - 숫자 200은 정상적으로 응답했다는 뜻
    - 200대는 성공, 400은 클라이언트 문제, 500번은 서버쪽 문제

- xhr.responseXML 
  - 요청으로 응답한 HTTP/ Xml 결과물
  - 변수에 할당하는 식으로 사용 : `respXml = xhr.responseXML`
  - respXml의 타입은 객체. (js에 활용하려고 객체로 바꾼것)

- xhr.responseText
  - 요청으로 응답한 문자열

  

- xhr.open('방식','경로') : GET or POST 방식으로 해당 경로를 요청한다는 형식을 지정하고,

  - xhr.open('GET','emplist.xml',[true]) 
  - 세번째 인자는 비동기/동기 선택. default는 true이며 true는 비동기 방식을 지정
  - 만약 동기 방식을 사용한다면 함수가 다 실행될 때까지 기다렸다가 수행한다

- xhr.send() : 보낸다

  - 만약 'POST'방식을 썼다면 send내의 인자에 문자열 작성해야함

  

## 기타:

- length()가 아니라 __length__

- ```javascript
  var nodeLst = document.getElementsByName("btns")
  var val = ""
  
  for (var i=0; i < nodeLst.length ; i++ ){
  	if (nodeLst[i].checked){
          val = "resources/imgs/" + nodeLst[i].value
      }
  }
  ```

- CSS : vertical-allign : middle => __부모 요소의 중앙에 위치__ 

- CSS : text-decoration : underline => 해당 텍스트에 underline 긋기 (none 도 있고 다양한 속성 줄 수 있음)

- `var anchs = document.querySelectorAll("a") ` : 이렇게하면 anchs라는 변수에 a의 여는태그부터 닫는태그까지 다 들어가는 것임. text만 들어가는게 아니라. 
  - 예) `<a href="#" id="lt">◀</a>` 이거 통째로 들어간다.
  - querySelectorAll은 CSS기 때문에 콤마안에 작성!
  - 그러면 `anchs[0].onclick = function(){}` 이렇게도 가능하다!
  
- a의 태그의 href가 빈값이라면 현재 페이지를 다시 요청해서 다시 받기 때문에 새로고침을 하게 된다

- return은 함수 실행 중단, break는 반복문 빠져나오기