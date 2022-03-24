# Crawling using Selenium on ubuntu(VM) for M1 Macbook

- M1이 arm64라 ubuntu arm을 지원하는 브라우저와 드라이버를 찾기가 어렵다
- 하루내내 투자한 결과 찾아냈음....



### 사용정보 :

- 사용하는 VM : UTM
  - 해보지는 않았지만 utm64 ubuntu를 제공하는 모든 가상서버는 99%로 가능할듯
- 사용하는 브라우저 및 드라이버 : 크롬



### 1. chromium-browser 설치

- 공식 크롬 웹사이트에서 chrome for ubuntu arm은 없음 

  - m1 arm은 있지만 동일한 arm이라하더라도 다른 architecture를 가지고 있기 때문에 ubuntu arm 전용 chrome browser만 사용가능

  -  chrome 대신 **chromium** 사용해야함

- `sudo apt install chromium-browser`



### 2. selenium 설치

- `pip install selenium`



### 3. chromium 버전 확인

- `chromium-browser --version` => 본인은 99.0.4844.51 나옴



### 4. 해당 버전에 맞는 드라이버 선택하기

-  [http://ports.ubuntu.com/pool/universe/c/chromium-browser/](http://ports.ubuntu.com/pool/universe/c/chromium-browser/)에서 chrome버전에 일치하는 arm64.deb 선택해서 설치
- 본인은 `chromium-browser_99.0.4844.51-0ubuntu0.18.04.1_arm64.deb` 선택

- 선택한 버전 드라이버 클릭해서 copy address



### 5. 드라이버 설치

- 복사한 설치주소를 `wget` 명령어를 통해 드라이버 설치
- `wget http://ports.ubuntu.com/pool/universe/c/chromium-browser/chromium-browser_99.0.4844.51-0ubuntu0.18.04.1_arm64.deb`



### 6. 드라이버 압축 해제

- `sudo dpkg -i chromium-browser_99.0.4844.51-0ubuntu0.18.04.1_arm64.deb `



### 7. 드라이버 확인

- `/usr/lib/chromium-browser/`에 chromedriver 존재한다면 성공한 것임!



### 8.  Selenium 실행해보기

- `vim practice.py`

- ```python
  from selenium import webdriver
  
  driver = webdriver.Chrome('/usr/lib/chromium-browser/chromedriver')
  driver.get('http://naver.com')
  ```



