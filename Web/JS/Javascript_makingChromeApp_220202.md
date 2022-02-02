# Javascript

> JS로 크롬 앱 만들기 

## Basic

### Const vs let

- 개발자의 의도를 빠르게 파악 가능

- const => 상수, 값이 바뀔 수 없음, 즉 업데이트 불가능.
  - 처음 const를 생성한 곳 말고 다른 곳에서 const사용없이 그냥 변수를 업데이트 하고 싶어서 같은 변수명으로 생성하면 오류 뜬다.
- let => 나중에 업데이트 가능



### Datatype

- false => 값이 있는데 false인 것임 
- null => 값 자체가 없음, 자연스럽게 생기지 않음. 일부로 컴퓨터에게 여기는 empty하다는 것을 단어로 표현하기 위해서 작성. 파이썬의 None과 같음
- undefined => `let something;` => undefined가 뜸, 메모리안에 공간을 차지하지만 존재하지 않을 때
- NaN => not a number



#### Array

- 파이썬의 append => push



#### Object

```javascript
const player = {
    name: "nico",
    points: 10,
    fat: true,
}

console.log(player)
console.log(player.name)
console.log(player[name])
player.fat = false // const인데도 가능, constant는 player고 우리가 바꾸려는 것은 그 안에있는 fat이라는 property
player.lastName = "potato" // property 생성 가능
```



## Function

```javascript
const player = {
    name: "nico",
    sayHello: function(PersonName){
        console.log("hello" + PersonName)
    } // object안에서도 function 정의가능
}

console.log(player.name)
player.sayHello("lynn") // 이렇게 변수지만 실행하기 위해 () 사용
```

- argument : 함수 실행할 때 정보 보낼 수 있음.



- 만약 함수나온 값을 사용하고 싶다면 return 사용

```javascript
const age = 06;
    function calKrAge(age){
    return age + 2
}

const krAge = calKrAge(22)
```



### Conditionals (조건문)

- prompt("사용자에게 질문 묻기") => 단점: css에서 꾸미기 불가능. 그래서 잘 사용하지 않음
- prompt를 이용해서 사용자가 입력한 값은 string.
- parseInt() 사용!

```javascript
const age = prompt("나이 입력")
```



- isNaN(값) : false면 number라는 뜻

- if(condition){condition==true} else {condition==false}

- else if를 elif대신 사용!
- && / || 



## JS in Browser

> 자바스크립트에서 HTML 접근하기

- 변수를 만드는 이유 : 나중에 후에 오타로 인한 오류를 잡기 위해서

### Document object

- JS에서 HTML의 내용을 읽어오고 변경 가능

- document도 HTML에서는 객체.
- JS에서 `document.title = "Hi" `하면 바뀜.  => 즉 JS에서 HTML문서 변경 가능
- console.dir()은 element 내부를 보고 싶을 때.
  - console.log는 그냥 element자체만 보여줌
- `title.innerTEXT` => 값을 읽어오기
- `title.innertTEXT = "Got you!"` => 이런 것도 JS로 html의 innertext라는 속성 (document클래스의)를 변경하는 것임.

- `queryselector ` => 첫번째거만 가져옴
- `queryselectorAll` => array로



### Event

> on으로 시작하는 것들로 사용자의 행동 감지
>
> style변경은 CSS로 왠만하면..

1)

- `addEventListener("event이름", event실행시 실행할 함수명)`
  - 예) `addEventListener("click",handleTitleClick)` 
  - 유의할 점은 **두번째 매개변수인 함수는 괄호없이! eventlistener가 알아서 괄호를 붙여주기 때문에**
- 노마드는 이 방식을 더 선호하는 이유가 `.removeEventListener` 사용 가능
- `window.addEventListener("resize", handleWindowSize)` 이렇게 window창 자체에도 event를 줄 수 있음
- 중요한 것!!!! document.body.div 이런식으로 가져올 수는 있어도 document.div 는 불가능! document객체는 head, body, title등만 가지고 있는 것임!!!!!!!!

2)

- title.onclick = 
- title.onmouseenter=



#### event종류

- click
- mouseleave
- mouseenter
- offline
- online
- copy
- resize



- classList.contains(clickedClass) => clickedClass가 classlist에 포함되어있다면 이런식으로 가능

