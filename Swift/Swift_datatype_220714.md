# 데이터 타입

### 1) Bool

var variable: Bool = true

- Bool = 0 이런식으로는 불가능

### 2) Int, UInt

- Int : 정수 타입, 64비트 정수형
- UInt : 양의 정수 타입, 64비트 양의 정수형

`var someUInt: UInt = -100` =>컴파일 오류

### 3) Float, Double

- Float : 실수 32비트
- Double : 64비트 실수

### 4) Character, String

- Character : 문자 타입, 유니코드 사용, **큰 따옴표** 사용
- String: 문자열 타입, 유니코드 사용, 큰 따옴표 사용

## Any, Anyobject, nil

- Any - 스위프트의 모든 타입을 지칭하는 키워드
  - var someAny: Any = 100 -> 모든 타입을 수용가능함
    - someAny = "String"
    - someAny = 123.12
- Anyobject : 클래스의 인스턴스만 가질 수 있음
  - class SomeClass {}
  - var someAnyObject: AnyObject = SomeClass()
- Nil : Any에는 nil은 못들어옴. 모든 데이터타입이지만 특정 데이터타입이 들어가야하니. SomeAny도 마찬가지임

## Array

- 변수선언할 때 괄호를 열고닫는건 인스턴스 생성의 의미임

```swift
// 괄호로 array내 데이터 타입비정
// 배열 생성
var integers: Array<Int> = Array<Int>()
// 밑에 것들도 다 동일하게 빈 array표현함
//var integers: Array<Int> = [Int]()
// var integers: [Int] = [Int]()
// var integers: [Int] = []

integers.append(1)
integers.append(100)
integers.append(100.1) //이거는 불가능

integers.contains(100)

integers.remove(at: 0) // 0번째 인덱스 삭제
integers.removeLast()
integers.removeAll()

integers.count
```

```swift
// 이렇게 한번에 선언과 동시에 값 할당 가능(데이터 타입 명시 불필요)
let immutableArray = [1, 2, 3]
```

## Dictionary

```swift
// Dictionary<키, 값> 
// [키:값]
var anyDictionary: Dictionary<String, Any> = [String: Any]()
anyDictionary["someKey"] = "value"
anyDictionary["anotherKey'] = 100

anyDictionary // 출력가능

// 둘다 유사한 표현 (둘다 딕셔너리에서 삭제)
anyDictionary.removeValue(forKey: "anotherKey")
anyDictionary["someKey"] = nil

// 다르게 dictionary 생성방법
// 빈딕셔너리:
let emptyDictionary: [String:String] = [:]
// 초기화된 딕셔너리
let initializedDictionary: [String:String] = ["name" : "jisu", "gender" : "female"] 

```

- 보너스 : `let someValue: String = initializedDictionary["name"]` 이거는 안되는 이유가 name이라는 키의 값이 String이라고 하더라도 값이 할당안될 수도 있는 불확실성때문에 불가능

## Set

```swift
// 축약문법이 없음
var integerSet: Set<Int> = Set<Int>()
integerSet.insert(1)
integerSet.insert(99)
integerSet.insert(99) //불가능

integerSet.contains(1)

integerSet.remove(100)
integerSet.removeFirst()

integerSet.count

let setA: Set<Int>= [1,2,3,4,5]
let setB: Set<Int> = [3,4,5,6,7]

// 활용법 : (집합)
let union: Set<Int> = setA.union(setB) // 합집합
let sortedUnion: [Int] = union.sorted() // 정렬해서 리스트에 넣음
음
let intersection: Set<Int> = setA.intersection(setB)
let subtracting: Set<Int> = setA.subtracting(setB)
```



