# MySQL

### DataBase

- RDBMS : 정형데이터들(형태가 정해져있는 데이터) => MySQL은 RDBMS
- NoSQL(Not only SQL) : 빅데이터등 비 정형 데이터 저장



- Entity(table) : 객체, 데이터베이스에 저장하고자 하는 현실상의 개념
- Attribute(column): Entity의 속성
- Tuple(row): Entity의 값



- 세미콜론 꼭 찍기



MySQL 접속 :

- 명령프롬프트 열어서, 
- `mysql -u root -p` : mysql 유저는 root이고, password는...
- password 입력



### SQL

- SQL로 서버에 요청하면 그에 맞는 데이터를 응답받게 됨
- DDL(정의언어) : DB스키마 정의, 조작
  - CREATE, ALTER, DROP
- DML(조작언어) : 데이터를 조작
  - SELECT, INSERT, UPDATE, DELETE
- DCL(제어언어) : 데이터를 제어 (권한과 관련된 일 등)
  - TCL(COMMIT, ROLLBACK) => Transaction control language로 데이터 및 트랜잭션을 저장하거나 (마지막 저장된 곳으로) 복귀(취소)
  - (GRANT, REVOKE) => DB권한 부여, 취소



:white_check_mark: view : 데이터를 보여만 주기 위해서 사용 (수정 불가능)

:white_check_mark: procedure : 특정 작업에 필요한 query들을 함수처럼 사용



- 주석 : --
- 데이터베이스 조회 : `show databases;`
- 해당 데이터베이스 사용 : `use mysql;`
- 데이터베이스 내 테이블 조회 : `show tables;`
- table 열등 정보 확인 : `desc students;`
- 데이터 확인 : `select * from students;`
- mySQL 나가기: `exit;`



### DDL 

##### CREATE

- Database 생성
  - `create database multi;` => multi이름의 db 생성

- table 생성

  - `create table students` => students이름의 테이블 생성

  - `create table students(di int, name varchar(100), phone char(13), address varchar(1000));`

  - varchar : var character

  - ```sql
    create table students(
    
    id int,
    
    name varchar(100),
    
    phone char(13),
    
    address varchar(1000));
    ```
    
  



##### ALTER

- 테이블 수정 : (컬럼 추가) ; ALTER TABLE + ADD

- ```sql
  alter table students
  add job varchar(100);
  ```

- 테이블 수정 : 컬럼 삭제 ; ALTER TABLE + DROP

- ```sql
  alter table students
  drop job
  ```

- 테이블 수정 : 컬럼 수정; ALTER TABLE + MODIFY

- 수정 : student의 job 컬럼의 데이터 형태를 바꾸고 싶을 때

- ```sql
  alter table students
  modify job varchar(1000);
  ```




##### DROP

- 테이블 삭제

- ```sql
  drop table students;
  ```

- 데이터베이스 삭제

- ```sql
  drop database multi;
  ```



### DML

##### SELECT

- 지정한 컬럼의 데이터만 조회:

- ```sql
  select id, name, phone, address, job
  from students;
  ```



##### INSERT

- 데이터 삽입 : (use 테이블 할 필요 X)

- ````sql
  insert into students
  values(1, 'hong-gd', '010-1111-1111', 'seoul');
  ````


- 지정한 컬럼의 데이터만 삽입:

- ```sql
  insert into students(id, name, address, job)
  values (2, 'kim-sd', 'suwon', 'engineer')
  ```



##### UPDATE

- 데이터 수정 

- ```sql
  update students
  set name = '010-2222-2222',
  address = 'suwon',
  job = 'engineer'
  where id='2'
  ```



##### DELETE

- 데이터 삭제

- 모든 데이터 삭제 :

  ```sql
  delete
  from students
  ```

- 일부 데이터 삭제:

- ```sql
  delete
  from students
  where name = 'kim-sd'
  ```

  



#### 외부 db 다운

- `source employees.sql` => employees.라는 외부 db 다운



< 데이터의 개수 세기 >

- `select count(*) from employees`



<limit 걸어서 데이터 출력하기>

- `select emp_no, first_name, last_name from employees limit 10;` => 10개의 데이터만 출력



<where로 조건 걸기>

- `select * from employees where hire_date >= '2000-01-01';`
- `select count(*) from employees where birth_date > "1960-00-00" and birth_date < "1970-00-00";`=> 1960년대 사람들 출력
- 여러 조건은 and / or 사용



< 정렬 >

- order by (디폴트는 오름차순)
- `order by salary desc;` => 내림차순
- 정렬을 한 후에 limit이 있다면 limit을 수행함. 순서 기억하기
- date형의 오름차순은 옛날 데이터부터 최근 데이터 순서로...



#### groupBy

- `group by title`

- group by 하지 않은 컬럼은 select로 출력 불가능하다

  - ```sql
    select title, emp_no --불가능! 왜냐하면, title이라는 이름으로 group을 지었는데 emp_no때문에.
    from titles
    group by title;
    ```

