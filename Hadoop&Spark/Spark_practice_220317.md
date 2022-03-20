# Spark

## Zoom chatting 데이터로 실습하기

>  Local 데이터를 hadoop에 올리고 spark로 프로세싱하는 작업을 거칠 예정

- all.csv 폴더 열어서 home에 추가하기

  - all.csv 파일은 220302~220310까지의 zoom 채팅 데이터임 

  - ```
    chat_date,chat_time,chat_from,chat_to,chat_content
    220302,08:52:43,김선달,모두에게,내용
    220302,08:52:43,홍지수,모두에게,내용
    220302,08:52:43,박병규,모두에게,내용
    220302,08:52:43,김한나,모두에게,내용
    .
    .
    220303,10:49:37,유재욱,모두에게,내용
    .
    .
    .
    ```

    

- home에 있는 파일 하둡에 올리기 :

```python
# 현재 내 위치에 있는 all.csv를 hdfs내의 home/jisu 아래에 올리기
hdfs dfs -put ./all.csv /home/jisu/all.csv

# 확인하기
hdfs dfs -ls /home/jisu
```

---

- `pyspark`



#### 데이터 준비 :

```python
# df로 불러오기 
# 1)
multi = spark.read.format("csv").option("header", "true").load("/home/jisu/all.csv")
# 2)
multi = spark.read.option("header", "true").csv("/home/jisu/all.csv")

multi.count() #: 몇 개 있나 확인

# 테이블 생성 :
multi.createOrReplaceTempView("multi")

multi.show(multi.count())
```

- option먼저, 그 다음에 csv작성해야 먹힘!



#### 프로세싱 :

```python
# 채팅을 많이 보낸 사람 순서대로 정렬
from pyspark.sql.functions import col

multi.groupBy("chat_from").count().orderBy(col("count").desc()).show()

# 이동헌 강사님께 개인적으로 많이 보낸 사람 순서대로 정렬
multi.where(col("chat_to") == "이동헌강사").groupBy("chat_from", "chat_to").count().orderBy(col("count").desc()).show()
```



- date관련 프로세싱 :

```python
from pyspark.sql.functions import substring

# 시간별 채팅 횟수
multi.groupBy(substring("chat_time", 1, 2)).count().orderBy(substring("chat_time", 1, 2)).show()

# 날짜별 채팅 횟수
multi.groupBy(col("chat_date")).count().orderBy(col("chat_date")).show()

# 날짜와 시간별 채팅 횟수
multi.groupBy(col("chat_date"), substring("chat_time", 1, 2)).count().orderBy(col("chat_date"), substring("chat_time", 1, 2)).show()

# 시간별 채팅 가장 많이 한 사람
hour_chat = multi.groupBy(substring("chat_time", 1, 2).alias("hour"), "chat_from").count().orderBy(substring("chat_time", 1, 2), col("count").desc())

hour_chat.show(hour_chat.count())

# 시간과 채팅 보낸 사람의 rollup
hour_chat.rollup("hour","chat_from").agg(sum("count")).orderBy("hour", col("sum(count)").desc()).show(hour_chat.count())
```





#### 저장하기 :

- spark에서 저장하기 :

```python
# 파티션 여러개로 저장 :
hour_chat.write.format("csv").mode("overwrite").save("hour_chat")
```

- localhost:9870 접속
  - user/jisu/hour_chat 이라는 파일 아래에 csv파일이 한줄당 하나씩 만들어져있음
  - rdd가 파티션 여러개 만들어진게 싫다면 ?


```python
# rdd의 파티션을 하나로 만들어서 저장하기 :
# coalesce(1) 활용!
hour_chat.coalesce(1).write.format("csv").mode("overwrite").save("/home/jisu/hour_chat")

# 두 개로 만들어서 저장 :
hour_chat.coalesce(2).write.format("csv").mode("overwrite").save("/home/jisu/hour_chat")

# 지정한 컬럼의 파티션별로 나누기 :
# partitonBy
# chat_from별로 폴더 만들어지고 내부에 시간 별로 count 데이터가 있는 구조로 변환
hour_chat.write.option("header","true").partitionBy("chat_from").mode("overwrite").csv("hour_chat")
```

- :white_check_mark: 저장되는 csv파일이름은 따로 지정 불가능 ㅠㅠ

- `exit()`



- hadoop에서 저장하기 :

- `vim hour_chat.py` 

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, substring

# 스파크 session을 만들건데, 클러스터 매니저는 yarn이고 이름은 hour_chat으로 하고 이 파일이 스파크로 동작하게 되게끔 스파크 객체 생성 :
spark = SparkSession.builder.master("yarn").appName("hour_chat").getOrCreate()

multi = spark.read.format("csv").option("header", "true").load("/home/jisu/all.csv")
hour_chat = multi.groupBy(substring("chat_time", 1, 2).alias("hour"), "chat_from").count().orderBy(substring("chat_time", 1, 2), col("count").desc())

hour_chat.coalesce(1).write.format("csv").mode("overwrite").save("/hour_chat")
```

- 위 내용들은 spark_home/bin안에 있는 command sets .sh파일들이 있대

- `pip install pyspark` 
- `python hour_chat.py` : python 명령어로, 지정한 python 파일을 실행하겠다는 뜻. 
  - bash.rc에서 alias를 python=python3를 별칭줬었잖아?
  - `spark submit` 를 통해 실행하면 스파크를 통해 파이썬 파일을 실행하는 것임

- `hdfs dfs -cat /hour_chat/*.csv` : 하둡에 csv파일 저장 완료된거 확인 가능
- ![image](https://user-images.githubusercontent.com/96896873/158832069-f1c18a8c-d0a1-47b7-8303-a464a60edd65.png)
- localhost:9870에서도 확인 가능!

- `stop-all.sh`



---

### shell script 만들기

> .sh파일은 shell script임. 자동으로 hadoop을 실행하고 spark를 실행하는 script생성할 것임

- `vim multi.sh` 

```bash
start-all.sh
sleep 5
pyspark
```

- :white_check_mark: 여기 .sh파일 내에 있는 코맨드는 python이 아니라 bash인 것임.

- `sudo chmod 755 multi.sh`

- `ls -l | grep multi.sh`

- `./multi.sh` : 파이스파크 실행이 됨. 문제는 sleep이 안먹혀서 그냥 넘어감 ㅜㅜ


