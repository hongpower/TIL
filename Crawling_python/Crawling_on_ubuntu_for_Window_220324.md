# Crawling using Selenium on ubuntu(VM) for Windows

- 그냥 크롬만 설치하고 드라이버 돌리면 끝 !



### 사용정보 :

- 사용하는 VM : VMware
- 사용하는 브라우저 및 드라이버 : 크롬 또는 파이어폭스
  - 파이어폭스는 브라우저 설치 안해도 돼서 간편



## Chrome 사용 :

### 1. 크롬 브라우저 다운

- `wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb` 
- 또는 크롬 공식 웹사이트에서 ubuntu amd64로 설치



### 2. 크롬 설치

- `sudo apt install ./google-chrome-stable_current_amd64.deb`



### 3. 크롬 버전 확인

- `google-chrome --version` => 본인은 99.-.4844.92 뜸



### 4. 크롬 드라이버 설치

- https://chromedriver.storage.googleapis.com/index.html 접속해서 해당 버전 드라이버 설치 링크 복사
- `wget https://chromedriver.storage.googleapis.com/99.0.4844.51/chromedriver_linux64.zip`



### 5. 실행

- `vim test.py`

- ```python
  from selenium import webdriver
  
  service = webdriver.chrome.service.Service('driver위치')
  driver = webdriver.Chrome(service=service)
  
  driver.get("https://www.naver.com")
  ```

- `python test.py`



## Firefox 사용:

> firefox는 deprecated된 executable_path 사용. deprecated됬지만 잘작동함



### 1. 파이어폭스 버전 확인

- 파이어폭스 접속
- ![image](https://user-images.githubusercontent.com/96896873/159913018-c5ab5168-877d-4c7b-b031-6a6aaf4287ec.png)

- 오른쪽 상단 더보기 클릭후
- Help 클릭
- About Firefox 클릭
- ![image](https://user-images.githubusercontent.com/96896873/159913036-7cb436c1-961e-4f4e-98fc-9d3e9cac7261.png)

- 버전 98.0.1 사용중



### 2. 드라이버 설치

- https://github.com/mozilla/geckodriver/releases 접속
- ![image](https://user-images.githubusercontent.com/96896873/159913067-09700983-842f-408c-8a02-5e6784450d02.png)

- 설치!



### 3. 드라이버 압축 해제 후 원하는 경로로 이동

- 이번에는 그냥 gui 활용해서 다운받은 파일 내에 있는 geckodriver 원하는 경로로 이동시켜놓자



### 4. 연습

- `vim test.py`

- ```python
  from selenium import webdriver
  
  driver = webdriver.Firefox(executable_path='driver위치')
  
  driver.get("https://www.naver.com")
  ```

