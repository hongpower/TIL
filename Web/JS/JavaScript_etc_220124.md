# JavaScript

## Scope

> Scope determines the accessibility of variables. 

- ES6부터 let 과 const라는 새로운 키워드를 통해서 block scope 구현 가능해짐

- 3 types of scopes : 

  - global

  - function

  - block :

    - 기존에 변수생성에 사용하던 var를 활용하면, 나중에 그 변수가 생성되었다는 것을 까먹고 같은 이름으로 변수를 생성했을 때 덮여씌여지는 문제가 생기게된다. 이런 현상을 대비해서 block scope가 생겼음.
    - "{}"을 사용해서 block scope를 활용할 수 있음. 괄호 안에 const 또는 let 작성.

    - 대신 함수 작성할 때 {}안에 var작성하는 것이나 const/let은 비슷하겠지 (지역변수니까)
    - let도 마찬가지로 외부에서 생성되면 일반 var와 다른 점이 별로 X

|            | const | let  |
| ---------- | ----- | ---- |
| reupdated  | X     | O    |
| redeclared | X     | X    |

- 예제 : `let sayhello = "hello"` 
- reupdate :  `sayhello = "Hi"` => const는 불가능 let은 가능

- redeclare : `let sayhello = "Hi"` => 둘다 불가능

- redeclare를 거부하는 기능은 나중에 내가 변수명을 까먹고 실수로 같은 이름을 사용해 변수를 사용하게 되었을 때 유용.



## Break Point:

- 중단점이라고도 불림
- 코드를 실행시켰을 때 중단점이 찍힌 곳에서 빌드를 멈춤



## Div VS Span:

- div는 block, span은 인라인
- 활용  방법에도 차이 존재:
  - div는 document의 특정 section을 wrap하고 싶을 때
  - span은 사진이나 텍스트증 작은 portion, 작성된 부분만 wrap할 때 사용