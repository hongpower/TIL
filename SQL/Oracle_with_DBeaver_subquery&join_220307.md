# Oracle

#### Tables

##### emp

![image](https://user-images.githubusercontent.com/96896873/157049303-f1150fb9-c5d7-4309-abce-6282439d1aad.png)

##### dept

![image](https://user-images.githubusercontent.com/96896873/157049394-1c85baab-5c29-4cf4-9d83-4f6622260a57.png)

##### salgrade

![image](https://user-images.githubusercontent.com/96896873/157049436-7ab80204-e062-4bfe-9f69-8d214eb25409.png)



## Subquery(Nested Select)

- query안에 query가 있는 경우
- single row subquery : 결과가 하나
  - `select ename, sal from emp where sal > (select sal from emp where ename='Jones');` => 서브쿼리의 결과가 하나
  - 연산자 사용!
- multi row subquery : 결과가 여러개
  - `select empno, ename from emp where empno not in (select nvl(mgr, 0) from emp);` => 서브쿼리 결과가 여러개
  - in 이나 not in 사용! (연산자 불가능)
- multi column subquery: 결과가 여러개의 __컬럼__
  - `select ename, sal, deptno from emp where (deptno, sal) in (select deptno, sal from emp where job = 'MANAGER')` 
    - 이거 이렇게 풀어쓸 수 도 있음 :
      - `select ename, sal, deptno from emp where deptno in (select deptno from emp where job= 'MANAGER') AND sal in (select sal from emp where job = 'MANAGER')`
  - in 이나 not in 사용
  - 꼭 결과컬럼 개수와 비교하는 컬럼의 개수 맞춰주기!!

- inline view : 결과가 가상테이블로 사용

  - 즉 from 구문에 쿼리절이 있을 때

    ```sql
    select e.ename, e.sal, mydept.deptno, mydept.myavg
    from emp e,
    	(select deptno, avg(sal) as myavg
    	from emp
    	group by deptno) mydept
    where e.deptno = mydept.deptno and e.sal > mydept.myavg;
    ```

  - 인라인뷰는  공통되는 열을 기준으로 합치려면 where절에 공통열이 동일하다는 꼭 써주자 !



- all로 max/min 찾기
- all로 max 찾기: 직업이 salesman인 사람의 월급 모두보다도 크다

```sql
SELECT ename, sal
FROM EMP
WHERE sal > all(
	SELECT sal
	FROM emp
	WHERE job = 'SALESMAN'
);
```

 - all로 min 찾기:

```sql
SELECT ename, sal
FROM EMP
WHERE sal < all(
	SELECT sal
	FROM emp
	WHERE job = 'SALESMAN'
);
```



- any는 하나라도라는 뜻이기 때문에
  - sal > any() => 최소값보다만 크면
  - sal < any () => 최대값보다만 작으면



## JOIN

- Oracle에서는 join 방법이 총 3개 :

		1. inner join & **on** 테이블A.컬럼 = 테이블b.컬럼 => ANSI 표준, __중복열 있음__
		1. inner join & **using**(컬럼) => ANSI 표준, __중복열 없음__
		1. from 테이블a, 테이블b where **테이블a.컬럼 = 테이블b.컬럼 ** => __중복열 있음__

- 3번은 mysql에 없음

- Oracle에서 join은 inner join



- Non-equi join : 일치하는 값이 없을 때 join하는 경우
  - `select * from emp e JOIN salgrade sg ON(e.sal BETWEEN sg.losal AND sg.hisal)`
  - emp와 salgrade는 공통 열이 없어서 합치는 기준을 between으로 준 것임

- Self join : 하나의 테이블이 두개의 테이블처럼 alias를 주어서 활용할 때

- Left outer join : 오른쪽 테이블에 해당 데이터가 없더라도 왼쪽 테이블에 있는 데이터는 다 넣고 싶을 때

  - :white_check_mark: Oracle에서는 left outer join 을 (+) 연산자를 활용해서도 표현가능!

    - 예) 

    - ```sql
      -- left outer join :
      select 사원.ename, 사원.empno, 관리자.ename, 관리자.empno
      from emp 사원, emp 관리자
      where 사원.mgr = 관리자.empno(+);
      ```

    - __(+)는 반대로 작성!__



- join이 여러개일 때 기준 컬럼잡기

1) on

```sql
select *
from emp e join salgrade sg on (e.sal between sg.losal and sg.hisal) join dept on (e.deptno = dept.deptno);
```

2. using + on

```sql
select *
from emp e join salgrade sg on (e.sal between sg.losal and sg.hisal) join dept using(deptno);
```

3. where 조건

```sql
select *
from dept d, emp e, salgrade sg
where d.deptno = e.deptno and e.sal between sg.losal and sg.hisal;
```



- join과 groupby를 함께 사용할때 :

  - 집계함수는 groupby에 있을 필요가 없으나, 집계함수가 아니라면 groupby내에 꼭 작성해주어야함

  ```sql
  -- 부서이름, 위치, 각 부서의 사원수, 평균 월급을 출력하자.
  -- 1. on
  select dname, loc, count(*), avg(sal)
  from emp join dept on (emp.deptno = dept.deptno)
  group by dname, loc;
  
  -- 2. using
  select dname, loc, count(*), avg(sal)
  from emp join dept using(deptno)
  group by dname, loc;
  
  -- 3. where
  SELECT d.dname, d.loc, count(*), avg(e.sal)
  FROM emp e, dept d
  WHERE e.deptno = d.deptno
  GROUP BY d.dname, d.loc;
  ```

  - 여기서 avg(sal)은 __그룹별__ avg(sal)이기 때문에 sal을 group by에 넣을 필요 없음!
  - count(*)대신 count(dname)해도 같은 결과!





## 기타:

- != null 불가능 : null은 연산이 불가능하기 때문에
- nvl(comm, 0) 은 습관적으로 작성하기
