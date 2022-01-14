## 모듈(Module)

1. 모듈 전체 참조 : import 모듈명
2. 모듈 내에서 일부 참조(추천): from 모듈명 import 변수명 or 함수명

​	ex) from random import randint, randrange

3. 스페셜 변수/매직 메서드를 제외한 모든 것을 참조 : from 모듈명 import *



if \_\_name\_\_ == '\_\_main\_\_' :  -> 만약 지금 킨 창이 main이라면, 즉 이 창을 아무도 참조하고 있지 않다면 실행

만약 다른 위치에 있는 모듈을 참조하고 싶다면, settings에 추가 가능