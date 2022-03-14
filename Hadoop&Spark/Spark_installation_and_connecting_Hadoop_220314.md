# Spark

## Spark Installation

- hadoop 초기 설정 끝낸 상태에서 작업 이어서 시작! (start-all한 상태)
- https://spark.apache.org/spark 웹사이트 접속 
- download 클릭
- ![image-20220314121738534](C:\Users\jisuh\Desktop\데이터엔지니어링\Github\TIL\Hadoop\Images\image-20220314121738534.png)
- 3.1.3버전 선택(zlib노트북 사용 할거라서)
- pre-built with user-provided apache hadoop 선택 (우린 이미 hadoop설치 해놓았으니)
- 3번의 spark-3.1.3-bin-without-hadoop.tgz 클릭
- ![image-20220314121916546](C:\Users\jisuh\Desktop\데이터엔지니어링\Github\TIL\Hadoop\Images\image-20220314121916546.png)
- 여기서 http 링크 주소 복사
- `cd` : home으로 이동
- `wget https://dlcdn.apache.org/spark/spark-3.1.3/spark-3.1.3-bin-without-hadoop.tgz `  : 방금 link 주소 복사한거 wget + 붙여넣기



- 이전에 실행했던 demons 종료시키기 :

  - 방법1:

    - `stop-dfs.sh`
    - `stop-yarn.sh`

  - 방법2:

    - `stop-all.sh (dfs.sh + yarn.sh)` 

    

- `tar xvzf spark-3.1.3-bin-without-hadoop.tgz` : wget으로 다운받은 압축파일 압축 해제

- `ln -s spark-3.1.3-bin-without-hadoop spark` : spark라는 이름으로 symbolic link 만들기



## Connecting Hadoop & Spark

- 꼭!꼭! 연결할 때 **하둡 꺼놓고 진행**

  - 하둡을 꺼놔야하니까 위에 stop-all.sh를 했던 것임.

- `sudo vim ~/.bashrc`

- ```
  # spark
  export SPARK_HOME=/home/jisu/spark
  export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
  export SPARK_DIST_CLASSPATH=$(${HADOOP_HOME}/bin/hadoop classpath)
  
  ```

- `source ~/.bashrc`

- `cd $SPARK_HOME/conf`

- `ls`

- ![image-20220314130823951](C:\Users\jisuh\Desktop\데이터엔지니어링\Github\TIL\Hadoop\Images\image-20220314130823951.png)

  => spark는 xml대신 template주면서 기본으로, 우리가 알아서 고치라고 되어있음

- `cp workers.template workers`

- `cat workers` : 여기서 workers는 hadoop의 슬레이브 노드



- `cp spark-env.sh.template spark-env.sh`

- `vim spark-env.sh` : 스파크 기본 환경설정

- ```
  export JAVA_HOME=/home/jisu/java
  export HADOOP_CONF_DIR=/home/jisu/hadoop/etc/hadoop
  export YARN_CONF_DIR=/home/jisu/hadoop/etc/hadoop
  export SPARK_DIST_CLASSPATH=$(/home/jisu/hadoop/bin/hadoop classpath)
  
  export PYSPARK_PYTHON=/usr/bin/python3.7
  export PYSPARK_DRIVER_PYTHON=/usr/bin/python3.7
  ```

- `cp spark-defaults.conf.template spark-defaults.conf`

- `vim spark-defaults.conf`

  - conf파일은 key와 value를 스페이스로 구분

- ```
  spark.master 				yarn
  ```

  => 위에 tab 사이에 여러번 띄워놨음

  => spark.master : 연결할 cluster manager를 YARN cluster로 설정.

  - 위에서 설정한  hadoop_conf_dir OR yarn_conf_dir에서 cluster location이 찾아진대



spark 설치 완료!

---

- `cd`

- `start-dfs.sh`

- `start-yarn.sh`

- `pyspark`

---

## 