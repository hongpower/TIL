## MySQL

### MySQL 설치 

- `sudo apt install mysql-server -y`

- `sudo service mysql start`

- `sudo mysql_secure_installation` : 
  - 유효한 pw 설정 : n
  - 익명 유저 삭제 : y
  - 외부 접속 차단 : n
  - test데이터 삭제 : y
  - 권한 테이블 로딩 : y

![image](https://user-images.githubusercontent.com/96896873/159167174-cbdac5d0-1822-4ff2-b147-d28a94c57be8.png)

![image](https://user-images.githubusercontent.com/96896873/159167179-a2059179-858b-4bbb-bdca-463e2e2567a8.png)



### MySQL config

- `cd /etc/mysql/mysql.conf.d`

- `ls`
  -  mysql.cnf와 mysqld.cnf가 있음 (d는 데몬)



##### mysqld.cnf 설정

- `sudo vim mysqld.cnf`

```python
# 약 30번째 줄 변경 :
bind-address : 0.0.0.0
mysqlx-bind-address : 0.0.0.0 
```

- ip가 0.0.0.0이라서 누구나 접속 가능

  - but 실무에서는 db보안을 위해 특정 유저만 접속할 수 있는 ip사용해야함(ip관련 찾아보기)

  



### MySQL 실행

> MySQL은 MySQL을 설치하자마자 root user를 자동으로 생성한다. 이 유저는 MySQL 서버에 모든 접근 권한이 있기 때문에 관리와 연관되지 않은 일에서는 사용이 지양된다. 

- `cd`
- `sudo mysql -u root -p` : mysql 시작! (`sudo mysql`로만도 초기에는 시작 가능)
  - :white_check_mark: ubuntu에서 MySQL을 실행할 때는 비밀번호 대신 `auth_socket` 이라는 plugin을 통해 root유저를 인증해야함. 이 말은 MySQL을 실행하는 OS상의 유저이름과 MySQL의 유저이름과 매치가 되어야한다는 것을 뜻함. 즉, MySQL의 root로 접근하려면 sudo로 mySQL을 실행해야한다는 것을 뜻함.
    -  하지만 만약 내가 비밀번호를 설정한다면, 그 때부터는 `mysql -u root -p`로 실행 가능




- root 유저로 새로운 유저 생성 및 권한 부여 : 
  - :white_check_mark: identity는 username과 clienthost 2가지로 정의됨. 즉 이름이 동일하더라도 clienthost가 다르다면 다른 identity임. (MySQL account names consist of a user name and a host name, which enables creation of distinct accounts for users with the same user name who connect from different hosts)

```sql
show databases;

use mysql;

// 비밀번호 변경 : 
// local에서만 접속 가능한 root의 비밀번호를 1234로 설정해줘
// alter user '유저명'@'' identified with mysql_native_password by '비밀번호' 또는 identified by '비밀번호'도 가능 (두개 다 사용하면 이중 비밀번호)
alter user 'root'@'localhost' identified with mysql_native_password by '1234'; 

// 동일한 이름인 root지만 다른 호스트명을 가진 user 생성 : 
// CREATE USER '유저명'@'host명' identified by '비밀번호'
CREATE USER 'root'@'%' IDENTIFIED BY '1234'; 

// 사용자 권한 적용 :
// GRANT ALL PRIVILEGES ON DB이름 TO '유저명'@'localhost' : 특정 db
// GRANT ALL PRIVILEGES ON *.* TO '유저명'@'localhost' : 모든 db
// local에서 접속하는 root라는 user에게 권한 부여 :
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION; 
// 다른 서버에서 접속하는 root라는 user에게 권한 부여 :
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION; 

// 저장/실시간 반영 :
flush privileges;
```

- 위 예제에서는 root라는 user가 이미 있는데, 외부에서도 이 user를 접근 가능한 새로운 root생성하려고 작성. 

- 유저 생성 :
  - 내 ubuntu server내에서만 이 유저를 사용할 예정이라면 `user명@localhost`라고 작성. 
  - 원격으로 연결하고 있다면 MySQL을 호스팅하고 있는 원격 시스템의 IP주소를 작성 :  `user명@ip주소`
  - 모든 가상 머신에서 접근 가능한 유저 생성 : `user명@'%'`

- `*.*` :  모든 DB에 접속 가능케함
- flush 꼭 해줘야함!

  - 하지 않으면 생성하 user는 메모리에만 저장되기 때문에 꼭 저장 ..




- 테이블 생성 :

```sql
CREATE TABLE test(id int, name VARCHAR(30));

INSERT INTO test VALUES(1, "hadoop");
INSERT INTO test VALUES(2, "spark");

select * from test;
```



### MySQL & Spark 연결

- mysql과 spark연결하려면 **mysql server와 java를 연결하는 driver가 필요**(spark는 jvm즉 java기반임)

  - jdbc외에도 odbc나 python 패키지로도 연결 가능 
  - mySQL외에도 JDBC나 ODBC지원하는 모든 SQL서버는 스파크랑 연결 가능!

- :white_check_mark:jdbc란 ?

  - stands for Java database connectivity
  - 자바 프로그램이 db 관리 시스템을 접근 가능하게끔 도와줌

  


##### 1. driver설치

- mysql 접속 : https://www.mysql.com/
- downloads->mysql community(GPL) downloads->connector J
  - connector J : mySQL과 Java언어로 개발된 client app을 연결해주는 드라이버로 jdbc임

- OS는 **ubuntu로!!!!!!!!!!!**

  - 우리가 사용중인 ubuntu는 20.04버전이라서 이걸로 선택
- download-> no thanks, just start my download의 **copy link**

![image](https://user-images.githubusercontent.com/96896873/159167166-766f2997-12e1-41f9-a19b-ed9402b02c72.png)

- `wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java_8.0.28-1ubuntu20.04_all.deb`
- 압축해제 :

  - deb파일 압축 해제는 `dpkg -i`
    - i flag는 install 뜻 
  - ` sudo dpkg -i mysql-connector-java_8.0.28-1ubuntu20.04_all.deb `



##### 2. connector 경로 확인

- `cd /usr/share/java/`

- `ls` : mysql-connector 잘 들어가 있는거 확인!

  - `/usr/share/java/mysql-connector-java-8.0.28.jar` <- 이 경로 잘 기억하기. 후에 spark에서 경로로 지정해줄 것임

  

##### 3. 스파크 설정

- `cd $SPARK_HOME/conf`



- `vim spark-defaults.conf`

```python
# spark.master 밑에 mysql-connector 경로 입력하기 :
spark.jars /usr/share/java/mysql-connector-java-8.0.28.jar
```

- :white_check_mark: spark.jars는 

##### 4. 작동 확인

- `cd `

- `start-all.sh`
- `pyspark`

- ```python
  # 변수 만들기 :
  user="root"
  password="1234"
  url="jdbc:mysql://localhost:3306/mysql" # 오라클 dbeaver할 때와 동일
  driver="com.mysql.cj.jdbc.Driver" #: 최근 버전은 cj라는 단어가 추가로 들어감
  dbtable="test"
  ```

- 참고 : https://spark.apache.org/docs/2.4.0/sql-data-sources-jdbc.html
- `jdbc:mysql://[host명][포트명]/[db명]`
  - `jdbc:mysql://localhost:3306/mysql` : 기본포트가 3306
- `driver명` : JDBC 드라이버의 class name 작성
  - `com.mysql.cj.jdbc.Driver` : MySQL의 Connector/j의 class name이 이걸로 고정. 



- MySQL에서 생성했었던 테이블 불러오기 :

```python
test_df = spark.read.format("jdbc").option("user", user).option("password", password).option("url", url).option("driver", driver).option("dbtable", dbtable).load()
```

- :white_check_mark: option말고 options라고 작성하면 한번에 여러 option 작성 가능



- DB 테이블에 행 추가하기 : 
  - 데이터프레임으로 만든 뒤에 insert 가능!

```python
# ID와 name을 컬럼으로 하는 리스트 -> rdd -> df로 만들기 :
test_insert = [(3, "mysql"), (4, "zeppelin")]
insert_df = sc.parallelize(test_insert).toDF(["id", "name"]) 

# mode는 append로!
insert_df.write.jdbc(url, dbtable, "append", properties={"driver" : driver, "user" : user, "password" : password}) 

# 확인
test_df.show() : append된 모습 !
```

- `exit()`

:white_check_mark: 지금 하는 모든 것들은 zeppelin노트북에서도 가능!

