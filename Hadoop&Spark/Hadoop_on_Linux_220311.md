# Linux

## Hadoop

### SSH server

- SSH server & ssh-askpass 설치 :
  - hadoop core는 slave node와 namenode가 커뮤니케이션할 때 ssh server가필요하기 때문에 설치가 필요하다
  - `sudo apt install openssh-server ssh-askpass -y` 
  - SSH server는 신뢰할 수 없는 네트워크 상에서 컴퓨터끼리 데이터를 안전하게 교환하기 위한 프로토콜로 아이디 & 비번 없이 열쇠로 컴퓨터끼리 접속할 수 있게끔 도와줌
  - ssh askpass는 ssh패키지 다운할 때 같이 다운받는 거중 하나
- 공개키 만들기 :
  - `ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa`
    - ssh-keygen : 키 만들기
    - -t rsa : type은 rsa타입
    - -P '' : password 이전에 뭐가 있었다면 ''로 덮어씌우기 하기. 즉 password를 empty password로 설정
    - - f ~/.ssh/id_rsa : 경로는 ~/.ssh/id_rsa이다
  - 원래는 name node & data node는 별개의 컴퓨터로 분리해놈
  - 하지만 우리는 한 컴퓨터 안에 저장할 것임 but name node와 data node가 서로를 다른 컴퓨터라 생각하고 접근하게끔 할 것임.
  - 서로 다른 컴퓨터에 있다고 생각하면서 특정 key를 통해서 서로 접근할 수 있게끔 함!
- 생성한 키 저장하기 :
  - `cat명령어 >> 새로운 경로 ` : 원래 위치에서 만들어진 키를 새로운 경로에 저장
    - `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys` : 생성한 키를 authorized_keys에 저장



### Java Installation

- 구글에 일반적으로 java라고 검색했을 때 나오는 웹사이트는 개발자 버전이 아니라 for 클라이언트

- openjdk : java development key로 개발자버전 java임

  - openjdk에는 oracle java, amazon coretto 등 많음

  

- amazon coretto 11버전 설치 :

  - [Downloads for Amazon Corretto 11 - Amazon Corretto](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/downloads-list.html) 접속
  - `wget https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.tar.gz` -> 터미널에 입력
    - 만약 m1에 linux깔면 arm이기 때문에 linux/Aarch64다운받아야함!
  - wget : 파일을 요청해서 응답받아오기
  - `tar xvzf amazon-corretto-11-x64-linux-jdk.tar.gz` : 압축파일 해제
  - amazon까지만 타이핑하고 tab하면 나머지 자동 완성!



- symbolic link(바로가기) 만들기 :

  - `ln -s 파일 java` : java라고 부르는 symbolic link를 해당 파일로 만들기

  - `ln -s amazon-corretto-11.0.14.10.1-linux-x64/ java` 

    - amazon까지치고 자동완성을 위해 tab눌러도 완성이 끝까지 안됌. 자동완성 후보가 여러개라서

    - tabtab 빨리 치면 : 자동완성 후보 확인 가능

      - 자동완성 후보중 내가 원하는 후보 차이 위치까지만 치고 tab하면 마저 자동완성
  - 끝에 symbolic link의 이름 작성 잊지마!

     

- .bashrc에 환경변수 추가 :

  > 리눅스에서 가장 널리 사용되는 셀인 bash에서 작업할 때마다 수행되는 파일로서 별칭(alias)와 함수를 제어하는 시스템 설정과 관련된 파일이다. etc아래에 .bashrc는 모든 사용자, 개별 홈디렉토리 내에 있는 .bashrc는 개별 사용자에게만 영향을 미친다.

  - `sudo vim ~/.bashrc ` : bashrc 확인

  - 화살표로 쭉 내린다음에 하단의 내용 추가 후 :wq!로 저장하고 끄기:

    ```
    # java
    export JAVA_HOME=/home/jisu/java
    export PATH=$PATH:$JAVA_HOME/bin
    ```

    => /home/jisu/java (coretto symbolic link임)를 java_home으로 지정

  - `export 변수명=환경변수값` :  환경변수 설정 (재부팅 시 초기화 됌)

    - :white_check_mark: **변수명과 데이터값 사이에 있는 "=" 양옆에 띄어쓰기하면 "="를 데이터로 인식해서 오류생기니 띄어쓰기 하지말기!

  

- 추가한 환경변수 적용 :

  - 컴퓨터 재시작 OR
  - `source ~/.bashrc` 



- `java -version` : 버전 확인 (java는 java실행파일)
- `javac -version` : 버전확인 (javac는 compiler임. )



### Python Installation

- `python3 --version` : 우리가 지금 설치된 python이 3.8버전이지만 
- zlib이 3.8과 호환되지 않아서 3.7버전을 따로 다운받아 연결할 예정!