```javascript
const h1 = document.querySelector("div.hello:first-child h1")

function handleTitleClick(){
    const clickedClass = "clicked";
    // classList는 class들의 모음이다. 기존 class는 유지하면서 새 class추가 가능
    // 이미 classList에 clicked를 클래스가 있다면:
    if(h1.classList.contains(clickedClass)){
        h1.classList.remove(clickedClass)
    } else {
        // 만약 classList에 clicked라는 클래스가 없다면 :
        h1.classList.add(clickedClass)
    }
}


h1.addEventListener("click", handleTitleClick)
```

=> 이게 toggle!!!!!!

```javascript
const h1 = document.querySelector("div.hello:first-child h1")

function handleTitleClick(){
    // click이라는 event가 일어나면 clicked라는 class를 classlist에 만들었다 없앴다 반복
    h1.classList.toggle("clicked")
}


h1.addEventListener("click", handleTitleClick)
```

-> true인지 false인지 매번확인해서 그에 해당되는 액션들을 취해주는게 toggle.



- HTML의 input:

  - input.value로 사용자가 입력한 값을 가져올 수 있다
  - input의 유효성 검사를 활성화시키기 위해서는 input이 form 태그 안에 있어야한다




- addEventListener할 때마다 브라우저는 event에 대한 여러 정보들을 포함해서 유저에게 넘겨준다

  - 이 정보를 받으려면 첫번 째 argument를 지정해주면 된다

    ```javascript
    function1(event){
        console.dir(event) // 이 event에 대한 정보들을 console에 띄울 수 있다
    }
    
    h1.addEventListener("submit", function1) // h1이 submit될 때마다 function1을 브라우저가 수행
    
    ```

  - 이 event라는 변수 안에는 사용자가 event를 실행한 위치, 시간 등등의 여러 정보들이 들어있다

  

- HTML에서 submit버튼 클릭 아니라 엔터만 쳐도 submit라는 event가 일어나게 하고 싶다면 :
  - button click이 아닌 submit를 이벤트로 주면 된다.
  - BUT, submit event를 할 때마다 웹사이트는 refresh가 된다. 이것이 default behavior고  이러한 행동을 막기 위해서는 preventDefault() 메소드를 쓴다
  - `preventDefault()` => default behavior를 막는다?
  - submit의 새로고침이라는 default behavior말고 a 태그의 default behavior는 href로 이동이겠지?



```javascript
function onLoginSubmit(event){
    event.preventDefault(); // 새로고침을 막기위해서 
    const username = loginInput.value;
    // CSS파일에 hidden이라는 클래스가 있는 경우 display가 none이 되도록 설정 되어있는 상황
    loginForm.classList.add("hidden") 
    console.log(username)
}
loginForm.addEventListener("submit", onLoginSubmit)
```

:white_check_mark: string만 담은 변수는 대문자로 저장하는 경우가 많음

- 예) `const USERNAME = "jisu"` 

:white_check_mark: ""와 + 없이 문자열과 다른 datatype합치기 : $, {}, ``(backticks)

```
    greeting.innerText = "Hello " + username 
    greeting.innerText = `Hello ${username}'
```



### Local Storage

> 이미 브라우저에 존재하는 storage로 브라우저 저장소임

- 확인 방법 : 개발자도구 => Application => Storage => Local Storage 

- setItem("key","value")
- getItem("key")
- removeitem("key","value")

```javascript

const loginForm = document.querySelector("#login-form")
const loginInput = document.querySelector("#login-form input")
const greeting = document.querySelector("#greeting")
const HIDDEN_CLASSNAME = "hidden"
const USERNAME_KEY = "username"

function onLoginSubmit(event){
    event.preventDefault();
    loginForm.classList.add("hidden")
    const username = loginInput.value;
    localStorage.setItem(USERNAME_KEY, username)
    paintGreetings(username)
}

function paintGreetings(username){
    // 먼저 화면을 다 구성한 다음에 display none속성을 없애기 위해 hidden class를 없애는 작업을 수행해야한다
    greeting.innerText=`Hello ${username}`
    greeting.classList.remove(HIDDEN_CLASSNAME)
}


const savedUsername = localStorage.getItem(USERNAME_KEY)

