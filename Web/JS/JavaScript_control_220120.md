# JavaScript

## Control

- alert는 말 그대로 경고메세지, 알림처럼 일방적으로 팝업창하나 뜨고 '확인'버튼밖에없다
- prompt는 사용자가 답변을 할 수 있는 공간이 있다

1. if

   - if (조건문) {실행문}
   - else if (조건문) {실행문}
   - else {실행문}
   - if는 조건문을 ()안에 작성하고, 실행문은 {}에 작성한다
   - 전체적인 프로세스는 파이썬과 유사하다

2. swith

   - switch는 변수명을 받아와서 case들의 조건 중에 변수와 일치하는 case로 이동한다.

   - switch(변수명){}

   ```javascript
   function switchFavorite(){
       var food = prompt("좋아하는 음식을 입력하시오 1. 스시 2. 마라탕 3. 훠궈 4.라멘")
       switch(food){               //switch가 food라는 변수를 호출하고, prompt가 실행된다
           case "스시":
               alert("뭘 아는놈");
               break;
               
           case "마라탕":
               alert("인정");
               break;
               
           case "훠궈":
               alert("하이디라오 존맛");
               break;
               
           case "라멘":
               alert("한국 라멘은 별로던데");
               break;
            
           default:
               alert("여기에 있는 음식 입력해줘") //default는 기본값으로 else처럼 작성하지 않아도 실행하는데 문제가 없다.
       }
   }
   ```

   - switch는 break를 넣는 것을 유념해야한다. 만약 break를 넣지 않는다면, 매개변수와 일치하는 case부터 그 아래에 모든 구문들을 break를 만날때까지 시행하기 때문이다.


3. for

- for(var 초기값 ; 조건식 ; 증감식 ; 증감연산자){실행문}
- 순서가 중요하다 : 초기값 확인 -> 조건식 -> 실행문 -> 연산자

```javascript
//forTest함수를 실행하면, 1) 초기값은 0인 i를 2) 조건 i <20에 해당되는지 확인후 3) 조건에 맞다면 i를 콘솔에 띄우고 4) i를 증감시켜라 
function forTest(){
for (var i=0; i < 20; i++){
    console.log(i)
}
}
```

:heavy_check_mark: console.log(x)는 개발자도구의 콘솔창에 x를 띄우라는 뜻이다 

4. while

- while만 사용 : while(조건문) {실행문} => 우리가 아는 파이썬처럼 해당 조건에 맞을 때 실행문을 실행한다 (선조후실)
- do, while문 사용 : do{실행문} while(조건문) => 적어도 한번은 do안의 실행문을 실행하고, 이후에 해당 조건이 참이면 do구문을 또 실행한다. (선실후조)

```javascript
function whileTest(){
	while(i < 10){
		console.log(++i) // 조건에 맞다면, i를 증감시키고 변수값을 리턴해라(log창에 띄워라), 만약 i++이였다면 0부터 출력
	}
}

function dowhileTest(){
    do{
        console.log(j); 
        j--	 //띄운 이후에 j를 감소시킨다
    } while (j > 0) 
}
```

- 연산자 표현 방법:

  - i += 1
  - i ++ : i값 리턴하고 연산 수행
  - ++i : 연산먼저 수행하고 i값 리턴
  - console.log(i++)

  

# 구구단 만들기

```javascript
//while 활용해서 구구단 만들기
function gugudan(){
    var guguNum = prompt("원하는 1~9 사이의 숫자를 입력하시오 ");
    guguNum = Number(guguNum);
    var i = 1;
    while (i < 10){
        console.log(i + "*" + guguNum + "=" + i * guguNum);
        i++;
    }
}

//if와 for활용해서 구구단 만들기
function gugudan2(){
    var guguNum2 = prompt("원하는 1~9 사이의 숫자 입력하시오");
    guguNum2 = Number(guguNum2);
    if ((guguNum2 > 9) || (guguNum2 < 1)) {
        alert("1에서 9사이의 숫자를 입력해주세요");
    } else {
        for (var i = 1 ; i < 10 ; i++){
            console.log(guguNum2 + "*" + i + "=" + guguNum2 * i);
        }
    }
}
```





## 기타: 

- 자바스크립트는 낙타, python은 클래스전까지는 스네이크 방식을 변수명 지을 때 사용한다
- head에 title은 변경 가능하고, 변경하면 웹 상단에 title이 바뀐다.
- 오류가 났을 때 f12개발자도구를 실행하면 쉽게 확인 가능하다.
- html에서 콜론은 생략 가능 (여러 속성 넣을 때 제외) 
- onclick 처럼 on이붙은거는 event인 경우 99% 

- __head랑 body사이에 script써도 된다! body안에 script써도 된다! 즉 script는 위치 상관없다.__

- onload는 브라우저에 내 코딩이 화면에 표시되면 ~을 시행해라라는 뜻이다.
- JS에 주석은 //, 여러줄 주석은 css처럼 /* */
- id는 한번만 사용하기 때문에 getElemen__t__ById임. class처럼 여러번 사용할 수 있는 속성은 getElement__s__ByClass이다.
- innerHTML은 누르면 HTML언어로 작성된걸 뜨게하는듯????????????
- ctrl + enter하면 키보드로 오른쪽 움직이지 않고 한번에 다음줄 이동 가능
- __JS에서는 true와 null 둘다  소문자!__!  ; null은 파이썬의 None과 거의 비슷
- 메서드는 종속, 함수는 독립적으로 존재.
- type="text/javascript" <= 생략가능.

- 개발자도구에서 favicon에러는 title을 안정했을 때



