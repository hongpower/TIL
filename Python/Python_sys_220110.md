# Python

### sys.stdin.readline:

- input()대신 sys.stdin.readline()을 사용하는 이유:
  - 만약 반복문으로 긴 줄을 입력 받아야할 때 input()으로 입력을 받는다면 시간 초과 발생이 가능하다.
  - 한줄 단위로 입력받는다는 특징 때문에 개행문자(\n)가 같이 입력받아짐
  - 입력한 한 라인을 iterable한 컨테이너에 저장
  - std stands for standard input

```python
import sys
# 입력값이 한 줄 일때 문자열로 저장:
a = int(sys.stdin.readline()) 

# 입력값이 여러개일 때 하나씩(띄어쓰기 포함) 출력:
for i in sys.stdin.readline():
    print(i)

# 입력값을 리스트로 저장 (개행문자 빠짐)
list(map(int,sys.stdin.readline().split()))
```

