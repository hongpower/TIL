# /Spark

- 시작 :

`start-all.sh`

`jps` or local site 방문

`pyspark`

---

## File

- CSV 파일 불러오기
  1. `csv_file01 = spark.read.format("csv").option("header", "true").load("/home/jisu/data/flights/csv/2010-summary.csv")` 
  2. `csv_file02 = spark.read.option("header", "true").csv("/home/jisu/data/flights/csv/2010-summary.csv")`



- CSV로 파일 저장

  1. `csv_file02.write.format("csv").mode("overwrite").save("/tmp/csv") ` : csv_file02를 csv파일로 저장한다. (저장되는 이름은 컴퓨터가 자동으로 생성)

  

:white_check_mark: dataframewrite의 모드들 : append, mode, error/errorifexists, ignore

| append                          | overwrite                  | error/errorifexists  | ignore                   |
| ------------------------------- | -------------------------- | -------------------- | ------------------------ |
| 파일이 존재한다면 추가해서 작성 | 파일이 존재한다면 덮어쓰기 | 파일이 존재하면 에러 | 파일이 존재해도 무시하기 |



- csv 파일 저장된거 확인 :

```python
# pyspark 나가기
exit()

# 확인
hdfs dfs -ls /tmp/csv/

# 파일 내용 확인
hdfs dfs -cat /tmp/csv/*.csv
```



- json파일 불러오기

1. `json_file01 = spark.read.format("json").load("/home/jisu/data/flights/json/2010-summary.json")`

2. `json_file02 = spark.read.json("/home/jisu/data/flights/json/2010-summary.json") `



- json 파일로 저장하기

1. `json_file01.write.format("json").mode("overwrite").save("/tmp/json")`





:white_check_mark: 파일이 hdfs에 저장되는 이유 :

- core-site.xml 내부에 configuration에 fs.defaultFS 가 hdfs 경로로 되어있음 (hdfs://localhost:9000)
- 클러스터 매니저 :
  - yarn으로 하면 hdfs에 저장됨 (spark에서 저장/불러오기의 주체가 yarn)
  - standalone이면 linux local 내부에 저장
  - messo




---



## 집계 확인하기 :

- 데이터 준비 :

```python
retails = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/home/jisu/data/retails/2010-12-01.csv")

from pyspark.sql.functions import to_date, col

## date_df는 2010-10-23 이런식으로만 나옴 (date datatype으로 바뀐거니까)
date_df = retails.withColumn("date", to_date(col("InvoiceDate"), "yyyy-MM-dd HH:mm:ss"))

notnull_df = date_df.drop()
notnull_df.createOrReplaceTempView("notnullDf")

spark.sql("""
SELECT CustomerId, StockCode, sum(Quantity)
FROM notnullDf
GROUP BY CustomerId, StockCode
ORDER BY CustomerId DESC, StockCode DESC
""").show()
```



- 집계 :

```python
## rollup() : 집계화면 보여줌
from pyspark.sql.functions import sum

rollup_df = notnull_df.rollup("Date", "Country").agg(sum("Quantity")).selectExpr("Date", "Country", "`SUM(Quantity)` as total_quantity").orderBy("Date")

# A에 대한 집계와, 전체 집계 출력 : 
rollup_df.where("Country IS NULL").show()
```

![image](https://user-images.githubusercontent.com/96896873/158819836-8ddd749f-faf8-4a3d-b60e-1306face22ca.png)

```python
## cube() : a, b, a*b, 모든집계

notnull_df.cube("Date", "Country").agg(sum(col("Quantity"))).select("Date", "Country", "SUM(Quantity)").show()
```

![image](https://user-images.githubusercontent.com/96896873/158819943-748a8350-58ed-46e4-9e4c-7d668fad2264.png)



## Join

- 데이터 준비 :

```python
## join

# dataframe 생성하기 : 
# person df :
person = spark.createDataFrame(
[
	(1, "shin dongyep", 2, [1]),
	(2, "seo janghun", 3, [2]),
	(3, "you jaeseok", 1, [1,2]),
	(4, "kang hodong", 0, [0])
]
).toDF("id", "name", "program", "job")

# program df :
program = spark.createDataFrame(
[
    (1, "MBC", "놀면 뭐하니"),
    (2, "KBS", "불후의 명곡"),
    (3, "SBS", "미운 우리새끼"),
    (4, "JTBC", "뭉쳐야 찬다")   
]
).toDF("id", "broadcaster", "program")

# job df : 
job = spark.createDataFrame(
[
    (1, "main mc"),
    (2, "member")
]
).toDF("id", "job")

## 테이블로도 만들기 :
person.createOrReplaceTempView("person")

program.createOrReplaceTempView("program")

job.createOrReplaceTempView("job")

```

![image](https://user-images.githubusercontent.com/96896873/158819973-a5d37f13-7e8c-4fe0-ac33-d1096cfea894.png)



- join :

```python
## 메소드 :
# 테이블1.join(테이블2, 조건, join방식)

person.join(program, person['program'] == program['id']).show()

# join의 조건을 변수에 담아서 작성다시하기:
conditions = person.program == program.id
person.join(program, conditions).show()

# join 종류 명시하기 
person.join(program, conditions, "inner").show()
person.join(program, conditions, "outer").show()
person.join(program, conditions, "left_outer").show()
person.join(program, conditions, "right_outer").show()
# left semi : inner join한 결과를 왼쪽에 있는 컬럼만 나오는거
person.join(program, conditions, "left_semi").show()
# right semi는 없음

# left_anti : inner join에 포함되지 않는 애들
person.join(program, conditions, "left_anti").show()

# cross join :
# person.join(program, conditions, "cross").show() : X. 완벽하게 inner join한 상태기 때문에 cross가 안먹음
# 대신 how=cross로 작성
# 1) 불가능 :
person.join(program, conditions, "cross").show()
# 2) 가능 :
person.crossJoin(program).show()
# 3) 가능 :
person.join(program, conditions, how='cross').show()

## 쿼리 :
# inner :
spark.sql("SELECT * FROM person JOIN program ON (program.id = person.program)").show()

# outer :
spark.sql("SELECT * FROM person FULL OUTER JOIN program ON (program.id = person.program)").show()

```



```python
## 리스트에 담긴 데이터와 데이터를 합치기, 즉 데이터타입다를 때 join하기
from pyspark.sql.functions import expr

person.join(job, expr("array_contains(job, id)")).show()
# => 오류 (ambiguous하대)

# 중복 컬럼명은 바꾸고 다시 join 시작
person.withColumnRenamed("id", "num").withColumnRenamed("job", "role").join(job, expr("array_contains(role, id)")).show()
```





## Subquery

- 메소드에는 subquery없음!

```python
# subquery를 사용하여 SBS에서 프로그램을 하고 있는 사람 출력하자
spark.sql("""
SELECT *
from person
where program =
(SELECT id
FROM program
WHERE broadcaster = 'SBS')
""").show()
```





## 기타 :

- hdfs dfs -put 보낼(로컬) 받을(하둡)  

- 우리는 hdfs에 저장하는 방법을 사용했지만 spark는 amazon S3, Azure, GCP 등 다양한 파일 시스템에 저장도 지원함. 
- 우리는 .csv, .json파일로만 저장하는 법을 공부했지만 그 외에 parquet(spark 컬럼 기반 데이터 저장방식), orc(hadoop 컬럼기반 데이터 저장방식), text, db(db마다 파일 형태가 다르지만 여튼 db지원) 등 다양하게 지원

