# Crawling

- parser란? 문자열을 parsing해줌
- parsing이란? 객체화, 구조화 시키는 것



### 서버에게 응답받기

1. ```python
   import urllib.request
   
   resp = urllib.request.urlopen('url주소')
   ```

2. ```python
   import requests
   
   resp = requests.get('url주소')
   ```

| urllib.request                                               | requests                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 요청메서드 명시 X(urlopen사용). 데이터의 상태에 따라서 get과 post함수 구분 | get 또는 post 방식 선택해서 작성 (요청 메서드 명시)          |
| 없는 페이지 요청시 에러                                      | 에러 X                                                       |
|                                                              | fully restful API 제공                                       |
| 바이너리 형태로 전송                                         | 데이터를 딕셔너리 형태로 보냄, 빌트인 JSON 디코더도 제공     |
|                                                              | 이 방식으로 가져온 텍스트는 HTML모양만 띄고 있지 태그들이 의미를 갖고 있지 않은 단순한 텍스트기 |

:heavy_check_mark: REST

- World Wide Web을 위해 만들어진 표준화된 설계 디자인
- 표준화된 설계 스타일을 가짐으로써 REST 시스템을 가지고 있는 시스템과는 communication이 원활하게 이루어질 수 있음

:heavy_check_mark: RESTful API

- stateless하고 서버와 클라이언트를 분리하는 API (Request하고 돌려받고)
- HTTP 응답을 데이터를 사용하고 접근하는데 사용하는 API

:heavy_check_mark: REST API

- Resource, Verb, Representation으로 구성
- REST 아키텍처의 제약 조건을 준수하는 Application Programming Interface(API)로서 RESTful 웹서비스와의 상호작용이 가능



## Beautiful Soup

- Beautiful Soup 설치 후 

- urllib.request사용시 1번 requests방식 사용시 2번

- 1. `변수 = BeautifulSoup(resp, 'html.parser')` 

- 2. `변수 = BeautifulSoup(resp.text, 'html.parser')`

  - 서버로 부터 응답받은 http text를 파싱해서 BeautifulSoup 객체로 만듬
  - request와 달리 requests로 가져온 데이터는 꼭 끝에 .text를 작성 해주어야함!



### Beautiful Soup 객체에서 원하는 데이터 가져오기

- `from bs4 import BeautifulSoup`

- 현재 `soup = BeautifulSoup(resp, 'html.parser')` 로 soup 객체가 만들어진 상황
- 지금 soup라는 객체는 resp의 url의 html파일 텍스트로만 긁어온 데이터를 다시 구조화시킨 상태



- `soup객체.find_all('element이름', 조건 )` => 여러개를 찾기 때문에 리스트, array 형태라 생각하면 됨
  - `soup.find_all('dl', class_='food')` => food라는 이름의 클래스를 가진 dl 태그를 불러옴
  - `soup.find_all('ul', {'class':'food'})`=> 이런식으로 해당 태그 속성을 딕셔너리 형태로 줄 수 있음. 윗 코드랑 동일한 결과
- `soup객체.find('element이름', 조건)` => 해당 조건의 젤 첫번 째 태그를 찾아옴
  - `soup.find('a',class_='num')` => num이라는 class를 가진 젤 첫번 째 a태그
- `soup객체.get_text()` => 해당 객체의 text 추출
  - `movie.find('a').get_text()`
  - `soup객체.text`  => 이것과 같은 결과
- `soup객체.select('CSS선택자')` => CSS선택자 사용
  - `webtoons.select('span[class=title]')` => CSS선택자로 조건 주기
- `soup객체['속성']` => 해당 속성의 값 가져오기
  - `dl.find('a')['title']` => dl이라는 soup객체 내에 a태그중 title이라는 속성의 값 가져옴

soup객체라는 것은 방금 지정한 `soup` 도 soup 객체이고, soup객체를 BeautifulSoup의 함수들을 활용해서 filtering한 soup객체도 soup객체에 포함됨!

