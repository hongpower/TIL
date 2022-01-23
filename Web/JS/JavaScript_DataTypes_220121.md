# JavaScript(JS)

## 복습

- 변수의 값으로 저장된 함수를 실행하고 싶으면 ''()''
- return 함수 => 함수의 설계도 자체가 반환값이 되는 것임.
- do while은 최소 한번은 실행이 된다



## string

- 문자열끼리 합치기

  - 문자열끼리는 "+"로 합치면 띄어쓰기 없이 합칠 수 있다

  - "문자0".concat("문자1","문자2","문자3") : 문자0에다가 concat()안에 있는 문자들이 차례로 합쳐져서 한 문자열로 출력된다

  - \["문자1","문자2","문자3"](배열).join("이다 ") : "이다"가 문자들 사이에 배치된다. 파이썬과 반대다.

    - 참고로 리스트가 아니라 JS에서는 배열이라 한다.

    

- 다른 자료형 합치기

  - 문자열과 다른 데이터 타입 합치면 문자열로 출력된다. 즉 하나라도 문자열이 있다면 무조건 문자열이다
  - 타입확인하고 싶다면 typeof()함수 사용한다

  

- 문자열 비교하기

  :heavy_check_mark:요소.textContent = "" : innerText처럼 태그와 태그사이에 값을 작성한다

  :heavy_check_mark:요소.toLowerCase() : 실제로 값을 바꾸진 않고 앞에 요소를 소문자로 인식해라

  - JS는 자동으로 형변환을 해준다. 물론 형을 계산식에만 변환하고 반환값에만 적용하지 원본 데이터 형이 바뀌는 건 x.

    - 내가 숫자타입의 10을 입력하고 문자열 "10"을 입력해도 JS는 자동으로 형변환을 해서 같은 걸로 인식해준다

    :heavy_check_mark: 암묵적 타입 변환 :

    - "+" : 숫자보다 문자열을 우선시해서 숫자 -> 문자열로 변경
      - ex) 1 + "2" => "12"
    - *, -, /, % : 문자열보다 숫자열 우선시해서 문자열 -> 숫자 변환

  - 자동으로 변환하기 싫을 때는 ==대신 === 사용한다.
  - Object == 문자열도 마찬가지로 형변환을 해서 출력한다. ; 마찬가지로 엄격하게 하고 싶으면 ==말고 === 사용

  ```javascript
  var strObj = new Animal("라이언") //strObj는 문자열이 아니라 Object 타입이다.
  var strLiteral = "라이언"
  
  if (strObj == strLiteral): //여기서 자동형변환
  	alert("둘다 라이언입니다")
  	alert(typeof(strObj)) // 출력값 : Object
  ```

  

- 문자열 검색하기

  - 변수.indexOf(찾는값) : 찾는 값의 인덱스를 __왼쪽에서 오른쪽 방향으로__ 찾는다. 마찬가지로 인덱스는 0부터 시작한다.
    - 만약, 한글자가아니라 여러 글자의 값이라면 그 단어의 첫글자의 인덱스를 찾는다
    - 예 ) `alert(namelst.indexOf("홍지수"))` >> 홍 글자를 namelst에서 앞에서 뒤로 찾는다.
  - 변수.lastIndexOf(찾는값) : 찾는 값의 인덱스를 __마지막에서 처음__방향으로 찾는다. 
  - 중요중요!!!!! lastIndeXOf는 찾는 값의 __첫 글자__를 마지막에서 찾기 때문에, 만약 찾는 문자열의 길이가 3이라면 맨 마지막 인덱스가 나오는게 아니라 2를 뺀 인덱스가 나오게 된다

  :ballot_box_with_check: 내가 찾는 값이 변수 내에 여러개 있다면(예를 들어 홍지수라는 값이 한 10개있다면..) indexOf는 젤 처음 값을 찾을 테고 lastIndexOf는 젤 마지막 홍지수를 찾을 것이다. 그래서 값이 다르게 된다.



- 문자열 추출하기

  ```javascript
  /// 추출1
  var justText = "이것은 그냥 할말이 없어서 쓰는, 쓸모없는 텍스트입니다!!"
  var startIndex = justText.indexOf(,) // 인덱스가 변수에 들어간다
  var endIndex = justText.lastIndexOf(!) // 인덱스가 변수에 들어간다
  
  alert(justText.substring(startIndex, endIndex)) 
  // 출력값 : " 쓸모없는 텍스트입니다"
  
  
  /// 추출 2
  var splitText = justText.split(" ") //이렇게 공백을 주어야한다
  ```

  - substring(이상, 미만) : 슬라이싱처럼 일부 부분 추출

    

- 키워드 나누기
  - prompt("텍스트","예시") : 뒤에 값은 미리 작성되어있는 내용이다
  - .length 는 파이썬의 len()과 같음

```javascript
// 입력된 내용을 "/" 기준으로 나눠서 하나씩 출력
function PrnEach(){
	var FavSong = prompt("좋아하는 노래 제목을 '/'로 나눠서 작성해주세요", "붉은 노을/좋은사람/7 Years")
	var songSplit = FavSong.split("/")
	
	var songList = ""
	for (var i = 0 ; i < songSplit.length ; ++i){ //길이 이전만큼 i에 숫자 부여
		songList += songSplit[i] + "<br>" //songList라는 변수에 br과 함께 노래 제목 삽입
	} 
	document.getElementById("song").innerHTML = songList //하나씩 순서대로 출력하기 위해서 br을 삽입한 songList를 입력
	
}
```

