# Oracle

#### Tables

##### emp

![image](https://user-images.githubusercontent.com/96896873/157049303-f1150fb9-c5d7-4309-abce-6282439d1aad.png)

##### dept

![image](https://user-images.githubusercontent.com/96896873/157049394-1c85baab-5c29-4cf4-9d83-4f6622260a57.png)

##### salgrade

![image](https://user-images.githubusercontent.com/96896873/157049436-7ab80204-e062-4bfe-9f69-8d214eb25409.png)

## Functions

- `lpad & rpad(컬럼, 길이, 채워질문자) `: 지정한 길이만큼의 오른쪽/왼쪽 정렬 후 빈 공간은 지정한 문자로 채워짐
  - `select lpad(ename, 7, '0') from emp;` => '0085000'
  - lpad가 왼쪽에 지정한 문자 채워지는거 잊기 ㄴㄴ!
- `ltrim & rtrim('문자열', 제거할 문자(열))` : 지정한 문자를 왼쪽/오른쪽부터 찾아서 제거
  - 제거할 문자열이 `abc` 라면 a/b/c 이렇게 각자 제거한다는 것임
  - 모두 제거하는 것이 아니라 중간에 멈추는 부분(지우지 않아도 되는 부분)이 나오면 더이상 지우지 않음!
  - `select ltrim('acbccdab', 'abc') from dual;` => dab
  - 숫자 지우기 : `select rtrim('acbccdab 1234', '123456789') from dual` => acbccdab 
  - 공백도 제거하고 싶다면 `' 123456789'` 이렇게 작성!
- `substr (컬럼, 인덱스, 가져오고 싶은 문자열 수)` : 가져오고 싶은 위치의 값 원하는 길이만큼 가져오기
  - 오라클에서 인덱스는 1부터
  - `select ename, substr(ename, 3, 2), substr(ename, -2) from emp; ` => emp에서 ename, ename의 3번째부터 2글자, 뒤에서 두번째 글짜부터 끝까지 읽어오기
- `instr(컬럼, 찾는 문자열, 시작위치(양수 : 앞에서/ 음수: 뒤에서), nth)` : 인덱스 가져오기
  - 뒤에서부터 찾아도 인덱스는 앞에서부터 출력
  - 만약 nth가 given이라면, nth번째로 찾은 문자열의 인덱스 반환
  - 없으면 0 반환
  - `select instr('abcccva', 'a', -1, 2) from dual;` => 7이 아니라 1반환 (2nd 찾으니까)

- `length/lengthb` : 길이 반환/ 바이트 반환
  - DB가 UTF-8이라면 -> 한글은 3바이트로 잡힘



##### 소수점 관련 함수

- `round` : 반올림 ; 디폴트는 1의 자리
  - `select round(555.555)` => 556
  - `select round(555.555, -1)` => 560

- `trunc` : 버림
  - round와 문법 동일

- `ceil` : 올림 __자리수 지정 불가능 ; 무조건 1의 자리__
  - `select ceil(sal/1000) * 1000 from emp;` => 이렇게 하는

- `floor` : 버림 자리수 지정 불가능 ; 무조건 1의 자리



##### 날짜 관련 함수

- `add_months(날짜, 더하려는 개월수)` : 지정한 개월 수 이후의 날짜를 출력
  - `select ename, hiredate, add_months(hiredate, 24) from emp;`
- `months_between(날짜1, 날짜2)` : 두 날짜 사이의 개월수 반환
  - 날짜1 - 날짜2 ; 최근 날짜가 앞에

:white_check_mark: 날짜는 바로 숫자로 변경 불가능(역방향도 불가능)! => 숫자 <> 문자열 <> 날짜



##### 데이터 타입 변환 함수

- `to_char(input, [,형식])` : 문자열 변환
  - 형식 :
    - `9` : 자리수 지정
      - `to_char(1234, '9999999')` =>    1234 (앞에 공백 있음) 
    - `0` : 남는 자리 0으로 표시 (앞자리가 0으로 채워짐)
    - `$` or `L` : 원화 표시
    - `.` or `,`  : . 또는 , 표시
      - `to_char(1234, 'l9,999')` => \1,234
  - 날짜 => 문자열
    - HH24 => 24시간
    - MI => 분
    - SS => 초
    - YYYY => 년도
    - MON => 달(숫자) + 월 ; 예)3월
    - MM => 달(숫자만) ; 예) (03)
    - DD => 날짜(숫자만)
    - DY => 요일 (월)
    - DAY => 요일 (월요일)
    - FM => 공백 or 0 제거, FM이후의 0은 다 제거하게 되어있음!!!!!!!!!!
      - `to_char(sysdate, 'YYYY-FMMM-DD DAY')` => 2022-3-07 월요일
    - YEAR => 년도를 영어로 TWENTY TWENTY-TWO
    - Q => 분기
- `to_date` : __문자열로__ 바뀐 숫자를 날짜로 바꾸기
  - `select to_date('20220307', 'YYYYDDMM') from dual;`
  - 날짜 형태의 문자열의 형식만 바꾸기
  - `select to_char(to_date('20220307 213020', 'YYYYDDMM hh24miss'), 'YYYY, MON hh:mi:ss') from dual;` => 2020, 월 21:03:20

-  `to_number` : 숫자 형태의 문자열 값을 숫자로 변경



##### 조건 관련 함수

- `decode(컬럼, 비교값, 같을 때 반환값)`
  - `select decode(job, 'MANAGER', 1 , 'SALESMAN', 2, 0)` : 직업이 manager라면 1, salesman이라면 2, else는 0
- `case when 조건 then '조건 맞을 때 반환값' when ... else END ` 
  - `select case when job = 'MANAGER' then 1 when job = 'SALESMAN' then 1 else 0 end as jobno, job `  
  - else 미지정시 null값 반환
  - END 작성 잊지말기!!!!!



##### 결측값 처리 함수

- `NVL(컬럼, 대체할 값)` 
  - `select count(nvl(comm, 0)) from emp;`



##### 그룹 함수

- select 구문에 집계함수 하나 꼭 작성!!!!

- `rollup(a, b)`
  - ab집계, a집계, 전체집계 출력. (작성한 집계함수를 ab별, a별, 전체집계별로 보여주는 것임)
  - `select job, deptno, avg(sal) from emp group by rollup(job, deptno);` => (job, deptno)별 avg(sal), job별 avg(sal), 총 avg(sal)
- `cube(a,b)`
  - rollup과 반대로 출력
  - 전체집계, ab집계, b집계, a집계 출력

- `grouping sets` 
  - 그룹함수(rollup / cube)의 결과에 원하는 결과 추가할 때 사용!
  - `select job, deptno, avg(sal) from group by grouping sets(rollup(job, deptno), deptno)` => 원래는 deptno별로는 안보여주는데 grouping sets를 통해서 deptno별 avg(sal)도 추가됨



##### 정렬 하기 

1. rownum 활용

- `rowid`  : 해당 로우에 대한 내부적인 식별자, 불변
- `rownum` : 엑셀의 왼쪽 숫자와 비슷한 개념, 변함
  - 만약 한 행이 삭제된다면 rownum은 변한다
  - 하지만 orderby를 한다면 처음 insert한 순서 그대로 rownum이 뒤죽박죽이 된다

- 만약 rownum을 활용해서 원하는 열을 가지고 오고 싶다면 :

```sql
SELECT ename, sal, rn
FROM (SELECT ename, sal, rownum AS rn
	  FROM (SELECT ename, sal FROM emp ORDER BY sal DESC))
WHERE rn >= 3 AND rn <= 5;
```

- 제일 내부 쿼리문은 순서대로 정렬한 쿼리문 : 1. 정렬을 한다
- 두번째 쿼리문은 rownum을 별칭을 줘서 변경되지 않는 rownum을 가져오는 쿼리문 : 2. 정렬한 쿼리문의 rownum을 별칭을 주어서 가져온다
- 세번째 쿼리문은 별칭을 준 rownum을 활용해서 원하는 행을 가져온다 : 3. 원하는 행 가져오기



:white_check_mark: 왜 rownum을 바로 사용 불가능할까?

답 : rownum은 한 행씩 가져와서 where절에 있는 조건과 비교를 하기 때문에 앞서 조건이 불만족인 행이 있다면 새로 가져온 행의 rownum은 계속 1이니까

- rownum = 1 : 첫 행은 rownum이 1이니까 조건 만족, 다음 행부터는 rownum이 2로 되어서 조건 불만족 -> 정상적으로 출력
- rownum = 3 : 첫행은 rownum이 1이니까 조건 불만족, 다음 행도 rownum이 1부터 시작 -> 아무것도 출력 X
- rownum <=3 : 첫행은 rownum이 1이니까 조건 만족, 그 다음 행 가져왔을 때 (앞에 행이 조건에 맞으므로 count가 된 것이니) rownum은 2가 되니까 가져와짐. 
- rownum >= 3 : 첫행은 rownum이 1이니까 조건 불만족, 그 다음 행도 rownum이 1부터 시작 -> 아무것도 출력 X 



2. rank활용

- `rank() over(정렬된 컬럼)` : 동일한 값, 동일한 순위, 동일한 값의 개수만큼 누적 순위
- `dense_rank() over(정렬된 컬럼)` : 동일한 값, 동일한 순위, 건너뛰기 없음
  - 예) 3등이 2명인데도 다음 등수는 4등
- `row_number() over(정렬된 컬럼)`:  동일한 값이더라도 고유한 순위 부여



##### 기타 함수

- `UPPER()/LOWER()` : 대문자/소문자
- `initcap()` : 첫글자는 대문자, 나머지는 소문자



##### like

- `select ename from emp where ename like '%M%'`
- `_` : 하나의 문자 무조건 필요
- `%` : 0 또는 그 이상의 문자 가능
- like 대신 = 사용 가능하지만 대부분 like 사용