- python 3.7버전 다운로드 하기:

  - :white_check_mark: deadsnakes PPA : 여러 파이썬 버전을 ubuntu system에 설치할 때 사용

  ```
  # ppa 저장소 추가
  sudo add-apt-repository ppa:deadsnakes/ppa
  
  # python 3.7 버전 다운로드
  sudo apt update
  sudo apt install python3.7 -y
  ```

  - `sudo vim ~/.bahsrc` : 
    - 하단 내용 추가 후 저장하고 나오기 : 

  ```
  # python alias
  alias python=python3.7
  alias python3=python3.7
  ```

  => :white_check_mark: long command 사용하기 귀찮을 때 alias 지정해주면 간편하게 사용가능!

  - `source ~/.bashrc`
  - `python --version` => 이제 3.7 뜸
  - `python3 --version` => 이제 3.7 뜸

  

  

  - `pip --version` => 오류뜸

- pip 3.7버전 설치 하기:

  - `sudo apt install python3-pip -y`
  - `pip --version` : pip 정상적으로 확인은 가능하나 3.7버전이 아님

  - `sudo vim ~/.bashrc` 들어가서 하단 내용 작성후 저장하고 나가기
    - `alias pip="python3.7 -m pip"` 작성!
      - :white_check_mark: : -m flag는 module의 약자로 -m 뒤에 나온 module을 찾아서 실행
  - `source ~/.bashrc`
  - `pip --version` : 3.7버전!



### Hadoop

- 대용량 데이터를 분산 저장 및 처리할 수 있는  **Java** 기반의 오픈소스 프레임워크

- hadoop 모듈에는 4가지 :

  1. hadoop common : 나머지 3개 모듈 연결해주는 가장 기본적인 모듈


    2. hdfs : 대용량 데이터 데이터 분산 파일 시스템 
    
       - 구글 file system 기반으로 만든 대용량 분산 저장 / 처리 파일 시스템
       - master는 외부에서 저장하라는 데이터를 slaver에 분산시켜서 저장하고 가져오라고 외부에서 시키면 데이터를 slaver에서 가져오는 역할
       - block 구조 파일 시스템
    
    3. yarn : 리소스 관리 및 작업 예약/모니터링
    
    4. mapreduce : 데이터셋 병렬 처리

- node :
  - namenode(master) : 블록 및 메타데이터 관리, 클라이언트에서 온 요청 접수, 데이터 노드 모니터링
  - secondary name node : namenode가 문제 생겼을 때 datanode를 관리할 수 있는 예방 차원의 name node
  - datanode(slaver) : 데이터 저장 



#### Hadoop Download

- [Apache Hadoop](https://hadoop.apache.org/) 접속해서
- Download 클릭
- ![image](https://user-images.githubusercontent.com/96896873/158058364-e036d0cd-2d13-4bb4-8af4-47994813a10f.png)

- mirror site 클릭
- http 클릭
- hadoop-3.31 클릭
- hadoop-3.31.tar.gz 의 copy link

![image](https://user-images.githubusercontent.com/96896873/158058378-078d3d94-8a08-48ba-b25e-57ddb1e9bcdd.png)

- terminal 에서 :
  - `wget 복사한링크`
    - `wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz `
    - 굳이 다운안하고 링크 복사해서 하는 이유:
      - linux 다룰 때 gui대신 cli로 하니까, 즉 명령어로 이동하고 조작을 하니까, 다운도 wget으로 하는 것임!
- `tar xvzf hadoop-3.3.1.tar.gz` : 압축해제 (이것도 tab눌러서 자동완성하기!)
- `ln -s hadoop-3.31 hadoop` : symbolic link 만들기



- 여기까지 hadoop설치 완료

---

- 초기 configuration 설정은 hadoop_initial_config 참고

## 기타 :

- proxy server :
  - 기존 방식 : 유저가 직접 서버에 접근 해서 요청한 내용 가져오기
  - proxy server : 프록시 서버가 대신 서버에 요청한 후 내용은 클라이언트에 가져다 줌
- rm -rf :
  - r : 파일뿐만 아니라 directory까지도
  - f : force한다
- mirror kakao로 변경하기
  - `sudo vim /etc/apt/sources.list`
  - `:%s/us.archive.ubuntu.com/mirror.kakao.com` : %s는 일괄 변경하기
  - `:wq!`
  - `sudo apt-get update`

- linux에서 ctrl +v 눌렀을 때:
  - 타이핑이 아예 안됌 ㅜㅜ
  - 해결방법:
    - 강제로 끄고
    - ls로 bashrc.swp 존재 확인
    - rm -rf bashrc.swp
    - ls로 bashrc.swp 삭제 확인

- linux에서 마우스가 갖혀서 안나오면 : ctrl + alt누르기 

- [DistroWatch.com: Put the fun back into computing. Use Linux, BSD.](https://distrowatch.com/) -> 요즘 mx linux가 1위임

- docker는 software 개념 : container
  - 개발한 내용을 docker이미지로 만들어서 배포하는 경우가 많음

- linux 단축어 확인
  - preferences -> shortcuts
- 리눅스에서 .으로시작하는 파일이 숨김 파일임. 윈도우에서는 .시작해도 숨김파일 아님
- power off는 종료, logoff는 계정선택 창으로 이동