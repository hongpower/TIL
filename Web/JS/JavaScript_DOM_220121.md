# JavaScript(JV)

## 번외 : HTML

- input 태그 : 사용자에게 값을 입력받는다
  - input의 value는 사용자가 작성한 데이터고, 미리 설정할 수도 있다.
  - input type="text" : 사용자가 텍스트를 작성할 수 있는 작은 텍스트 박스 생성
  - input type = "button"
  - input type ="password"
  - input type=""

- pre 태그 : preformatted text로 박스 요소이다.

<img src="https://user-images.githubusercontent.com/96896873/150629098-f0219099-ed10-4a07-a79c-ec319a7ba148.png"/>

- 왼쪽에도 공간이 있다.
- button 태그 : `<button onclick="함수()">내용</button>`



## DOM

> 문서((HTML의 텍스트와 같은)를 javascript가 이해할 수 있는 객체로 만들어주는 인터페이스.

- Document Object Model
- 웹페이지를 핸들링하기 위해서 Document를 Object(객체는 하나하나의 덩어리 = Node)로 바꿔서 가져오는 거
- 번외로 GOM도 있다
- JS가 HTML 요소에 접근하기 위해서는 Document 객체를 사용한다.

- object = node
- node가 여러개인 것 모음 = nodelist



- DOM 탐색 : 구조화된 데이터에서 원하는 노드 탐색

  - document 객체의 메소드들
  - document.querySelector("css연산자"){}  : 주어진 css 연산자 활용해서 원하는 노드 고를 것이다. 노드가 여러개라면 제일 첫번째 만나는 값 반환. 
    - css연산자는 : #id .class 등 이런것들. CSS파트에 있음
  - document.querySelectorAll("css연산자"){} : 해당 노드 모두 고름.
  - document.getElementById("Id이름"){} :  id 속성으로 탐색.
    - Id중 d는 소문자이다
    - Id는 하나기 때문에 한 '노드' 가 변수에 전달된다
  - document.getElementsByName("name이름"){} : 이름으로 탐색
    - 만약에 해당 이름이 여러개가 있다면 해당 값을 지정받은 변수는 nodelist가 된다
    - list의 일종이기 때문에 [0] 이런식으로 인덱스를 활용해 리스트 중 한 노드 꺼낼 수 있다
  - document.getElementsByTagName("태그이름"){} : 태그로 탐색



< DOM 요소의 내용 변경 >

- innerHTML : HTML DOM 요소중 하나다

  - document 객체의 getElementByID() 등으로 HTML요소를 선택한 후 사용.
  - 지정한 노드의 내용/속성 변경 가능
  - __원래 써져있던 내용은 초기화하고 작성__

  

< DOM 요소의 속성 변경 >

- style 프로퍼티 활용해서 요소 접근 가능!
  - `function changeColorToBlue(){document.getElementById("coat").style.color = "Red"}`
- value가 없을 때 value=""도 가능하다
  - `document.getElementByTagName("p")[1].value = "여기는 두번째 파라그래프이다"`



< 이벤트 핸들러 추가 >

- onclick과 function을 섞어서 클릭 이벤트가 일어날 때 code가 작동되도록
  - document.getElementById("id이름").onclick = function(){}



## Object

- JS는 클래스라는 개념이 없다 
- 대신 JS는 객체를 상속하기 위해서 클래스 대신 프로토타입 방식을 사용한다.
- 생성자는 함수도 될 수가 있다.



- JS에서 객체 생성 방법 :

1. New 생성자 사용:

     - 사용자 정의 생성자, 또는 JS에서 제공하는 생성자(Date와 같은)도 사용 가능

     - `var 변수명 = new 함수명() `-> 변수가 함수명을 참조하게 된다/ 함수로 객체(인스턴스)를 생성했다

  ```javascript
function Dog(type, name){ //객체 생성
	this.type = type	// 프로퍼티 부여
	this.name = name
}

Dog.prototype.color = "white" //프로토타입 프로퍼티로 color 속성 추가
Dog.prototype.introduce = function(){ // 프로토타입 프로퍼티로 함수 실행하면 강아지 소개문을 리턴하는 함수 생성
    return this.type + "이고 이름은" + this.name + "입니다" 
}

var dog1 = new Dog("포메라이언","코코") //인스턴스 생성

document.write("우리집 강아지의 종은 " + dog1.introduce()) //함수실행
  ```

  

2. 객체 리터럴 사용  : {}사용

   - 훨신 간결해서 객체 생성 구문으로 많이 사용

   - {키 : value}

   - 각각의 property 쉼표로 구분.

   - `const plus = {a1 : 1, a2 : 2}` : a1을 "a1" 이렇게 감싸도 된다.

   - console.log(plus.a1) >> 1

   - console.log(plus) >> {a1: 1, a2: 2}

   - console.log(plus[0]) >> 1



- this : 객체 생성했을 때, 해당 객체의 값을 외부에서 접근 가능하게 하는 변수
  - this는 object01라는 객체를 지칭 (self랑 this랑 같은 느낌)
- 그래서 함수나 객체 내에서 생성된 this.name01은 접근가능, var name02는 접근 불가능



- prototype : 이미 만들어진 객체에 기능을 추가할 때 굳이 객체 생성된 곳까지 가지 않고 기능을 수정 가능하다.

  - 만약에 prototype이 어느 함수나 변수 내부에서 실행되는게 아니라면 추가된 기능은 어디서든 접근 가능하다.

  

- 이외에도 이미 만들어져있는 표준 객체들이 JS에는 존재한다. 
  - 예) Math, Date, String 등등



