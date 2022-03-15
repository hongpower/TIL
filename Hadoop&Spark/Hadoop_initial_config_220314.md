# Hadoop setting

- ls 검색해서 hadoop softlink 정상적으로 만들어졌는지 확인

- 우리는 현재 pseudo-distributed mode
  - :white_check_mark: pseudo-distributed mode : single-node cluster라고도 불리는데 namenode와 datanode가 동일한 가상머신 위에 있을 때. single machine내에 있지만 seperate java process를 이용할 것임.
  - fully distributed mode : 2개 이상의 머신으로 mode가 분리되어있는 경우

##### .bahsrc 수정

- `sudo vim ~/.bashrc`

```
# hadoop
export HADOOP_HOME=/home/jisu/hadoop
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

# hadoop user
export HDFS_NAMENODE_USER=jisu
export HDFS_DATANODE_USER=jisu
export HDFS_SECONDARYNAMENODE_USER=jisu
export YARN_RESOURCEMANAGER_USER=jisu
export YARN_NODEMANAGER_USER=jisu
```

=> PATH : 지금까지 있는 PATH경로에다가 :$HADOOP_HOME/bin 추가하고, HADOOP_HOME/sbin 추가한 것임

=> hadoop user : 사용가능 유저 설정

- 저장하고나오기! :wq!

`source ~/.bashrc`



#### Hadoop configuration

- hadoop_conf_dir, 즉 /home/local명/hadoop/etc/hadoop에서 모든 hadoop configuration file를 찾을 수 있음
- 이 경로에 있는 몇몇 파일들은 초기 설정이 필요

- `cd $HADOOP_CONF_DIR`

- `ls`

![image](https://user-images.githubusercontent.com/96896873/158278288-2b792efd-eeee-4ccb-82fb-53a796fe21bd.png)



##### 1) hadoop-env.sh 수정

- `vim hadoop-env.sh` : java 환경변수 재설정하기

```python
#54번째 줄부터 : 
export JAVA_HOME=/home/jisu/java # 주석풀고 home위치 잡아주기
export HADOOP_HOME=/home/jisu/hadoop # 주석풀고 hadoop_home 잡아주기
export hadoop_conf_dir# 얘는 주석만 풀기 이미 경로etc/hadoop으로 잘 잡혀있음

#198번 줄부터 :
export HADOOP_PID_DIR=/tmp  # 주석풀고 hadoop_pir_dir 위치 새로 잡아주기
# PID파일 어디다 저장할건지인데, 임시저장소에 저장하면 하둡 저장했던 파일들이 다 날라갈테니 바꿀것임 => $HADOOP_HOME/pids (이파일 만들어서 여기로지정해줘요)
```

- `JAVA_HOME` : OpenJDK 설치가 된 경로 (softlink가 있는 곳)로 지정
  - 만약 java가 어딨는지 확인이 필요하다면 :
    - `which javac` : java binary directory경로 보여줌
      - which javac로 나온 경로중 bin/javac이전까지 자르면 그게 openJDK가 설치된 경로임
      - ![image](https://user-images.githubusercontent.com/96896873/158278306-daf29d3a-74d8-411a-8bb3-a4c7cf66ddc9.png)



##### 2) core-site.xml 수정

- `vim core-site.xml` : HDFS와 Hadoop core property 수정 가능
  - HDFS url : 요청할 기본 경로 설정 

```html
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

![image](https://user-images.githubusercontent.com/96896873/158278318-bb7cac51-6b0b-4b53-a390-503945ca7e8e.png)

- 참고 : https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/core-default.xml
  - 위 사이트에서 fs.defaultFS 의 value가 file:///형태로 작성하라 되어있음

![image](https://user-images.githubusercontent.com/96896873/158278332-707a9f60-aa25-4919-8b68-d554e7f41444.png)



##### 3) hdfs-site.xml 수정

- `vim hdfs-site.xml` : nameNode 및 DataNode 저장 directory 지정 가능

```html
<configuration>
        <property>
                <name>dfs.replication</name>
                <value>1</value>
        </property>
        <property>
                <name>dfs.namenode.name.dir</name>
                <value>/home/jisu/hadoop/namenode_dir</value>
        </property>
        <property>
                <name>dfs.datanode.data.dir</name>
                <value>/home/jisu/hadoop/datanode_dir</value>
        </property>
</configuration>
```

- `dfs.replication` : 우리는 single mode(datanode와 namenode를 한 컴터내에 하는거)쓰고 있음. 싱글 모드는 요청된 파일을 복사해서 저장할 필요가 없음. 그래서 우리는 하나만 있으면 되니까 dfs.replication의 값을 1로 넣은 것임.

  - 만약 분산 노드를 쓴다면:
    - 예) 우리가 값 1,2,3을 분산 저장할건데 1,2,3값을 각각 3개씩 복사되어서 저장할거라면(data node에) dfs.replication의 값은 3




##### 4) mapred-site.xml 수정

- `vim mapred-site.xml` : 맵리듀스 관련 설정하는 곳으로 framework 지정해줄 것임
  - local, classic, yarn 중에서 선택
    - yarn : Mapreduce version 2
    - classic : Mapreduce version 1
    - local : when no there is no cluster for exectuion

```html
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
</configuration>
```

---

hadoop configuration 초기 설정 끝!

---

#### MapReduce job을 local에서 run설정

- `hdfs namenode -format` : hdfs 내에 있는 namenode의 모든 파일을 삭제 (fsimage와 기타 등등 파일 basic information이 기본으로 적혀있는데 이걸 삭제) => hdfs-site.xml에 dfs.namenode.name.dir에 지정되어있는 경로에 있는 내용을 삭제하게 되는 것임
- `hdfs datanode -format` : hdfs 내에 있는 datanode의 모든 파일 삭제



#### Startup scripts

- 방법 1:

  - `start-all.sh` : namenode, datanode, nodemanager, secondarynamenode, yarn 데몬 실행 (hadoop_home/sbin에 존재)

- 방법 2:

  - `start-dfs.sh` : DFS daemnods(namenode와 datanode daemon) 시작

  - `start-yarn.sh` : resource manager와 node manager 시작



![image](https://user-images.githubusercontent.com/96896873/158278347-86a16bdf-577a-4831-a7f2-d2540db517e6.png)

`jps` : 내 머신 위의 Java process 다 list함.to check out all the hadoop deamon like DATnode, node manager that are currently running on machine.

- `hdfs dfsadmin -report` : filesystem info와 statistics를 report 해줌.
- ![image](https://user-images.githubusercontent.com/96896873/158278361-13de8a54-3835-48e3-a15f-05a0a3573725.png)
- firefox에 `localhost:9870` url에 입력 => HDFS 관련 (start-dfs.sh 이후 가능)
- firefox에 `localhost:8088` url에 입력 => Yarn 관련 (start-yarn.sh 이후 가능)