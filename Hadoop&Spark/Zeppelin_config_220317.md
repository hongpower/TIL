# Zeppelin

## Configuration

- Zeppelin_installation에서 만든 소프트링크 잘 만들어졌는지 확인!

- `sudo vim ~/.bashrc`

```python
# zeppelin
export ZEPPELIN_HOME=/home/jisu/zeppelin
export PATH=$PATH:$ZEPPELIN_HOME/bin
```

- `source ~/.bahsrc`



- `cd $ZEPPELIN_HOME/conf`

- `ls` : template 파일들 존재 확인 가능



- template 복사해서 .sh 파일 만들기 시작!

- https://zeppelin.apache.org/docs/0.8.2/setup/operation/configuration.html 참고

##### zeppelin-env.sh

- `cp zeppelin-env.sh.template zeppelin-env.sh`

- `vim zeppelin-env.sh`

```python
# 19번째 줄 
export JAVA_HOME=/home/jisu/java

# 79번째 줄 
export SPARK_HOME=/home/jisu/spark

# 89번째 줄
export HADOOP_CONF_DIR=/home/jisu/hadoop/etc/hadoop 
```

- :white_check_mark: 여기 경로 작성할 때 꼭꼭꼭 맨 앞에 /붙이는거 잊지말기 ! 실수 잦음



##### zeppelin-site.xml

- `cp zeppelin-site.xml.template zeppelin-site.xml`

- `vim zeppelin-site.xml`

```python
# 마지막 줄로 이동

## zeppelin.server.addr 수정 :
# 기존 :
127.0.0.1 #-> 내부에서만 접속 가능
# 바꾸는 값 :
0.0.0.0 #-> 어디서든 접속 가능

## zeppelin.server.port 수정 :
# 기존 :
8080 #=> 이거 다른 애들이 이미 많이 쓰는 포트라서 변경하기
# 바꾸는 값 :
8082 
```



![image](https://user-images.githubusercontent.com/96896873/158820832-6582db67-6f7f-4dac-afea-66ba9e9812d7.png)



