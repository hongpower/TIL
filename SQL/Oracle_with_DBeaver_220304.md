# Oracle

- CMD에서 Oracle 접근하기:
  - `sqlplus` -> `system` -> `pw입력` 
  - sqlplus는 __내 컴퓨터__ 내의 데이터베이스서버를 찾아서 클라이언트가 조작가능하도록 해줌
    - 클라우드에 데이터베이스를 설치 했다면 sqlplus 불가능

- 범용 데이터베이스 전용 툴 :

  - DBeaver

  - toad

  - datagrip

- 각 db의 전용 툴 :

  - oracle - sql developer

  - mysql - workbench

  - mongodb - 콤파스



## DBeaver

- DBeaver 설치
  - [DBeaver Community | Free Universal Database Tool](https://dbeaver.io/) 접속
  - Download - Windows 64 bit 설치
  - 설치 후 실행

#### Connecting Tool & DB

- 내 컴퓨터 내에 있는 DB서버만 선택 가능

- DBeaver 내에 왼쪽 상단 콘센트모양+

![image](https://user-images.githubusercontent.com/96896873/156920629-7ce2aa20-6a8a-49e0-8273-838fb1043add.png)

1. 오라클 선택

2. Database 이름은 ORCL -> xe로 바꾸기

   - ORCL : 유료

   - xe : 무료

3. Edit driver Settings 클릭

4.  Libraries 클릭 

5. 기존 내용 4개 다 삭제

6. Driver 다운로드 (DBeaver는 자바로 만들어져 있으며 오라클 DB와 자바를 연결해줄 Driver 필요)

   1. 오라클 홈페이지 접속

   2. resource -> 소프트웨어 다운로드

   3. 드라이버 및 유틸리티

   4. JDBC -> 18C 다운

      JDBC : JAVA DB CONNECT

7.  OJDBC8.JAR를 라이브러리에 추가. 



### Setup

![image](https://user-images.githubusercontent.com/96896873/156920758-ac07aafe-0597-4ee7-beaa-7b0616a97d42.png) 클릭!



## Oracle Practice

==== 여기서부터는 수업 진행 내용 ===

- scott.txt 복붙한 후 하나 씩 ctrl+ enter 눌러가면서 실행 시키기
- 내용 다 지우고 작업 이제 시작!



- char vs varchar2
  - varchar : 가변길이 ; 내가 넣어준 길이 만큼만 메모리를 차지
  - char : 고정길이 ; 내가 지정한 길이만큼 메모리 차지



- Auto increment VS Sequence
  - 증감을 표현할 때 사용 (sequence는 감소도 가능)

| Auto increment                                               | Sequence                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| MySQL                                                        | Oracle                                                       |
| 테이블 생성 시에 auto_increment 속성을 시퀀스로 지정해줄 컬럼에 적용 | oracle은 자동 증가 컬럼이 사용 불가능하기 때문에 sequence 활용. sequence 생성 후 적용 |

- Auto increment:
  - `CREATE TABLE test_table(id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(5));`
- Sequence:
  - 시퀀스 생성 : `CREATE SEQUENCE 시퀀스명 INCREMENT BY 1 START WITH 1 MINVALUE 1 MAXVALUE 100 NOCYCLE NOCACHE NOORDER;`  
    - 뒤에 옵션 다 생략하고 그냥 `CREATE SEQUENCE 시퀀스명` 도 가능!
  - 시퀀스 값으로 넣기: `INSERT INTO test_table(col1) VALUES(시퀀스명.NEXTVAL);`
- `CYCLE or NOCYCLE` -> 순환 여부
- `CACHE or NOCACHE` -> 메모리에 시퀀스값 미리 할당 여부

- `ORDER or NOORDER` -> 순서 여부



:heavy_check_mark: dual은 오라클에서 자동으로 생성되는 테이블로서 **간단하게 함수를 이용해서 계산 결과값 확인할 때 유용한 테이블**. 즉 함수에 대한 쓰임을 알고 싶을 때 **특정 테이블 생성 필요 없이** 함수 값을 리턴 받을 수 있음

- 예시 : `select 시퀀스.nextval from dual;`, `select current_date from dual;`



#### DDL

- 전체 복사
  - `create table 새로운 테이블 as select * from 테이블`

- 원하는 컬럼만 복사
  - `create table 새로운 테이블 as select 컬럼 from 테이블`

- 구조만 복사 (컬럼명은 포함)
  - `create table 새로운 테이블 as select * from 테이블 where 3 = 4;`
    - 1=2 처럼 false인 값을 넣기
    
      

- 데이터 무결성
  - 데이터가 손상되거나 원래의 의미를 잃지 않고 유지되는 상태

  - 제약 조건 :
    
    - 제약 조건 방법 :
      - 컬럼에서 :
        - `CREATE TABLE 테이블명( ID CHAR(3) UNIQUE, ... );`
      - 테이블에서 :
        - `CREATE TABLE 테이블명( ID CHAR(3), ..., CONSTRAINT 제약조건명 제약조건 (컬럼명))`
    - 입력되는 데이터들의 규칙 지정
    - not null : null 입력 불가 **(컬럼에서만 설정 가능)**
    - unique : 해당 컬럼 or 복합 컬럼의 값 유일 (컬럼, 테이블 가능)
      - null값 허락함(즉 not null이 제외되어 있음). not null은 하나의 값으로 인식안하기 때문에 여러개 가능
    - primary key : 각 행을 유일하게 식별 (컬럼, 테이블 가능)
    - foreign key : 다른 테이블 값 참조 (컬럼, 테이블 가능)
      - `references `사용!
      - 예) `CREATE TABLE test02( ID char(3) ... ADDRESS varchar(30) REFERENCES test01(address))`
      - 예2) `CREATE TABLE test02( ..... CONSTRAINT 제약조건명 FOREIGN KEY (ADDRESS) REFERENCES TEST01(address))`
    - check : 특정 값만 넣고 싶을 때 (컬럼, 테이블 가능)
      - `check` + `in` 사용!
      - 예) `CREATE TABLE test03( ID char(3) ... gender char(1) CHECK (gender in ('F', 'M')));`
      - 예2) `CREATE TABLE TEST03( ... CONSTRAINT 제약조건명 CHECK (gender in ('F', 'M')));`
      - 대문자/소문자 구분함!
    
    - 만약 제약 조건이 여러개의 컬럼에 묶여있다면 여러개의 컬럼을 하나의 컬럼으로 보아야함
      - 예) `CONSTRAINT 제약조건명 UNIQUE (ID, NAME)` -> ID가 같더라도 NAME이 다르다면 UNIQUE한 것으로 본다
      - 예) `CONSTRAINT 제약조건명 UNIQUE (ID), CONSTRAINT 제약조건명 UIQUE (NAME)` -> 다르게 봐야함

