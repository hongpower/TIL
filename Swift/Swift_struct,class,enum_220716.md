## 구조체

> swift에서는 대부분의 type이 구조체로 이루어짐

- 대문자 카멜케이스 활용

- Swift의 대부분의 큰 뼈대가 구조체로 구성

  - Int나 String같은 데이터타입도 대부분 구조체임

- `struct 이름 { 구현부 }`

- ```swift
  // 구조체 정의 :
  struct Student{
    // 1) 프로퍼티 
    // 가변 프로퍼티
    var name: String = "unknown"
    // 불변 프로퍼티 
    var job: String = "student"
    // 타입 프로퍼티
    static var school: String = "high school"
    // 만약 class라는 이름을 활용하고 싶은데, 이미 키워드가 있는경우에는 백틱(``)활용하면 된다
    var `class`: String = "Swift"
    
    // 2) 메소드
    // 인스턴스 메소드
    func sayHello(){
      print("My name is \(name) and studying \(self.class)") // self는 인스턴스 자신을 지칭하는데 class처럼 이미 있는 키워드 활용 경우를 포함해서 몇몇 경우가 아니라면 사용은 선택사항임.
    }
    // 타입 메소드
    static func introduce(){
      print("My school is yangjae school")
    }
  }
  
  // 구조체 활용 :
  // 타입메소드/타입프로퍼티
  Student.introduce()
  print(Student.school)
  Student.school = "middle school"
  print(Student.school)
  
  // 인스턴스메소드/인스턴스프로퍼티
  // 인스턴스 생성:
  var jisu: Student = Student()
  
  jisu.name = "jisu"
  jisu.sayHello()
  jisu.class = "Python"
  jisu.sayHello()
  
  // 불변 인스턴스 생성시 변경 불가능:
  let unknown: Student = Student()
  
  //unknown.name ="jiyoon"
  unknwon.sayHello()
  ```

- 불변 인스턴스는 가변 프로퍼티 **변경 불가능**



## 클래스

> 구조체와 매우 유사하지만 구조체는 값타입이고 클래스는 참조(reference)타입임. swift에서 클래스는 다중 상속이 되지 않는다

- 대문자 카멜케이스 활용

- 전통적인 OOP 관점에서의 클래스며 **단일 상속**이 가능

- Apple 프레임워크의 대부분의 큰 뼈대가 클래스로 구성됌

- 클래스 VS 구조체

  - | 클래스                                                       | 구조체                                          |
    | ------------------------------------------------------------ | ----------------------------------------------- |
    | 참조타입                                                     | 값타입                                          |
    | 불가변 인스턴스더라도 가변 프로퍼티는 변경 가능              | 불가변 인스턴스라면 가변 프로퍼티도 변경 불가능 |
    | 타입 메소드에 상속시 재정의가 가능/불가능한 2가지 메소드가 있음 | 상속이 불가능                                   |

  - ```swift
    class Sample {
      // 인스턴스 메소드
      func instanceMethod() {
        print("instance method")
      }
      // 타입 메소드
      // 재정의가 불가:
      static func typeMethod() {
        print("type method  - static")
      }
      
      class func classMethod() {
        print("class method - class")
      }
    }
    
    // 클래스 인스턴스 생성:
    var classInstance: Sample = Sample()
    ```



## 열거형

> 열거형은 유사한 종류의 여러 값을 한 곳에 모아서 정의한 것이다. 요일, 월, 계절 등이 있다. enum으로 표현한다. 

- enum -> 대문자 카멜케이스(대문자로 시작하는 카멜케이스)

- case -> 소문자 카멜 케이스 (각 case는 그 자체가 고유의 값이므로 각 case에 자동으로 정수 값이 **할당되지 않는다**)

  - case는 한 줄에 개별로도, 여러개도 정의 가능하다

- ```swift
  enum 이름 {
    case 케이스1
    case 케이스2, 케이스3, 케이스4 //이것도 가능하다
  }
  
  // 예)
  enum BoostCamp{
    case iosCamp
    case webCamp
    case androidCamp
  }
  ```



- 처음 선언시에는 무조건 **열거형 타입 명시**해줘야함