- 집계함수(count와 같은)을 조건에 사용한다면 where 대신 having 사용한다

  - ```sql
    select count(*)
    from employees
    where gender="F";
    --이거는 ok. count가 select에 있으니
    
    select dept_no, count(dept_no)
    from dept_emp
    where count(dept_no) > 50000;
    -- 이거는 불가능. 같은 group별 집계함수는 꼭 having을 쓰자
    
    select dept_no, count(dept_no)
    from dept_emp
    group by dept_no
    having count(dept_no) > 50000;
    ```

  - 

### DCL

##### Commit

- 데이터, 트랜잭션 저장. 즉 체크 포인트 지정. 

- auto commit이 1이라면 즉 true라면 모든 행 마다 commit이 되기 때문에 auto commit을 0으로 만들고 commit을 하면 이제 commit이라는 명령문 실행할 때만 commit을 하게됨. 만약 commit후 rollback이 안된다면 auto commit을 확인해보자

##### Rollback

- 데이터, 트랜잭션 취소 ( 가장 마지막 commit으로 복귀)

##### Grant

- ```sql
  grant all privileges on *.* to 'root'@'localhost' with grant option;
  -- user root에게, '%' => 어디서든 접근가능한 권한 부여
  
  create user 'root'@'%' identified by '비밀번호';
  
  grant all privileges on *.* to 'root'@'%' with grant option;
  
  flush privileges;
  commit;
  ```

- 

##### Revoke



### Join

#### InnerJoin

- 두 테이블의 특정 컬럼의 데이터가 같은 데이터들만 추출

- on OR using활용해서 합칠 기준 열 정하기!

- on 사용:

  - `select * from employees emp inner join dept_emp de on emp.emp_no = de.emp_no`
  - on은 emp_no라는 같은 이름의 컬럼이 2개가 추가 된다.

- using 사용:

  - `select * from employees inner join dept_emp using(emp.no);`
  - using은 on과 튜플 개수는 비슷하지만 컬럼이 1개만 출력된다.
  - on은 두 테이블의 컬럼명이 달라도 지정가능하지만, using의 컬럼명이 같아야 사용 가능

  

#### NaturalJoin

- 어짜피 연결될 수 있는 같은 이름의 컬럼이 하나밖에 없을 때 사용 ( inner join인데 공통 컬럼이 하나밖에 없을 때)
- using, on 생략 가능
- 그래서 만약 기준이 될 컬럼의 이름이 같다면 굳이 innerjoin안하고 natural join 하면 된다.



#### Join

- using. on 사용하지 않으면 카테시안 곱으로 출력
- using, on을 사용하면? inner join과 같음!



#### Cross Join

- 일반 join중 using, on 사용안했을 때와 결과가 같음
- 즉 카테시안 곱이다.



#### Left Join

-> 왼쪽의 데이터는 다 나오고 열에 해당하는 데이터가 없는 부분은 null값
-> 같은 값 먼저 출력하고, 그 다음에 left 실행한다 생각



#### Right Join

-> 오른쪽의 데이터는 다 나오고 열에 해당하는 데이터가 없는 부분은 null값
-> 같은 값 먼저 출력하고, 그 다음에 right 실행한다 생각



:heavy_check_mark: MySQL은 outer join이 없어서, left join + right join을 union하자!

### 서브쿼리

- select, insert, update, delete 구문 안에 select가 또 들어갈 때 
- 굳이 join을 하지 않고 여러 테이블의 데이터 활용할 때 사용

```sql
use employees;

-- last name이 haraldson인 사원의 월급 출력
select salary
from salaries
where emp_no in
(select emp_no
from employees
where last_name = 'Haraldson' );
```







### MySQL & 파이참 연동

```python
import mysql.connector

# mysql 접속
cnx = mysql.connector.connect(
    host='127.0.0.1',
    user='root',
    password='password명',
    database='선택할 데이터베이스 명'
)
cursor = cnx.cursor()

# usertable 생성 :
# query = 'CREATE TABLE userTable(id char(4), userName char(15), email char(20), birthYear int)'
# cursor.execute(query)

cursor.execute("INSERT INTO userTable VALUES( 'hong' , '홍지윤' , 'hong@naver.com' , 1996)")

# 항상 까먹지 말고 닫기
cnx.close()
```



## 기타:

- 같은 이름의 테이블은 생성 불가능 

- 강제종료 : ctrl + c : 터미널 강제적으로 멈추고 싶을 때, 오타가나와서 빠져나오고 싶을 때

- 별칭 주는 방법 : employees emp 이런식으로 `테이블명 띄어쓰기 별칭`

- mysql bulk insert 검색하면 insert into로 하나씩 데이터 넣지않고 한번에 가능

  - ```sql
    insert into 테이블명 (column1, column2, column3)
    values(1,2,3),(4,5,6),(7,8,9)
    ```

    

  

  