:heavy_check_mark: 만약 가져온 데이터가 너무 더럽고 띄어쓰기가 많다면 strip 사용!



soup로 원하는 데이터 긁어온 예시:

- 페이지 넘버만 문자열 형태로 가져오기
- `list(filter(None, map(lambda x: x.text if x.text.isdigit() else None, paging)))` => paging이라는 변수(iterable형태임)의 요소들 text가 문자형태인 숫자인지 확인하고 맞다면 text를 저장하고 아닌 경우는 None을 저장한 뒤, None이라는 값은 filter한 상태로 리스트로 만듬



#### JSON 파일 만들기

- json은 딕셔너리 내에 리스트가 있고 그 리스트안에 또 딕셔너리가 있는 형태

- 파이썬 내에서 json형태를 만들고 난 후에

- `변수이름 = json.dumps(json형태의변수)` => json객체를 text로 변경

  - `res_json = jsondumps(res, ensure_ascii=False)`

    - 여기서 res라는 변수는 지금 json의 structure를 가지고 있음
    - 한글이 ascii코드로 변환 되는 것을 막기 위해 ensure_ascii를 false로 지정

- ```python
  import json
  
  with open('practice.json','w',encoding='utf-8') as f:
  	f.write(res_json)
  ```

  => res_json이라는 json 객체를 담음 'practice.json' 이라는 파일 생성



## Element Tree

> xml파일 가져올 때 사용하는 패키지로 이미 파이썬 내에 내장되어있음

- `from xml.etree import ElementTree`



#### 공공데이터 파일로 연습해보기

- https://www.data.go.kr/ 여기서 원하는 데이터를 신청한다
- 승인된 데이터의 일반 인증키(개인마다 고유하게 가진 허가 키)를 변수에 담아준다
- 활용 가이드를 참고해서 또 다른 변수에 일반인증키를 포함한 url을 작성해준다

예)

```python
# 일반 인증키
service_key = '블라블라블라블라'

# url 주소
url = f'http://블라블라블라....?ServiceKey={service_key}'
```

- `resp = requests.get(url)`=> 해당 url 주소 가지고 마찬가지로 응답 받아준다

- `tree = ElementTree.fromstring(resp.text)` => 텍스트파일인 응답 받은 데이터를 구조화 시킨다 (parsetree로 변환)



#### 정규식 연습:

- 정규식은 특정한 규칙을 가진 문자열의 집합 표현하는데 사용하는 형식 언어
- 파이썬은 re모듈이라고 정규 표현식 지원하는 모듈 제공
- `import re` 
- `re.sub` => 어떤 패턴에 맞는 문자 대체
  - `re.sub(pattern, new_text, text)` => text에서 pattern에 맞는 부분을 new_text로 변경해라
- `r''` => 문자열 앞에 r을 붙여주면 raw 문자열이 되어서 일일이 '\\'라는 특수문자 처리를 해주지 않아도 특수 문자를 그대로 판단 가능. 
- 문자 클래스 :` []`
  - `[]` 사이의 문자들과 매치라는 의미
  - `[abc]` => 'a'는 정규식과 일치하는 문자인 'a'가 abc에 포함되어있으므로 매치, 'before'는 'b'가 있으므로 매치 'dude'라는 문자는 abc중 하나도 없으므로 매치 X
  - `[a-c]` : a부터 c까지, `[1-5]` : 1부터 5까지
  - 만약 문자 클래스 내에 ^를 사용한다면 반대(not)을 의미
  - `[^0-9]` 숫자가 아닌 문자만 매치
  - `\d` -> 숫자와 매치 , `[^0-9]`와 동일
  - `\D`-> 숫자가 아닌 것과 매치
  - `\s`-> whitespace 문자와 매치
  - `\w` -> 문자 + 숫자와 매치. 
  - d, s, w의 소문자가 대문자가 되면 `^`를 포함한 것과 같음

