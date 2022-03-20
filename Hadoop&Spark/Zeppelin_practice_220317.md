# Zeppelin

- zeppelin설치와 config마치고 시작!
- `start-all.sh`
- `zeppelin-daemon.sh start` => 결과창이 Zeppelin start ok 뜨면 잘 된 것임



##### 웹사이트 접속 :

- localhost:8082 접속

- 오른쪽 상단 anonymous -> interpreter

- search interpreters에 spark 검색 

![image](https://user-images.githubusercontent.com/96896873/159167135-923c483e-3f99-4f26-b442-d76a7d264ec8.png)

-> %spark : 스칼라, %ipyspark : 주피터, %r : r형태, %kotlin : kotlin으로 만든 spark도 동작하네? 이런거 확인 가능



##### 변경사항

- % 옆에 edit 클릭
  - spark.master는 yarn으로 변경
  - spark.submit.deploy/Mode는 clinet로 변경





- %라고 위에 적혀있던 곳 옆에 edit 클릭

- edit 눌러서 

  - spark.master 는 yarn으로 변경

  - spark.submit.deploy/Mode는 client로 변경

    - deployment mode는 내가 생성한 스파크 앱/job의 드라이버를 어디서 run할지 지정하는 것임
    - deploymode는 client외에도 cluster가 있음
    - cluster :  드라이버가 worker node중 하나에서 run됨. 클러스터 내부에서 driver가 run됨
  - client : 드라이버가 내 app을 submit하고 있는 local에서 spark-submit 명령을 통해 run됨. client mode에서는 드라이버만 local에서, 나머지 tasks들은 cluster worker node에서 진행됨 . 즉 custer 외부에서 driver 수행
  
- save 누르기
  

![image](https://user-images.githubusercontent.com/96896873/159167142-f5802752-d931-475d-9277-c15c084c6b47.png)



##### 노트북 만들기

- 상단에 Notebook눌러서
- Create new note

![image](https://user-images.githubusercontent.com/96896873/159167147-f649eb01-0c17-49f4-bdac-c9f74fb6e516.png)

- create 해주기



- 생성한 노트북 클릭해서 작성 :

```python
%pyspark

test = [1,2,3,4,5]
test_rdd = sc.parallelize(test)


test_rdd.collect()
```

- `zeppelin-daemon.sh stop`

- `stop-all.sh`