if (savedUsername === null){
    // username이 등록된게 없을 때 : show the form
    loginForm.classList.remove(HIDDEN_CLASSNAME)
    loginForm.addEventListener("submit", onLoginSubmit)
} else {
    paintGreetings(savedUsername)
}
```



## Clock

- interval : 
  - 특정 시간마다 반복적으로 함수를 실행한다
  - `setInterval(함수명, milisecond단위의 특정 시간)`
  - `setInterval(sayHello, 5000)` => addEventListener처럼 함수는 내가 아니라 브라우저가 실행하기 때문에 함수이름만 쓴다

- Timeout:
  - interval과 비슷한 문법이지만, 일회성이다
  - `setTimeout(sayHello, 5000)` 5초뒤에 1회만 sayHello함수 실행


시간이나 분 초가 1의자리일 때 앞에 십의 자리를 0으로 채우고 싶다면?

- padStart:
  - __문자형__ 데이터를 지정한 길이의 데이터로 만든다
  - ;add Padding at Start
  - 위에 setInterval과 setTimeout은 단독으로 잘 쓰이는 함수인 반면 padstart는 메소드와 가깝다
  - `.padStart(2, "0")` => 문자형데이터의 길이를 2로만들며 만약 빈자리는 0으로 채운다
    - 꼭 두번째 argument는 ""로 감싸줘야하며,
    - padStart의 대상인 데이터는 **문자열 ** 이여야 한다.

- padEnd

```javascript
const clock = document.querySelector("#clock")

function getClock(){
    const date = new Date()
    const hours = String(date.getHours()).padStart(2, "0") //padStart는 string만 적용 가능하니까.
    const minutes = String(date.getMinutes()).padStart(2, "0") 
    const seconds = String(date.getSeconds()).padStart(2, "0")
    clock.innerText = (`${hours}:${minutes}:${seconds}`)
}
// 바로 getClock()을 시행하고
getClock()
// 그 다음부터 1초마다 계속 호출되고 실행
setInterval(getClock, 1000)

// padStart(characterslong, "0") : 만약 characterslong의 길이가 아니라면 0으로 채우기
// add padding in the start 
// 문자형만 적용 가능 !!!!

// padEnd(2, "0") : 이것도 마찬가지
```





## Quotes & Background

- Math.random()
- Math.round() => 반올림
- Math.ceil() => 올림
- Math.floor() => 내림

=> 이것들을 활용하여 array내에 있는 quotes나 background를 랜덤으로 불러올 수 있다

```javascript
const quotes = [
{
    quote:"Mistakes are a fact of life. It is the response to the error that counts.",
    author: "Nikki Giovanni"
},
{
    quote:"If I wait for someone else to validate my existence, it will mean that I’m shortchanging myself.",
    author: "Zanele Muholi"
},
{
    quote:"I've been very fortunate to have good people in my life, and when you find good people, you gotta hold onto them real tight.",
    author: "Lana Condor"
},
{
    quote:"Life is not so much what you accomplish as what you overcome.",
    author: "Robin Roberts"
},
{
    quote:"If you feel like there’s something out there that you’re supposed to be doing, if you have a passion for it, then stop wishing and just do it.",
    author: "Wanda Sykes"
},
{
    quote:"If you don't like the road you're walking, start paving another one.",
    author: "Dolly Parton"
},
{
    quote:"The most beautiful things in the world cannot be seen or even touched. They must be felt with the heart.",
    author: "Helen Keller"
},
{
    quote:"Life is a series of baby steps.",
    author: "Hoda Kotb"
},
{
    quote:"Love yourself first and everything else falls into line.",
    author: "Lucille Ball"
},
{
    quote:"Lead from the heart, not the head",
    author: "Princess Diana"
}
]

const quote = document.querySelector("#quote span:first-child")
const author = document.querySelector("#quote span:last-child")

// 랜덤 숫자 형성
const todaysQuote = quotes[Math.floor(Math.random() * quotes.length)]

// todayQuote는 dict형태로 되어있어서 키를 이용해 값 불러오기
quote.innerText = todaysQuote.quote
author.innerText = todaysQuote.author
```



```javascript
const images = [
    "0.jpg",
    "1.jpg",
    "2.jpg",
    "3.jpg",
    "4.jpg",
    "5.jpg",
    "6.jpg"
]

const chosenImage = images[Math.floor(Math.random() * images.length)]

// HTML에서 말고 JS 내에서 HTML element 생성 : 
const bgImage = document.createElement("img")
// 중요한 것은 여기 경로 기준은 HTML기준으로 세워야한다.
bgImage.src = `img/${chosenImage}`

document.body.appendChild(bgImage)
```

