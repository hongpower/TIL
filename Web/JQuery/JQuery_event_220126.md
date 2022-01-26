# JQuery

## Event:

- 이벤트 핸들러란?

  - 이벤트를 처리하기 위한 함수
  - 이벤트 핸들러가 연결되어있는 element에서 지정된 event가 발생하면, 연결된 이벤트 핸들러를 실행하게 된다
  - click, mouseenter, mouseleave등이 해당됨

- 이벤트 연결이란(event binding)?

  - 이벤트 핸들러를 특정 요소에 연결하는 것
  - click이라는 이벤트 핸들러를 element에 연결하는 여러 예시:
    - #("div").click(function(){})
    - #("div").bind("click", function(){})
    - #("div").on("click", function(){})

- .on() :

  - 이벤트 연결을 위한 사용하는 메소드
  - 하나의 이벤트 핸들러에 여러 개의 이벤트 동시에 연결 가능케함
  - 예)

    - $("button").on("mouseenter mouseleave",function(){})
  - $("button").on({mouseenter: function(){}, click: function(){}}) => 여러 이벤트에 각기 다른 function을 실행하고자 한다면 __중괄호{}__안에 작성. 
    - mouseenter에 따옴표 써도 관계 X 

  - $("body").on("mouseenter", "button", function(){}) : 이렇게 .on하고 원래 앞에 작성했던 element를 on의 요소로 적용 가능. => body내의 button에 mouseenter하면 function 수행

- .bind() : 

  - on과 기능에 차이가 없지만 on() 쓰기 권고.

- .unbind():

  - bind를 취소

  - ```javascript
    $("div").bind({
    	mouseover: function(){
            $(this).css("color","green")
        }
    })
    
    $("p").click(function(){
            $("div").unbind()
    })
    // p를 클릭해서 unbind를 수행하면 mouseover를 해도 함수 실행 X
    ```

- 순서 : 이벤트가 다 전달되고나서 기본 동작을 수행

- 이벤트 전파란?

  - 각 element가 중첩관계일 경우에 하나의 element의 이벤트가 발생했을 때 중첩되어있는 요소들에도 연쇄적으로 이벤트가 전파되는 현상

  

- 이벤트 전파를 막는 방법

  - stopPropogation() : 이벤트 요소의 전파를 막기, 예를 들어서 click이벤트를 전파하지 않는다. 기본 동작은 수행
    - propogation = bubbling up 같은 말.
  - preventDefault() : 이벤트로 발생되는 디폴트(기본 동작)을 막기
  - return false : 위의 두 기능 모두 적용 + __call back 실행 멈추고 다시 불렸을 때 수행__
    - return false는 제이쿼리 사용할 때만 두 기능이고 제이쿼리 사용 X면 preventDefault와 같음
  - 위 함수(stopPropagation과 preventdefault)은 __이벤트 객체__에만 사용 가능하다!!!!!!!!1

- jQuery에서 종종 매개변수로 e(event)를 전달받는 코드가 있는데, 이는 JQuery에서는 이벤트 메소드를 binding하면 __첫 매개변수로 event 객체__가 무조건 들어온다. 매개변수 e말고도 다른 매개변수 작성도 괜찮다.

  - 예) 

  - ```javascript
    $("button").click(function(e, x, y){
        console.log(e) // event객체
        console.log(x) // undefined
        console.log(y) // undefined
    })
    ```



- event 객체는 event가 일어난 element만 지정한다:

- ```html
  <button>
      <p>
          <div>
              button의 p의 div의 text야
      </div>
      </p>
  </button>
  
  <script>
  $("div").click(function(event){
  	alert("나는 div야")
  	// event.stopPropagation()
      // event.preventDefault()
      // return false
  })
  $("p").click(function(event){
  	alert("나는 p야")
  	event.stopPropagation() // 여기의 event는 p가 아니라 div의 event이다. 그러므로 여기에 작성해도, 나는 div야가 뜨지 않지 나는 p야는 뜬다
      // event.preventDefault()
      // return false
  })
  </script>
  
  ```

  

## 번외 :

- script 작성위치 :
  - html이 load가 다 되었을 때 수행하는 $(function(){})는 위에서 아래로 수행하기 때문에
    - 이 함수 내에 p를 클릭했을 때 특정 함수를 수행하는 이벤트 함수와 그 아래에 p를 append("\<p> 새로 추가 됨 \</p>")와 같이 생성했다면 새로 추가 된 p를 클릭해도 함수 실행 X.

- 메소드 체인 : 메소드 return값이 jquery라서, .css().css().css() 이렇게도 가능하다. 즉 .css()한 값이 $로 나오기 때문에. 

- 만약 $ is not defined라는 오류가 뜬다면 :  jquery언급안했을 확률이 높다



## 기타 :

- html 속성 중 hidden은 hidden=true/false 이렇게 작성하지 않고 `hideen` or `hidden=""` or `<input type='hidden'>`등으로 가능
- input type이 submit인 버튼을 누르면 method, action이 지정되어있는 form에서 발생.

- html에서 불러온 문서는 다 __문자열__이다. (예를 들어 val()을 활용해서 jquery, value()를 활용해서 js에서 가져오면 다 문자열로 가져온다)
- jquery는 여러 요소들을 한번에 적용이 가능하다
  - 예) $("p").children().css("color", "blue") : children의 요소가 여럿이더라도 한번에 적용 가능

- HTML의 input type=button VS button
  - input : content가질 수 없다. 사용자에게 보이는 button의 이름은 __value__로 지정한다
    - `<input type="submit" value="제출버튼">`
  - button : content를 가질 수 있다. 사용자에게 보이는 button의 이름은 태그 사이 __text__로 지정한다
    - `<button type="submit">제출버튼</button>`
    - tag사이에 HTML element를 또 추가가 가능하므로 이미지 삽입도 가능하다 : `<button><img src="" alt="">제출버튼</button>` => button안에 ''이미지 + 제출버튼''가 생성

- Element VS Tag
  - HTML Tag : 열고 닫는 entity
  - HTML element : 열고 닫는 tag 뿐만 아니라 content까지 포함

- 단축키 :
  - ctrl + shift + 방향키 ( 왼쪽, 오른쪽, 위쪽 다 다름)