- `re.sub(r'(\D)+','',item.find('stdDay').text)` => stdDay를 찾아 text에 해당하는 부분이 숫자가 아닌 데이터라면 공백으로 만들어라. 즉 stdDay중 숫자만 추출해라
  - 만약 데이터가 `<stdDay>2022년 02월 09일 00시</stdDay>` 라면 해당 텍스트에서 2022020900만 추출



## Selenium

- `from selenium import webdriver`

- 내 크롬 버전과 맞는 webdriver를 selenium에서 다운 받은 후에 압축파일을 풀어서 chromedriver.exe를 원하는 경로에 삽입(나 같은 경우는 py파일이 작성된곳 상위 폴더에 drivers라는 새폴더를 만들어서 삽입했음)

  ```python
  from selenium import webdriver
  
  # 절대경로 or 상대경로로 내 webdriver 위치 제공
  service = webdriver.chrome.service.Service('web드라이버위치 경로')
  
  # 크롬 객체 생성.
  driver = webdriver.Chorme(service = service)
  ```

  - 이전 버전까지는 `driver = webdriver.Chrome('web드라이버위치 경로')` 가 가능했지만 버전 업데이트 이후로 이렇게 작업 진행해야함

- 이렇게 작업을 끝내놓으면 크롬 드라이버가 자동으로 브라우저를 control이 가능해진다.



#### 메소드/속성

- `driver.implicitily_wait(num)` => 몇초간 기다렸다가 실행
- `driver.get(url) ` => 해당 url을 get 방식으로 보관해서 저장하고 또 새로운 크롬창으로 열어준다. 항상 이렇게 get먼저 해줘야지 여기서 원하는 데이터를 추출 가능하겠지?
- `driver.page_source` =>get방식으로 저장한 url의 page source 즉 html문서를 가져온다
  - `soup = BeautifulSoup(driver.page_source, 'html.parser')` 이렇게 가져온 html텍스트를 soup객체 생성 가능

:heavy_check_mark: 응답이 재빠르게 되지 않을 것을 대비해서:

​	`from time import sleep` : sleep이라는 함수 import 해서, `sleep(2)` 2초뒤에 응답한 파일 로드? 가능

- `driver.find_element(By.NAME, 'username')` => 이름이 username인 첫번 째 element를 찾는다
  - find_element말고 find_elements도 있음

:heavy_check_mark:__By를 쓸거면 꼭꼭 `from selenium.webdriver.common.by import By ` 꼭 해주자!

:heavy_check_mark: By에는 NAME도 있고, CSS_SELECTOR, XPATH도 있음

- 예)`driver.find_element(By.CSS_SELECTOR, '#loginForm > div > div:nth-child(3)').click()`  => 로그인폼이라는 id를 가진 div 자식에 3번째 자식이 div라면 click해줘!

- __nth-child는 인덱스가 1부터 시작하고, 만약 해당 자식이 지정한 태그가 아니라면 값을 가져오지 않음 !! div태그의 자식을 찾는게 아니라 상위 태그의 지정한 숫자의 자식이 div태그인지 확인하는 것임__
- `driver.find_element(By.XPATH, ''XPATH)` : XPATH는 해당 웹페이지의 개발자 도구에서 원하는 태그를 누르고 오른쪽 키의 Copy에 Copy의 Copy Xpath(절대경로)가 있음

- `driver객체.send_keys()`
  - 예) `pw = driver.find_element(By.NAME, 'password')`
  - `pw.send_keys('비밀번호')` => 해당 url의 password라는 이름을 가진 element에 '비밀번호' 값 작성
- `driver.refresh()` => 새로고침
- `driver.quit()`=> 창 닫기

- 만약에 함수를 작성했는데 밑줄이 그어져있다면 deprecated된 것이라 사용하지 말라는 뜻



## 기타

- 만약 password와 같이 secure하게 보관해야하는 경우가 있다면, OS에 환경설정하는 모듈이 있는데 이걸 사용하면 github같은 데에 올려도 안전