- 여기서 songList는 리스트형태도 아니고 그냥 __문자열__인데,` "붉은 노을<br>좋은사람<br>7 Years<br>"` 이렇게 들어가 있기때문에 HTML에서 작업을 수행하면 자동으로 한줄띄어쓰기 출력된다!!!!





## Number

- `isNaN(numInput)` : 숫자인지 아닌지 boolean값 반환 __NaN대소문자 구분 잘하기__

- ` var num02 = new Number(3)` : num02의 type은 Object이고 이것도 자동 타입 변환된다.

- parseInt() : 숫자가 적혀있는 문자 타입을 숫자로 변환

  - `alert(parseInt("1")+ 10)`: 11출력
  - `alert(parseInt("a") + 10)`: NaN 출력

  

- 난수:

- Math 객체 사용. 객체니까 대문자.

- Math.random() : __0에서 1사이(1 미포함 )의 값 생성__

- Math.random() * 101 : 0부터 100.324234 뭐 이정도까지 나오겠지 (101전까지)

- Math.floor(Math.random() * 101) : 그래서 이걸 버림하면 0부터 100이니까 0부터 100미만 값 나오겠지

- 10 <=x<100

  - Math 메서드 : random(), floor(), round(), ceil()

  

  ```javascript
  // 로또 숫자 랜덤으로 뽑기 ( 1~ 46(미포함))
  min = 1
  max = 46
  lotto = Math.floor((Math.random() * (min - max) + min)) // 1에서 45이하까지 랜덤으로 추출
  
  fo
  ```

  

  

## Date

- date 객체 생성 : var date = new Date()

- date 메소드:

  - date객체명.toDateString() : Sun Jan 23 2022
  - date객체명.toLocaleDateString() : 1/23/2022
  - date객체명.toLocaleString() : 1/23/2022, 11:15:48 PM
  - date객체명.toLocaleTimeString() : 11:15:48 PM
  - date객체명.getFullYear() : 2022
  - date객체명.getMonth() __+1__ : 1 
    - month는 0부터 숫자 시작
  - date객체명.getDate() : 23
  - date객체명.getDay() : 0부터 6까지의 요일 숫자 반환. 0이 일요일

  ```javascript
  // 활용방법
         var date  = new Date(); //date객체 생성. 인수없이 호출 시에 현재날짜, 시간등의 데이터를 저장
              var year = date.getFullYear()
              var month = date.getMonth() + 1 //0부터 숫자가 시작해서 0에서 11이라 1을 더한다
              var date = date.getDate()
              var day = date.getDay() 
  			var weekdays = ["일", "월", "화", "수", "목", "금", "토"]
  
  	document.getElementById("today").innerHTML = year + "/" + month + "/" + date + "/" + weekdays[day]
  ```

  - 이미 생성되어있는 객체에 prototype활용 가능
    - 예) Date.prototype.변수이름 = function(){}
  - date객체명.setDate()

```html
<!--경과날짜 구하기-->
<script>
    var dates = document.getElementById("dates").value // 사용자가 입력한 날짜 가져오기
    var inputDate = document.getElementById("inputdates").value // 사용자가 원하는 경과일 수 불러오기
    
    var date = new Date(dates) // 여기 중요! 사용자가 입력한 날짜를 활용한 객체 생성
    
    date.setDate(date.getDate() + parseInt(inputDate)) //parseInt는 button으로 불러온 문자형을 숫자로 변환한 후에 더한 값을 계산하고, 마지막으로 date객체의 날짜를 변경한다.
    
    document.getElementById("result").value = date.toLocaleDateString() //value에다가 값을 집어넣는다
</script>

<input type="date" id="dates"> <!--사용자가 시작 날짜 입력하는 공간-->
<input type="number" id="inputdates"> <!--사용자가 경과일 수 입력하는 공간-->
<input type="text" id="result"> <!--반환된 결과 출력-->
```

- date객체명.getTime() : 1/1000초를 계산해서 나열. 1970년 1월 1일 00시부터 지난milisecond출력

```javascript
var resultVal = (afterDate.getTime() - nowDate.getTime()) / (1000 * 60 * 60 * 24)
```

- 1/1000초 * 1000 => 1초 => = 1분 => 1시간 => 1일



## 기타

- 옆에누르면 빨간색 버튼 누르면 break. debug할 때 활용 좋음
- Literal : 값 그자체. 함수리터럴은 정말 함수 형태를 띠지만 값으로만 남아있는 데이터
- 이미 만들어져있는 함수/메소드 해석 방법:isXXX 이렇게 되어있는건 hasXXX 다 BOOLEAN으로 나온다. toXXX는 당연히 뭘로 변환

- 개발자도구에서 바꾸면 실제 로컬에서 바뀌지는 않음. 그냥 테스트 해보는 것이다.

- 개발자도구 - sources에 사진 있음

- 개발자도구-elements-상단에 <img src="https://user-images.githubusercontent.com/96896873/150629350-d7920efe-6ec3-4828-85fc-05acf0a86b0f.png"/>이거 누르면 정보 쉽게 찾을 수 있음

- 개발자도구 콘솔 창에 document.getElementsByClassName("nav")[3].innerHTML = "지수" 하면 바뀐다

- 개발자도구에서 적용 안되는 것들은 -- 선이 그어져있음

- borderRadius는 4개 쓰는거맞는데 하나만쓰면 다동일하게

- date객체끼리 연산 가능



