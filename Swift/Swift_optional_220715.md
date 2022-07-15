# 옵셔널



> 값이 있을 수도 없을 수도 없음을 뜻함. 상수에다가 nil값 넣는게 불가능하니 그런 문제를 해결할 수 있음. 

- nil의 가능성을 **명시적으로 표현하는 것임** (즉 주석을 굳이 따로 달지않아도 가능)
- nil체크 하지않고도 안심적으로 활용 가능함 -> 효율적인 코딩, 에외 상황 최소화 가능



```swift
func someFunction(someOptionalParam: Int?){}

func someFunction(someParam: Int){}

someFunction(someOptionalParam: nil)
someFUnction(someParam: nil) // 불가능
```



- 옵셔널 표현 방법:
  - `!` : 기존 변수처럼 사용가능하나 이 점 때문에 컴파일 오류 가능성이 생김
  - `?` : 기존 변수처럼 사용 불가능. 그래서 옵셔널은 옵셔널 끼리만 연산이 가능함

- ```swift
  // ! : 암시적 추출 옵셔널
  var optionalValue: Int! = 100
  
  switch optionalValue{
    case .none:
    	print("nothing")
    case .some(let value):
    	print("value is \(value)")
  }
  
  // 기존 변수처럼 아무제약없이 활용 가능
  optionalValue = optionalValue + 1 //(O)
  // nil 할당 가능
  optionalValue = nil //(O)
  // 잘못된 접근으로 인한 런타임 오류가 발생할 수 있음
  optionalValue = optionalValue + 1 //오류발생
  
  // ? : 일반적인 옵셔널
  // 동일한 표현임
  var optionalValue: Optional<Int> = nil
  var optionalValue: Int?
  
  switch optionalValue{
    case .none:
    	print("nothing")
    case .some(let value):
    	print("value is \(value)")
  }
  
  // nil 할당 가능
  optionalValue = nil
  // 기존 변수처럼 활용 불가능 (옵셔널과 일반 값은 다른 타입이므로)
  optionalValue = optionalValue + 1 //(1때문에 불가능)
  optionalValue = optionalValue + optionalValue //(X)
  ```



### 옵셔널 추출

> 옵셔널에 들어있는 값을 추출하는 것을 말함

1. **옵셔널 바인딩**

   - nil인지 체크한 후 nil이 아니라면 진행하는 방식

   - 안전하다는 특징이 있음

   - `if - let`을 활용한다

   - ```swift
     func printName(name: String){
     	print(name)
     }
     
     var myName: String? = "Jisu"
     // 컴파일 오류 : printName(myName) 
     
     // 여기서 name 상수는 if let 구문에서만 활용 가능함
     if let name: String = myName {
       printName(name : name)
     } else {
       print("myName is nil")
     }
     
     // 여러 옵셔널 바인딩 하기(두 사람 모두 nil값이 아닌경우) :
     // 여러 옵셔널 바인딩 하기(두 사람 모두 nil값이 아닌경우) :
     var yourName: String! = "Jiyoon"
     
     if let name = myName, let friend = yourName {
       print("\(name) and \(friend)")
     }
     
     if let name: String = myName, let friend : String = yourName {
      print("\(name) and \(friend)")
     }
     ```

2. **강제 추출**

   - nil인지 확인하지 않고 강제로 값을 꺼낸느 방식이다. 만약 nil인 경우 런타임 오류가 생길 수 있다(!와 동일함)

   - ```swift
     var myName: String? = "yagom"
     
     // 강제추출
     printName(myName!) // 가능
     myName = nil
     printName(myName!) // 런타임오류
     ```