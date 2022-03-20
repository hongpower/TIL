## MongoDB

### MongoDB Installation

- https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/ 접속하면 순서 자세히 나와있음

##### 1. MongoDB 커뮤니티 버전 설치

- public key import하기 : `wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -`
- mongodb-org-5.0 list 이름을 가진 list file 생성하기 : `echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list`
- 로컬 패키지 db 재로드하기 : `sudo apt-get update` 또는 `sudo apt update`
  - 만약 여기서 upgrade할 거도 추가적으로 있다면 :
  - `sudo apt-get upgrade -y`
- mongoDB 패키지 설치하기 : `sudo apt-get install -y mongodb-org` 



##### 2. 실행하기

- systemd 방식과 system V Init방식 둘 중 하나 택1(최근 것이 전자)

  - systemd방식 : `sudo systemctl start mongod`

  - 또는 systemV Init 방식 :  `sudo service mongod status`

- `sudo service mongod status` : 확인



### 터미널에서 MongoDB 접속해서 작업하기

- `mongo`  : mongoDB 접속

- :white_check_mark: MongoDB에서 DB선택을 하지 않으면 default로 **test** db가 지정됨!

- 컬렉션 생성해서 document insert하기 :
  - `db.test.insertOne({"id" : "10", "name" : "mongodb"})`
- 잘 생성됐는지 확인하기 :
  - `db.test.find()`



- `exit` : mongoDB 종료



### Spark와 MongoDB 연결하기

- `cd $SPARK_HOME/conf`
- `vim spark-defaults.conf`
  - spark-mongo에서 읽기, 쓰기 mongo uri 지정


```python
spark.mongodb.input.uri		mongodb://localhost/test
spark.mongodb.output.uri	mongodb://localhost/test

spark.jars.packages			org.mongodb.spark:mongo-spark-connector_2.12:3.0.1 
```

- spark.jar.packages는 원래 작동이 되어야하지만 작동이 안돼서 일단 주석처리해서 추가함
- test라는 db는 우리가 생성하지도 지정하지도 않았지만 default로 지정되어서 test라는 db를 경로로 지정한 것임

![image](https://user-images.githubusercontent.com/96896873/159168528-983b3e04-2165-490b-b600-c322d4121cba.png)



- `pyspark --packages org.mongodb.spark:mongo-spark-connector_2.12:3.0.1`:
  - 위에 spark.jar.packages가 작동하지 않아서 pyspark실행할 때마다 옵션을 매번 주는 것임. 만약 input.uri와 output.uri도 작성하지 않았다면 실행할 때마다 conf 옵션으로 작성해주어야함
  - pyspark실행할건데 
  - 패키지 옵션은 spark와 연결하는 MongoDB Spark connector 패키지로 지정 which is mongodb.spark package
  - spark에서 mongodb하려면 매번 pyspark실행할 때마다 --package 작성 필수


:white_check_mark: MySQL과 다르게 jdbc가 아니라 mongoDB는 mongo사용



- spark에서 mongoDB내의 collection읽어오기 :

  - ```python
    test = spark.read.format("mongo").option("database", "test").option("collection", "test").load()
    
    test.show()
    ```

  

- spark에서 collection 저장 :

  - MySQL예시에서는 RDD를 df로 변경했지만 바로 df로 만들기 : 

    - `spark.createDataFrame([행1, 행2, ..], [컬럼명])`

  - ```python
    insert_df = spark.createDataFrame([("11", "mongo-spark")], ["id", "name"])
    insert_df.write.format("mongo").option("database", "test").option("collection", "test").mode("append").save()
    
    test.show()
    ```

- `exit()`
- `stop-all.sh`
- `sudo systemctl stop mongod`



## 기타

- aws 에서는 scp 명령어로 파일 업로드!

- `Fail to detect scala version` 오류 뜰 때는  home이아니라 /home/으로 설정

- focal이란 : ubuntu LTS마다 이름이 있는데, 20.04버전의 이름은 Focal Fossa임

- echo command
  - 화면 출력
  - echo hello -> hello출력함. echo가 별 의미가 없다 생각할 수 있지만
  - bash 스크립트와 같은 scripting할 때 echo가 활용성이 높음
- tee command
  - output을 복제해서 file에 보내주고 하나는 display해줌
- System V VS Systemd : 리눅스 시스템을 구분지음
  - System V가 더 오래됨.
  - Systemd가 더 빠른 부팅과, better dependency 등 더 괜찮은 성능들 가지고 있음



