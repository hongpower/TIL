# Installing Anaconda on Mac

1. https://www.anaconda.com/products/distribution/download-success-2 설치

2. 확인하기 : zsh로 들어왔을 때 자동으로 아나콘다 기본 환경(base)가 설정돼있음

- 중간에 아나콘다 navigator는 2.1.4로 업데이트했음

3. 가상환경 생성 : conda create -n book_project python=3.9
   - environment location : /Users/jisu/opt/anaconda3/envs/book_project/bins/python.exe

4. 가상환경 활성화 : conda activate book_project

5. 파이참들어가서 가상환경 위치가 자동으로 안잡힌다면 environment location지정해서 가상환경 만들기



이때까지 book_project에 설치한 파일(zsh):

- pip install bs4

- pip install requests

- pip install selenium
  - https://chromedriver.storage.googleapis.com/index.html?path=100.0.4896.60/ 크롬 드라이버 설치

conda deactivate