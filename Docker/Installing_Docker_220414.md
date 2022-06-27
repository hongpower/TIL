# Docker 설치

- 설치파일 다운로드 : https://www.docker.com/get-started/

- mac에서 docker 다운받기

  > 참고 : https://docs.docker.com/desktop/mac/install/

  - m1인 경우 : `softwareupdate --install-rosetta` terminal 창에 입력해서 로제타 설치
  - 이전에 다운받았던 설치파일 실행해서 app에 옮기기
  - `docker run -d -p 80:80 docker/getting-started`
    - `-d flag` : detached mode, 즉 background에서 container를 run하기
    - `-p flag` : port 지정. ` 호스트포트:컨테이너포트`순서대로 작성함. 이 케이스에서는 호스트의 80번 포트를 컨테이너의 80번 포트와 연결한다는 뜻
    - `docker/getting-started` : 사용할 도커 이미지. 이 케이스에서는 "getting-started"라는 이미지임



:question: docker compose란 :

- multi container applications를 run할 수 있게 해줌. 
- airflow를 실행하려면 webserver, scheduler, database의 최소 3개 이상이 필요한데 이 이유때문에 ariflow는 docker compose를 필요로함
- 이전까지는 docker compose를 따로 설치해야했지만 이제는 docker설치할 때부터 자동으로 설치가 돼서 별개로 docker compose 설치 필요 없음.