#### DML

- UNION (행을 늘림. 즉 같은 컬럼끼리 합쳐서 행들을 합침)
  - UNION : 중복 제거 후 병합
    - `SELECT deptno FROM dept UNION SELECT deptno FROM emp;`
  - UNION ALL : 중복 허용 병합
    - `SELECT deptno FROM dept UNION ALL SELECT deptno FROM emp;`
  - INTERSECT : 교집합
    - `SELECT deptno FROM dept INTERSECT SELECT deptno FROM emp;`
  - MINUS : 차집합
    - `SELECT deptno FROM dept MINUS SELECT deptno FROM emp;`



- UPDATE
  - UPDATE 테이블명 SET 컬럼 = 값 WHERE 조건
  - 예) `UPDATE test01 SET name='jisu', job='baeksoo' where id=981023`



##### Merging values from two different columns

- `||`
  - `SELECT name || '는 직업이 '|| job || '입니다.' AS "INTRODUCE" FROM emp;`
  - 쌍따옴표와 따옴표 주의할 것!
- `+`
  - `SELECT sal + comm FROM emp;`
  - 숫자끼리 더하기



##### Date

- 날짜 작성 방법 : '1980-12-17' or '1980/12/17' or '80/12/17' or '80-12-17'
- 만약 날짜 형식으로 인식 못하면:
  - ` where hiredate = to_date('1970-12-17', 'yyyy-mm-dd')` <- 이처럼 강제로 부여하기



- IN
  - `SELECT name, address FROM emp WHERE id in (981023, 970308, 960901)`



- ORDER BY
  - SELECT 이후에 수행이 된다! -> Column 위치로 가져오기 가능!
  - `SELECT job, avg(sal) AS "평균" FROM emp GROUP BY job ORDER BY 2 desc;`



- 몇몇 부분에서는 having과 where절의 차이가 없지만, 구분을 위해서 :

  - having -> 집계함수에 조건줄 때 이용
  - where -> 이외의 컬럼에 조건줄 때 이용

  - `SELECT deptno, sum(val) FROM emp where deptno <> 30 GROUP BY deptno having sum(val) >= 8000 ORDER BY 2 DESC;`

  - :white_check_mark: Python의 `!=` == `<>`

  

## 기타 

- '' 는 NULL로 인식 but ' '는 NOT NULL임

- 만약 제약조건 이름 안만들면 자동으로 만들어짐

- gpu -> 분석/cpu -> 일반적으로 빠른거 원할 때

- pl/sql : Extended SQL로 기존 SQL에서 확장된 기능들이 추가된 것임. 예를 들어 하나씩 쿼리 실행할 필요가 없어진다거나 ..

- "" VS '':
  - 오라클에서 '' : 문자열 감싸주는 기호
  - "" : 컬럼명, 별칭 등을 감싸주는 기호

- `sysdate` : 오늘 날짜 불러오기
  - 컴퓨터 시간에 따라 형태가 다르게 나옴
  - `select sysdate from dual;`

