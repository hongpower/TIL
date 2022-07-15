# Function

> 함수는 하나의 데이터타입으로도 표현가능함

- `func 함수이름(매개변수1이름 : 매개변수1 타입, 매개변수2이름 : 매개변수2타입)-> 반환타입 {}`

```swift
// 일반
func sum(a: Int, b: Int) -> Int {
  return (a + b)
}

// 반환값이 없을 때 1
func printMyName(name: String) -> Void {
	print(name)
}

// 반환값이 없을 때2
func printMyName2(name: String){
  print(name)
}

// 매개변수가 없을 때
func sayHello(){
  print("Hello, World")
}

// 호출
sum(a: 3, b: 5)
printMyname(name: "Jisu")
printMyname2(name: "Jiyoon")
sayHello()
```



- 매개변수 활용방법: `\(매개변수이름)`

- 매개변수 기본값 : 매개변수가 디폴트로 갖고 있음. 매개변수 목록 중 맨 뒤에 위치시키는 것이 좋음
- 전달인자 레이블 : 매개변수 역할을 좀 더 명확하게 하거나 함수 사용자의 입장에서 표현하고자 할 때 사용
  - `func 함수이름(전달인자레이블 매개변수1이름 : 타입, 전달인자레이블 매개변수2이름 : 타입)`
  - 동일한 함수명이 있더라도 전달인자 레이블이 붙는 순간 다른 함수로 인식해서 같은 함수명을 활용할 수 있다
  - 함수 호출시에는 **전달인자 레이블을 활용하고**
  - 함수 내부에서 인자를 활용할 때는 **매개변수를 활용**한다
- 가변 매개변수 : 전달 받을 값의 개수를 알기 어려울 때 사용 가능
  - 가변 매개변수는 **함수당 하나**만 가질 수 있고 맨 뒤에 위치시키는 것이 좋음
  - `...`을 활용한다
  - `func 함수이름(매개변수1이름: 매개변수1타입, 전달인자 레이블 매개변수2이름: 매개변수2타입 ...)`

```swift
// 매개변수 기본값

func greeting(friend: String, me: String = "jisu"){
	print("Hello \(friend) I am \(me)")
}
greeting(friend: "hannah")
greeting(friend: "jiyoon", me: "hannah")


// 전달인자 레이블
func greeting(to friend: String, from me: String = "jisu")
{
  print("Hello \(friend) I am \(me)")
}
greeting(to: "hannah", from "jiyoon")

// 가변 매개변수
func sayHelloToFriends(me: String, friends: String ...) -> String{
  print("Hello \(friends)!")
}
sayHelloToFriends(neL "jisu", friends "hana", "eric", "jiyoon")
```



- swift에서는 함수가 데이터타입으로 사용될 수 있음
  - **반환 타입이 생략이 불가능하다**
  - **매개변수의 타입또한 동일해야한다**

```swift
func greeting(to friend: String, from me: String = "jisu")
{
  print("Hello \(friend) I am \(me)")
}

// 1) 변수 선언을 통해서 함수 호출하기
// someFunction으로 들어온 두개의 인자가 greeting의 매개변수의 인자로 각각 넘어가는 것임
// (데이터 타입, 데이터타입) -> 이것도 앞서 array같은 것들에서 활용했던 shorter version이라 생각하면 됨
var someFunction: (String, String) -> Void = greeting(to:from:)
someFunction("jiyoon", "jisu")

// 2) 함수 선언을 통해서 다른 함수 호출하기
func runAnother(f: (String, String) -> Void){
  f("jenny", "mike")
}
runAnother(f: greeting(friend, me))
runAnother(someFunction)
```