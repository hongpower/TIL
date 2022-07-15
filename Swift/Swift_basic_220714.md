# Swift 기초

- camel 문법 사용 
  - swift는 대소문자 구분이 있음

- `print()` : 문자열 출력
- `dump()`: 인스턴스의 description까지 출력

- 상수선언 : `let`
- 변수선언 : `var`



### 1) 선언과 동시에 변수 값 할당하기

- 일반적으로 상수나 변수 선언할 때 데이터 타입까지 명시해줘야하지만, 값의 타입이 명확한 경우라면 데이터 타입은 생략가능하다

  - `let constant: String = "THIS IS STRING"`
  - `var variable: String = "THIS IS STRING"`
  - `constant = "CONSTANT CANNOT BE CHAGNED"` => 불가능
  - `variable = "VARIABLE CAN BE CHANGED"` => 가능

  

### 2) 선언 후 값을 할당하기

```swift
let sum: Int // 만약 선언 후 나중에 값을 할당하려면 무조건 데이터 타입은 명시해야함
let inputA: Int = 100
let inputB: Int = 200

sum = inputA + inputB // 이렇게 상수 sum이 한번 지정되고 나면 변경 불가능
```

 