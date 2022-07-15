# 반복문



- `for - in`

- ```swift
  for integer in integers {
    print(integer)
  }
  
  // 딕셔너리인 경우(튜플 활용)
  for (name, age) in people {
    print("\(name) : \(age)")
  }
  ```

- `while`

  - 소괄호는 필수 x

- ```swift
  while (integers.count > 1){
  	integers.removeLast()
  }
  
  while integers.count > 1{
    integers.removeLast()
  }
  
  //while 1 은 불가능!
  ```

- `repeat - while`

- ```swift
  // repeat의 중괄호 먼저 활용하고 while을 확인함. 즉 while과 순서가 반대임.
  repeat {
  	integers.removeLast()
  } while integers.count > 0
  ```