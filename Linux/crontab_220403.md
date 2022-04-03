# crontab할 때 오류들

## 1. python .py 명령 작동안됨

- python filename.py 명령이 terminal에서 직접 입력할 때 됐지만 만약 crontab으로 할 때만 안될 때
- driver와 버전이 다르다는 오류가 뜰 때 



1)  `which python` 으로 python 명령어가 연결되었는는지 확인 (본인은 아무것도 안잡힘)
1)  `which python3`는 잘잡힘



=> python 이라는 soft링크가 안잡힌 것이 이유



#### 해결 방법 :

-  softlink 다시 생성하기
  - `cd usr/bin`
  - `sudo rm python3`
  - `sudo ln -s python3.7 python3`



## 2. 터미널 창 안열릴 때 : 

### 2. 터미널 창 안열릴 때 :

> python버전 업그레이드할 때 많이 생기는 오류라고 함. 앞서 했던 설정때문에 Python3.7하고 pyhon3.8하고 충돌나서 그럼

- 여기서 구글링하다가 화났던거.. 지금 터미널 창을 못켜서 난리인데 터미널 창을 켰다는 전제하에 터미널에 명령어 입력하라고함 
- 단축키 안먹힘 
- shortcuts에 이것저것 설정해도 안됌



#### 터미널 창 여는법 :

- 폴더 열어서 아무 폴더 선택하고 오른쪽 마우스 누르면 Open in Terminal 있음 ^^ 



#### 해결 방법 :

- ```
   1. 우선순위 적용하기
  # sudo update-alternatives --install {link} {name} {path} {priority}
  # /usr/bin/python3의 우선순위를 지정하는 것임
  sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
  sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 2
  sudo update-alternatives --config python3 # 3.7를 선택!
  
  ## 2. gnome-terminal 활용!
  # 가장 추천!
  sudo nano /usr/bin/gnome-terminal
  # 맨 상단에 :
  /usr/bin/python3 => /USR/BIN/python3.8 로 변경하기
  # exit 하는 단축키 밑에 적혀있으므로 해당 단축키 누르고 save하고 나오기
  ```



