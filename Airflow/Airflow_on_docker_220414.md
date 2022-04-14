# Airflow on Docker

## 1. Airflow 설치

> Airflow 2.4버전 설치할 것임. 참고 : https://airflow.apache.org/docs/apache-airflow/stable/start/docker.html

1. `docker-airflow`라는 폴더 생성
2. terminal에서 `docker-airflow`내로 이동한 후 `curl -LfO 'https://airflow.apache.org/docs/apache-airflow/2.2.5/docker-compose.yaml'` 입력
3. `ls`명령어로 `docker-compose.yaml`이 잘 생성됐는지 확인하기
4. `mkdir ./dags ./plugins ./logs`를 통해 dag, plugin, log폴더 새로 생성



:white_check_mark: Mac OS/Linux 유저의 경우에는 .env파일을 만들어서 아래 추가 작업 필요 :

> 실행하려는 이미지에게 호스트유저 아이디와 그룹아이디를 명시해야하는데 . Mac os/linux의 경우에는 root permission하고 airflow의 user와 동일하게 맞춰줘야함. 즉, airflow의 디폴트 user id는 50000이지만 맥/리눅스의 user id와 다르기때문에(맥은 디폴트가 501) 이걸 명시해주는 작업을 따로 해줘야하는 것임.

- `echo-e "AIRFLOW_UID=$(id -u)\nAIRFLOW_GID=0" > .env` 작성

  - `echo -e` : 백슬래쉬를 해석할 수 잇는 echo

  - `$(id -u)` : user의 id. 현상황에서는 맥의 root user의 디폴트 id인 501을 AIRFLOW_UID 라는 변수에 넣은 것임. (docker-compose.yaml에는 airflow_uid가 50000으로 돼있어서 통일시켜주기 위해 .env에 작업한 것임.)

  - `> .env` : .env파일 생성해서 해당 내용 삽입

- 설명 :
  - UID : airflow container안의 user id
  - GID : group id
  - 만약 후에 디폴트 user id이름이 airflow대신 다른 유저로 image를 run하고 싶다면 유저의 gid는 무조건 0 (root)으로 설정해야함. 만약 gid가 0이 아닌 다른 그룹으로 접근을 한다면 에러 발생



5. `docker -compose up airflow-init`
6. `docker-compose up`

7. 잘 진행됐는지 확인을 위해 새로운 터미널을 열어서 `docker ps` 라는 명령어 입력해서 어떤 container가 running중이고 healthy한지 체크
8. 브라우저에서 `localhost:8080` 입력해서 GUI로 체크하기

:white_check_mark: `docker exec 컨테이너아이디 airflow version` -> 특정 컨테이너 airflow 버전 확인 가능



9. api와 연동확인을 위해서 : `curl -X GET "http://localhost:8080/api/v1/dags"` 입력하면 unauthorized결과가 나옴. 이런 경우에는 `docker-compose.yaml` 에서 새로운 환경변수 추가 :

- `AIRFLOW__API__AUTH_BACKEND: 'airflow.api.auth.backend.basic_auth'`
- 본인 같은 경우에는 자동으로 생성되어잇었음!

- ![image-20220414031054593](/Users/jisu/Library/Application Support/typora-user-images/image-20220414031054593.png)



10. docker 종료후 다시 시작 : `docker-compose down && docker-compose up`

11. 유저 옵션 명시해서 다시 api 접근 : `curl -X GET --user "airflow:airflow" "http://localhost:8080/api/v1/dags"`
    - `--user 아이디:비밀번호 ` 형식으로 작성



## 2. Airflow 기초

### DAG

- Directed Acyclic Graph
- airflow에서 여러 task로 구성된 하나의 워크플로우를 보여주는 그래프. 각 task는 node로 구성되어있고 각 edge는 task들의 dependency를 묘사함

### Operator

- DAG가 실행되면 각 task마다 어떤 일을 수행할 것인지 결정함.

- 예시 :
  - python function으로 구성된 task에는 python operator
  - bash command으로 구성된 task에는 bash operator
  - db에 data를 insert하는 task에는 postgresoperator 



## 3. Airflow 실습

- airflow의 dag는 `dags`파일 내부에 `.py`형식으로 작성해서 생성 가능하다



- 최종 파일 모습 :

````python
from airflow import DAG
from airflow.operators.python import PythonOperator, BranchPythonOperator
from airflow.operators.bash import BashOperator

from random import randint
from datetime import datetime

def _choose_best_model(ti):

    # xcom은 data를 테스크끼리 공유할 때 사용하는 걸로 데이터베이스에 있는 데이터 가져올 수 있음
    # xcom을 통해서 a, b, c 테스크에서 가져온 데이터를 pull하기 :
    accuracies = ti.xcom_pull(task_ids=[
        'training_model_A',
        'training_model_B',
        'training_model_C'
    ])

    best_accuracy = max(accuracies)

    if (best_accuracy > 8):
        return "accurate"
    else:
        return "inaccurate"

def _training_model():
    return randint(1, 10)