- 객체는 여러 형태가 될 수 있다. 함수, 변수 등이 다 덩이리로 인식되면 그것이 객체다.

- 함수 객체 생성 : 

```javascript
// 함수를 객체로 한 인스턴스 생성은 가능하지만, 함수 내에 var로 만들어진 지역변수는 접근 불가능
<script>
 
// sayHello()라는 함수 생성.
function sayHello(){
    this.myname = "지수"
    var age = "25"
    this.introduce = function(){
        return "내 나이는 " + age + "이다"  //값을 비밀함수로 갖는 introduce라는 지역변수
    }
}

function HelloTest(){
    var hello1 = new sayHello() // 객체더라도 함수기 때문에 () 붙어야 한다
    alert(hello1.myname) // 지수 출력
    alert(hello1.age) // 나이 출력 불가능. 지역 변수라서. 그래서 undefined가 출력된다
    
    // 지역 변수 접근 방법:
    alert(hello1.introduce()) // introduce변수는 sayHello()함수 내에 있기 때문에 age라는 지역변수 접근 가능하고 그 값을 담아서 출력해준다
    alert(hello1.introduce) // 함수 실행(호출)이 되지 않았기 때문에 값을 불러오기만 한다 값은 "function(){return " 내 나이는 "..}" 그~대로
}
    
</script>

// 소개하기 버튼 클릭시 HelloTest() 호출
<button onclick="HelloTest()">소개하기</button>
```



- 전역 변수인 객체를 만들었었을 때 접근 :

```javascript
<script>
    // {}를 활용해서 객체 생성. 전역변수로
    var food = {
        type : "한식", // 콤마 중요!
        name : function(){
            alert(food.type + " 나랑 먹으러 갈래 ? ")
        }
    }
    

    function EatSth(){
		alert(food.type) //굳이 인스턴스 생성안하고 객체로 바로 접근 가능. 왜? 전역변수니까
        alert(food.name()) //함수실행
        alert(food.name) // 함수가 실행되지 않아서 뒤에 값 그대애로 출력
    }
</script>

// 소개하기 버튼 클릭시 HelloTest() 호출
    <button onclick="EatSth()">배고플 땐 밥먹기</button>
```
