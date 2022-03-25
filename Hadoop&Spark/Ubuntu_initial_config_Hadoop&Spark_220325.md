
# Quick setup for Hadoop & Spark

- UTM 가상서버가 다 만들어졌다는 전제하에 시작 (필요한 기본 파일들까지 다)
- hadoop, spark, spark에 db연결(mysql 및 mongod)만 작성(zeppelin 및 sparkstreaming은 제외)
- 본인 관리자 계정 이름은 jisu라서 드문 드문 jisu라고 나오는 부분은 본인 관리자 유저명으로 대체 해야함ㄴ

~~~
sudo apt update
sudo apt upgrade -y
sudo apt install vim
~~~

~~~
sudo apt install openssh-server ssh-askpass -y
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
~~~

- open jdk 다운로드하기

```
wget https://corretto.aws/downloads/latest/amazon-corretto-11-aarch64-linux-jdk.tar.gz
tar xvzf amazon-corretto-11-aarch64-linux-jdk.tar.gz
ln -s amazon-corretto-11.0.14.10.1-linux-aarch64/ java
```

~~~
sudo vim ~/.bashrc

# java
export JAVA_HOME=/home/jisu/java
export PATH=$PATH:$JAVA_HOME/bin

# python alias
alias python=python3.7
alias python3=python3.7
alias pip="python3.7 -m pip"

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

# spark

export SPARK_HOME=/home/jisu/spark
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
export SPARK_DIST_CLASSPATH=$(${HADOOP_HOME}/bin/hadoop classpath)

```
- 위 내용들은 미리 다 작성해놓고 설치할 때마다 주석풀어서 설치하는게 편함!
	- #java만 주석 풀어놓고 나머지는 다 주석 처리해서 작성해놓자!
```

sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.7 -y
~~~

~~~
sudo vim ~/.bashrc
python 관련 alias 주석풀기
source ~/.bashrc
~~~

- pip 다운받기


~~~
sudo apt install python3-pip y
~~~



# Hadoop
~~~
 wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.1/hadoop-3.3.1.tar.gz

tar xvzf hadoop-3.3.1.tar.gz
ln -s hadoop-3.3.1 hadoop
~~~

~~~
sudo vim ~/.bashrc

주석 hadoop user까지 다 풀기

soure ~/.bashrc
~~~


~~~
cd $HADOOP_CONF_DIR
vim hadoop-env.sh
#54번째 줄부터 : 
export JAVA_HOME=/home/jisu/java 
export HADOOP_HOME=/home/jisu/hadoop 
# 이거는 주석만 풀면 됌 :
export HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop

#198번 줄부터 :
# export HADOOP_PID_DIR=/tmp 이거 변경 to
export HADDOP_PID_DIR=$HADOOP_HOME/pids
~~~

~~~
vim core-site.xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>

vim hdfs-site.xml
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

vim mapred-site.xml
<configuration>
        <property>
                <name>mapreduce.framework.name</name>
                <value>yarn</value>
        </property>
</configuration>
~~~


~~~
cd
hdfs namenode -format
hdfs datanode -format

start-all.sh
jps
~~~



# Spark
~~~
wget https://dlcdn.apache.org/spark/spark-3.1.3/spark-3.1.3-bin-without-hadoop.tgz

stop-all.sh
tar xvzf spark-3.1.3-bin-without-hadoop.tgz
ln -s spark-3.1.3-bin-without-hadoop spark

~~~

~~~
sudo vim ~/.bashrc

spark 관련 주석풀기

source ~/.bashrc
~~~


~~~
cd $	SPARK_HOME/conf

cp workers.template workers
cp spark-env.sh.template spark-env.sh

vim spark-env.sh
export JAVA_HOME=/home/jisu/java
export HADOOP_CONF_DIR=/home/jisu/hadoop/etc/hadoop
export YARN_CONF_DIR=/home/jisu/hadoop/etc/hadoop
export SPARK_DIST_CLASSPATH=$(/home/jisu/hadoop/bin/hadoop classpath)

export PYSPARK_PYTHON=/usr/bin/python3.7
export PYSPARK_DRIVER_PYTHON=/usr/bin/python3.7


cp spark-defaults.conf.template spark-defaults.conf
vim spark-defaults.conf
spark.master				 yarn
~~~

~~~
cd
start-all.sh
pyspark
~~~

# MySQL
~~~
sudo apt install mysql-server -y
sudo service mysql start
sudo mysql_secure_installation

n
password입력
y
n
y
y
~~~

~~~
cd /etc/mysql/mysql/conf.d
sudo vim mysqld.cnf

# 약 30번째 줄 변경 :
bind-address : 0.0.0.0
mysqlx-bind-address : 0.0.0.0 :
~~~

~~~
cd
sudo mysql -u root -p

show databases;
use mysql;

alter user 'root'@'localhost' identified with mysql_native_password by '1234';

CREATE USER 'root'@'%' IDENTIFIED BY '1234';

GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;

RANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION;

GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;

flush privileges;
~~~

해보고 싶다면 실습 예제(왠만하면 확인 용도로 하자) :

~~~
CREATE TABLE test(id int, name VARCHAR(30));

INSERT INTO test VALUES(1, "hadoop");
INSERT INTO test VALUES(2, "spark");

select * from test;

exit;
~~~

~~~
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java_8.0.28-1ubuntu20.04_all.deb

sudo dpkg -i mysql-connector-java_8.0.28-1ubuntu20.04_all.deb
~~~

~~~
cd $SPARK_HOME/conf
vim spark-defaults.conf

# spark.master 밑에 mysql-connector 경로 입력하기 :
spark.jars /usr/share/java/mysql-connector-java-8.0.28.jar

~~~

~~~
cd
start-all.sh
pyspark

# 변수 만들기 :
user="root"
password="1234"
url="jdbc:mysql://localhost:3306/mysql" 
driver="com.mysql.cj.jdbc.Driver" 
dbtable="test"

test_df = spark.read.format("jdbc").option("user", user).option("password", password).option("url", url).option("driver", driver).option("dbtable", dbtable).load()

# ID와 name을 컬럼으로 하는 리스트 -> rdd -> df로 만들기 :
test_insert = [(3, "mysql"), (4, "zeppelin")]
insert_df = sc.parallelize(test_insert).toDF(["id", "name"]) 

# mode는 append로!
insert_df.write.jdbc(url, dbtable, "append", properties={"driver" : driver, "user" : user, "password" : password}) 

# 확인
test_df.show()
~~~


# MongoDB 

~~~
wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -

echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/5.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-5.0.list

sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get install -y mongodb-org

sudo systemctl start mongod
sudo systemctl status mongod

#!!!!!여기서 중요한거! mac은 end뜨는데 이럴 때 아무것도 누르지말고 q 작성하면 나올 수 있음 ㅎㅎ
~~~

~~~
mongo
db.test.insertOne({"id" : "10", "name" : "mongod"})
db.test.find()
exit()
~~~


~~~
cd $SPARK_HOME/conf
vim spark-defaults.conf

spark.mongodb.input.uri		mongodb://localhost/test
spark.mongodb.output.uri	mongodb://localhost/test

spark.jars.packages			org.mongodb.spark:mongo-spark-connector_2.12:3.0.1 
~~~

~~~
pyspark

test = spark.read.format("mongo").option("database", "test").option("collection", "test").load()

test.show()

insert_df = spark.createDataFrame([("11", "mongo-spark")], ["id", "name"])
insert_df.write.format("mongo").option("database", "test").option("collection", "test").mode("append").save()

test.show()
~~~

~~~
exit()
sudo systemctl stop mongod
stop-all.sh
~~~