# 인스턴스 생성 dag object with content manger를 생성한 것임
# DAG(ID, start_date, scheduling_interval(cron expression), catchup=False)
# 1월 2일 미드나잇에 trigger댐. latest non-triggered dag will be only automatically triggered.
with DAG("my_dag", start_date=datetime(2022, 1, 1), schedule_interval="@daily", catchup=False) as dag:


    ## first 3 tasks calling Python function :  
    # task_id : unique task_id 무조건 중요!
    # python_callable : python function I want to call
    training_model_A = PythonOperator(
        task_id="training_model_A",
        python_callable=_training_model
    )

    training_model_B = PythonOperator(
        task_id="training_model_B",
        python_callable=_training_model
    )

    training_model_C = PythonOperator(
        task_id="training_model_C",
        python_callable=_training_model
    )

    ## 
    choose_best_model = BranchPythonOperator(
        task_id="choose_best_model",
        python_callable=_choose_best_model
    )

    ##
    accurate = BashOperator(
        task_id="accurate",
        bash_command="echo 'accurate'"
    )

    inaccurate = BashOperator(
        task_id="inaccurate",
        bash_command="echo 'inaccurate'"
    )

    # 순서 정하기(upstream, downstream)
    # 작성할 때 task id로 작성하면 된다. (함수는 앞에 언더바 붙여놓았음)
    [training_model_A, training_model_B, training_model_C] >> choose_best_model >> [accurate, inaccurate]
````



1) `my_dag.py`파일 생성

2. dag object 생성

- dag object 속성 : 
  - ID
  - start_date : datetime 객체로 작성
  - schedule_interval : cron expression으로 작성
  - :white_check_mark: catchup=False : 주기적으로 DAG가 생성되고 run될 것인데 이때까지 생성된 DAG들 중에서 가장 최근의 non-triggered DAG만 자동으로 실행하도록 옵션을 준 것임.

- ```python
  from airflow import DAG
  
  from datetime import datetime
  
  with DAG("my_dag", start_date=datetime(2022, 1, 1), schedule_interval="@daily", catchup=False) as dag:
  ```

3. 파이썬 함수를 실행하는 3가지 task 생성 :

- PythonOperator 파라미터 2가지 :
  - task_id : task마다 고유한 task id
  - python_callable : 실행할 python function

- ```python
  from airflow.operators.python import PythonOperator
  
  from random import randint
  
  
  # 랜덤한 숫자를 불러오는 함수 생성 :
  def _training_model():
      return randint(1, 10)
    
    
  with DAG("my_dag", start_date=datetime(2022, 1, 1), schedule_interval="@daily", catchup=False) as dag:
    
    # 3가지 task 추가 :
  		training_model_A = PythonOperator(
          task_id="training_model_A",
          python_callable=_training_model
      )
  
      training_model_B = PythonOperator(
          task_id="training_model_B",
          python_callable=_training_model
      )
  
      training_model_C = PythonOperator(
          task_id="training_model_C",
          python_callable=_training_model
      )
  ```



4. 3가지 task의 결과값 중 가장 큰 숫자를 뽑아내고 그에 해당되는 accurate/inaccurate 결과를 가져오는 task 생성

- 3가지의 python 파일의 결과값을 가져오기 때문에 branch python operator 활용
- xcom은 data를 테스크끼리 공유할 때 사용하는 것으로, db에 있는 데이터를 가져올 때 활용한다.

- ```python
  from airflow.operators.python import PythonOperator, BranchPythonOperator
  
  
  def _choose_best_model(ti):
  
      # xcom을 통해서 a, b, c 테스크에서 가져온 데이터를 pull하기 :
      accuracies = ti.xcom_pull(task_ids=[
          'training_model_A',
          'training_model_B',
          'training_model_C'
      ])
  
      best_accuracy = max(accuracies)
  
      if (best_accuracy > 8):
          return "accurate"
      else:
          return "inaccurate"
  
  with DAG("my_dag", start_date=datetime(2022, 1, 1), schedule_interval="@daily", catchup=False) as dag:
    
  		training_model_A = PythonOperator(
          task_id="training_model_A",
          python_callable=_training_model
      )
  
      training_model_B = PythonOperator(
          task_id="training_model_B",
          python_callable=_training_model
      )
  
      training_model_C = PythonOperator(
          task_id="training_model_C",
          python_callable=_training_model
      )
     
     choose_best_model = BranchPythonOperator(
          task_id="choose_best_model",
          python_callable=_choose_best_model
      )
  ```



5. accurate/inaccurate 결과에 따라 실행되는 테스크 생성 - bash operator 사용

- task_id만 있으면 바로 하나의 task 선택해서 실행하게 만들기 가능

- bashoperator 파라미터 :

  - task_id
  - bash_command : 실제로 입력될 배쉬 명령어

- ```python
  with DAG("my_dag", start_date=datetime(2022, 1, 1), schedule_interval="@daily", catchup=False) as dag:
  
  .
  .
  .
  
  	accurate = BashOperator(
          task_id="accurate",
          bash_command="echo 'accurate'"
      )
  
      inaccurate = BashOperator(
          task_id="inaccurate",
          bash_command="echo 'inaccurate'"
      )
  
  ```



- 여기까지 했을 때 GUI에서 확인한 모습 :

![image-20220414021722955](/Users/jisu/Library/Application Support/typora-user-images/image-20220414021722955.png)

6. 순서 정하기

- 작성할 때는 task_id로 작성하면 됌. (현재 예제에서는 함수는 맨 앞에 언더바 붙여서 task_id와 구분가도록 해놓음)
  - `    [training_model_A, training_model_B, training_model_C] >> choose_best_model >> [accurate, inaccurate]`









해결할 문제 : 일일이 로그인 하지않고 자동 update하는 방법? 왜 unhealthy?