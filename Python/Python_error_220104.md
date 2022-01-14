#  Python

## Error

참고: [ErrorTypes](https://docs.python.org/ko/3/library/exceptions.html)

- ZeroDivisionError : 0으로 나눈 경우
- TypeError: 부적절한 형의 연산이나 함수 실행
- NameError : 존재하지 않는 변수를 선언할때
- ValueError: 연산이나 함수가 올바른 형이지만, 부적절한 값을 가진 인자를 받았을 때
  - 예) print('%d%' %b)
- SyntaxError : 문법 오류, 콜론 빼먹기
- IndexError: 없는 인덱스 추출
- UnboundLocalError: NameError의 서브 클래스로, 함수나 메서드에서 특정 지역 변수를 참고하는데 그 변수에 값이 연결되어있지 않은 경우
- ModuleNotFoundError : module이 존재하지 않거나 경로가 잘못된 경우
- FileNotFoundError : 열고자 하는 파일이 없는 경우
- OSError : 경로명을 잘못 쓴 경우
  - 예) f = open('c:\\test.txt') -> 역슬래쉬가 쓰임. 역슬래쉬를 쓰고 싶으면 2번 연속 작성!



예외 처리 : Error 발생했을 때 프로그램 중단하지 않고 실행 가능

형식:

```python
try: 
	에러가 발생할 문장들, 이 문장을 시도해봐라
except:
	에러가 발생했을 때 처리하는 코드
else:
	에러가 없을 때 처리하는 코드
finally:
	에러 여부와 관계없이 수행하는 코드
```

- except 뒤에는 Error명을 작성하면 후에 이점이 많다
  - 예) except TypeError
  - ​      except NameError as e

