# Spark Streaming

- https://spark.apache.org/docs/latest/streaming-programming-guide.html
- 실시간 처리 하는 Kfaka와 같은 소스로부터 가져온 데이터 -> sparkstreaming에서 여러 function을 통해서 처리 -> 프로세싱된 데이터를 HDFS, DB, 대쉬보드 등에 푸쉬
- spark streaming 내부에서 처리 방법 :
  - live input data stream을 받음 -> spark streaming에서 data를 배치로 나눔 -> spark engine에서 프로세싱(다양한 function을 이용해서 전처리) -> 프로세싱된 배치 데이터를 output으로 생산



- `vim streaming_test.py `: streaming_test.py home에서 만들기!

- ```python
  from pyspark import SparkContext
  from pyspark.streaming import StreamingContext
  
  # Create a local StreamingContext with two working thread and batch interval of 1 second
  sc = SparkContext("local[2]", "NetworkWordCount")
  
  # StreamingContext(sparkcontext객체 , batch interval(sec))
  ssc = StreamingContext(sc, 1)
  
  # Create a DStream that will connect to hostname:port, like localhost:9999
  # StreamingContext객체.socketTextStream(호스트명, port) : 데이터 서버로부터 받은 데이터 스트림
  lines = ssc.socketTextStream("localhost", 9999)
  
  # Split each line into words
  words = lines.flatMap(lambda line: line.split(" "))
  
  # Count each word in each batch
  pairs = words.map(lambda word: (word, 1))
  wordCounts = pairs.reduceByKey(lambda x, y: x + y)
  
  # Print the first ten elements of each RDD generated in this DStream to the console
  wordCounts.pprint()
  
  ssc.start()             # Start the computation
  ssc.awaitTermination()  # Wait for the computation to terminate
  ```

- streamingContext : streaming을 하기 위한 가장 첫번째 main entry point 

- DStream(위의 예제에서는 lines변수) 내부의 each record는 a line of text이기 때문에 space기준으로 데이터를 split해서 단어로 변경하는 것임
- pairs는 words('word', 'sentence', 'covid' ..)의 요소를 하나씩 가져와서 ('word', 1) 이렇게 바꾼 것임



- 위 코드 작성이 끝나면 NetCat을 실행해야함 ! 
- NetCat :
  - 유닉스 베이스의 system에서 찾을 수 있는 small utility
  - port 스캐닝과 port listening을 허용해주는 back-end tool임

- `nc -lk 9999` : 
- 새로운 창열어서 : `spark-submit streaming_test.py localhost 9999`  하면 실시간 데이터 확인 가능!



![image](https://user-images.githubusercontent.com/96896873/158731898-30d533d7-700e-45ef-baab-392c20ca3537.png)