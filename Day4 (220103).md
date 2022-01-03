# Python

## Module

if \_\_name\_\_ == '\_\_main\_\_' :  -> 만약 지금 킨 창이 main이라면, 즉 이 창을 아무도 참조하고 있지 않다면 실행

만약 다른 위치에 있는 모듈을 참조하고 싶다면, settings에 추가 가능

## File

> 굉장히 어렵다.. 죽을 것 같다 책 내용도 추가했다

- 메모장에 작성 :

  ```python
  f = open('data.txt','w') #open파일명에 절대경로 작성도 가능
  f.write('내용')
  f.close()
  ```

- 메모장에서 읽기:

  ```python
  f = open('data.txt','r')
  data = f.read()
  f.close()
  ```

:heavy_check_mark:만약 한글이 메모장안에 적혀있다면 open함수 옆에 encoding = 'utf-8'을 작성하거나 저장할때 encoding을 utf로 바꿔주면됨.

- readline():

  - 첫째 줄만 읽어오기:

    ```python
    f = open('test.txt','r')
    line = f.readline()
    print(line)
    f.close()
    ```

    - 한줄씩 끝까지 읽어오기:

      ```python
      f = open('test.txt','r')
      while True:
      	line = f.readline()
      	if not line:
      		break
      	print(line,end='')
      f.close()
      ```

- readlines():

  - 전체라인을 읽어오며, 리스트로 반환함. 한 리스트의 요소는 한 행.

  - 한 리스트로 출력:

  ```python
    f = open('test.txt','r')
    lines = f.readlines()
    print(lines)
    f.close()
  ```

  - 리스트의 요소들을 하나씩 출력:

  ```python
  f = open('test.txt','r')
  lines = f.readlines()
  for line in lines:
       print(line, end='')
  f.close()
  ```
  
  - 또 다른 리스트의 요소를 하나씩 출력하는 방법:
  
  ```python
  f = open('test.txt','r')
  for line in f : #f.readlines()와 같은 방식
  	print(line, end='')
  f.close()
  ```
  
  

- seek(offset, whence) :

  - offset : 상대위치; 시작 위치로부터 열의 위치
  - whence : 시작위치; 
    - 0: 파일시작위치 1: 현재위치 2: 파일의 끝
  - seek(0,0) : 시작위치로부터 0열의 위치
  - seek(10,0) : 시작위치로부터 오른쪽으로 10열 이동한 위치

  :heavy_check_mark:seek는 1바이트씩 읽는데, 한글은 한 글자당 2바이트기 때문에 짝수로 써야지 정상적으로 출력 가능

- 메모장에서는 한줄 바꿀 때 마다:

  - \n : line feed ; 줄바꿈
  - \r : carriage return ; 그 줄의 맨 첫번째로 이동

  

- read() : 텍스트 파일 내용 전체를 읽어 1개의 문자열로 반환

- txt파일 끝에 내용 추가:

  ```python
  f = open('test2.txt', 'a')
  appendData = '\n\nPythong Programming'
  f.write(appendData)
  
  f = open('test2.txt','r')
  print(f.read())
  f.close()
  ```



- with문 : with문이 종료되면 파일객체는 자동으로 close()

  - with open('파일명','열기모드') as 파일객체 : 

    ​		수행코드

  ```python
  with open('test3.txt','w') as f:
  	f.write('hello')
  ```

  ```python
  file = 'test3.txt'
  data = 'Python'
  
  with open(file,'a') as f :
  	f.write('\n' + data)
  ```

  



