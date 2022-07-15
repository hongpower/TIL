# 조건문



- `if`, `else if`, `else`

  - 소괄호는 생략가능하나 중괄호는 **생략 불가능**
  - if (someInteger)이런식으로 불가능, **bool 값이 꼭나와야함**

  ```swift
  // 1)
  if (someInteger < 100){
  	print("100")
  }
  
  // 2)
  if someInteger < 100
  	print("100")
  ```

- `switch case` : ~인 경우에 

  - 중괄호 작성 필요 X
  - 만약 명확한 모든 경우를 작성하지 않는다면 **default**를 꼭 작성해야함
  - 각 case는 if else와 동일한 느낌이라 하나의 case를 충족하면 다른 케이스를 확인 못함. 그래서 만약 다른 케이스까지 확인하게끔 하고 싶다면
    - 1) `case 0, 1`: 이런식으로 작성하기
      2) `fallthrough` 키워드 활용하기

- ```swift
  switch someInteger{
  	case 0:
  		print("zero")
    case 1:
    	print("one")
    // 범위 연산자 :
    case 1..< 100: // 1이상 100 미만
    	print("1~99")
    case 101...Int.max:
    	print("over 100")
    default:
    	print("unknown")
  }
  
  // 다른 언어처럼 케이스가 충족하더라도 다른 케이스도 확인하게끔 만들고 싶다면:
  switch someInteger{
    case 0, 1:
    	print("0 or 1")
    case 2:
    	print("2")
    	fallthrough
    case 3:
    	print("3")
  }
  ```

- 범위연산자
  - `..` : 이상 미만
  - `...` : 이상 이하



- Switch VS If
  - if는 true/false고 switch는 특정 값을 가리킬 수 있기 때문에 만약 수식과 같이 특정 값을 도출하는 경우에는 switch활용이 더 유용함
  - case는 상대적으로 다뤄야하는 케이스가 많은 경우 더욱 유용함 (성능이 빠르기 때문에)