- ```swift
  // <열거형 사용>
  
  enum WeekDay{
    case mon
    case tue
    case wen
    case thu
    case fri, sat, sun
  }
  
  // 열거형 타입과 케이스 모두 활용해서 정의하기 :
  var day: WeekDay = Weekday.mon
  
  // 한번 열거형으로 정의된 변수 같은 경우에는 열거형 타입 굳이 선언하지 않고 케이스 처럼 표현해도 무방하다
  day = .tue
  
  var day2: 
  
  // 열거형은 switch와 활용 자주된다:
  switch day{
    case .mon, .tue, .wen, .thu:
    	print("힘내세요")
    case .fri, .sat:
    	print("불금, 불토")
    case WeekDay.sun: //이렇게도 작성가능
    	print("쉬세요")
  }
  ```



- 원시값

  - C언어의 enum처럼 정수값을 가질 수 ㅇㅆ다.
  - 원시값은 case별로 **고유의 값**을 가져야한다
  - **자동으로 1이 증가된 값**이 할당된다
  - rawValue는 옵션이기 때문에 굳이 작성하지 않아도 된다
  - 정수 뿐만 아니라 **Hashable 프로토콜을 따르는 모든 타입을 원시값의 타입으로 지정할 수 있다**
  - 만약 특정 변수를 rawValue의 원시값으로 초기화 했다면 **optional타입**으로 지정이 된다. 

- ```swift
  import UIKit
  
  // 원시값이 필요한 경우에는 'enum 이름: 데이터타입 {}' 형식으로 작성
  enum Fruit: Int {
    case apple = 0
    case grape = 1
    case peach // 자동으로 2가 할당된다
  }
  
  print(Fruit.peach.rawValue) // 0출력
  
  
  // let apple: Fruit = Fruit(rawValue: 0) -> 불가능 : 왜냐하면 rawValue가 0인 case가 없을 수도 있으므로 옵셔널 타입으로 지정이 되어서 무조건 ?를 붙여줘야한다
  let apple: Fruit? = Fruit(rawValue: 0)
  
  if let someFruit: Fruit = Fruit(rawValue: 5){
    print("\(someFruit) exists")
  } else {
    print("no case for 5")
  }
  
  // String을 원시값으로
  enum School: String {
    case e = "elementary"
    case m = "middle"
    case h = "high"
    case u
  }
  
  // 만약 원시값이 정의되지 않았다면 그냥 case를 출력한다
  print(School.u.rawValue) // u출력.
  
  // 메서드도 추가 가능하다
  enum Month{
    case dec, jan, feb
    case mar, apr, may
    case jun, jul, aug
    case sep, oct, nov
    
    func printMessage(){
      switch self { // self를 통해 자기자신의 case 확인 가능하다
      case .mar, .apr, .may:
            print("spring")
      case .jun, .jul, .aug:
            print("summer")
        case .sep, .oct, .nov:
            print("fall")
        case .dec, .jan, .feb:
            print("winter")
      }
    }
  }
  
  let mar: Month = Month.dec
  mar.printMessage()
  ```



## 클래스, 구조체, 열거형

| 클래스       | 구조체        | 열거형        |
| ------------ | ------------- | ------------- |
| 참조타입     | 값타입        | 값타입        |
| 상속가능     | 불가능        | 불가능        |
| 익스텐션가능 | 익스텐션 가능 | 익스텐션 가능 |



- 참조 VS 값

  - 참조는 데이터를 전달할 때 *메모리 위치* 전달함

  - 값은 데이터 전달 때 복사해서 전달함

  - ```swift
    import UIKit
    
    struct ValueType{
      var property = 1
    }
    
    class ReferenceType{
        var property = 1
    }
    
    // 구조체 :
    // let firstStructType: ValueType = ValueType()와 동일함
    let firstStructType = ValueType()
    var secondStructType = firstStructType
    secondStructType.property = 2
    
    print("firstStructType : \(firstStructType.property)")
    print("secondStructType : \(secondStructType.property)")
    
    // 클래스 :
    let firstClassReference = ReferenceType()
    let secondClassReference = firstClassReference
    secondClassReference.property = 2
    
    print("firstClassType : \(firstClassReference.property)")
    print("secondClassType : \(secondClassReference.property)")
    ```



## 