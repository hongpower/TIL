# JavaScript

## Datatypes

1. string
2. number
3. boolean
4. object
   1. Object
   2. Date
   3. Array
   4. String ( this can be an object when new keyword로 정의되었을 때)
   5. Number  ( this can be an object new keyword로 정의되었을 때)
   6. Boolean  ( this can be an object new keyword로 정의되었을 때)
5. function

## Array

- Array 생성 방법:
  - new 키워드 사용 : var array명 = new Array()
    - new Array(n) : n개의 빈공간인 요소를 가진 array를 만든다
    - new Array(1,2,3,4,5) : [1,2,3,4,5]라는 요소를 가진 array 만듬
  - literal notation 사용 : var array명 = []
- 2차원 배열 생성

```javascript
var arr = new Array(5) // 1번째 배열

for (var i=0; arr.length < 5 ;i++){
	arr[i] = new Array(3) // 1번째 배열 안에 또 배열 생성
}

// 바깥 배열은 5개의 요소, 각 요소는 3개의 배열로 이루어짐

arr[0][0] = ['pig',['tiger']] // 3, 4번째 배열 생성
// tiger라는 값을 바꾸고 싶을 때 :
arr[0][0][1][0] = 'cow' //마지막에 0을 또 추가하는 것 잊지말자
```

- arr.toString() : 배열 안에 있는 모든 값을 다 불러온다 

  - 출력형태 :  값1, 값2

  ### 정렬 :

- arr.sort() : 정렬. ASCII코드 순으로 

  - 숫자는 같은 자리수 끼리 대소 비교를 해서 추가적인 처리 필요:
    - `arr.sort(function (a,b){return a - b})`
    - 또는 sort 괄호 내에 있는 functon을 comparefunction이란 이름의 함수로 정의해서 사용하기도 한다
    - sort는 :
    - 두 개의 배열 element를 파라미터로 입력 받는다
      - return a- b했을 때 a가 더 크면 __1 리턴하고 뒤로 배치__
      - return a-b의 결과가 0이라면 동일하기 때문에 __순서를 변경하지 않는다__
      - return a-b의 결과가 a가 더 작다면 __-1 리턴하고 앞으로 배치__
  - 내림차순 정렬:
    - `arr.sort(function (a,b){return b - a})`
      - return b-a에서 b가 더크면 __1 리턴하고 -> a(첫번째 매개변수) 뒤로 배치__ (ascending이든 descending이든 1을 리턴하면 a는 뒤로배치)
      - !!! 즉 리턴 값(양수냐, 0이냐, 음수냐)에 따라 배치 위치가 달라진다 !!!
      - 그래서 만약에 내가 function(b,a){return b-a} 하면 오름차순이 되는 것임
  - :white_check_mark:sort는 원본값 변경!!!

- arr.reverse() : 원래 순서 역순 정렬



-  arr.push("값") : 값이 array 마지막 인덱스에 push된다
-  arr.shift() : 맨 앞 요소 잘라내기
   -  alert(arr.shift()) : 잘라낸 요소는 변수로 가져올 수 있음
-  arr.pop() : 맨 마지막 요소 잘라내기



### slice

- 이상 미만으로 __인덱스 위치__로 잘라오기
- `arr.slice(1,3)` 1부터 2 __인덱스__ 위치의 값을 잘라온다
- array를 슬라이싱을 하면 shallow copy가 되어 만약 slicing한 부분을 변경한다면 원본값도 변경된다

```javascript
var arr = new Array(3)
arr[0]= new Array(1,2)
arr[1]= new Array(3,4)
arr[2]= new Array(5,6)

// slicing
var sub_arr = arr[0,2] //0과 1 인덱스의 내부 array가져옴
sub_arr[0][0] = "바꾼값"

alert(sub_arr)
alert(arr) //원본값에 1이란 값이 "바꾼값"으로 바뀜
```



## Null VS Undefined

- Null : null은 객체로서 'no value'라는 value로 assign될 수 있음.

  - null은 객체이다

  - ```javascript
    var Word = null;
    alert(Word) //null
    alert(typeof(Word)) // object
    ```

    

- Undefined : 변수가 선언되었지만 값을 가지고 있지 않은 상태. unassigned

  - ```javascript
    var Word
    alert(Word) // undefined
    alert(typeof(Word)) //undefined
    ```

  - undefined는 그 자체로 type인 것이다.

    :heavy_check_mark: : typeof는 괄호 없이 사용 가능! 예) typeof Word

