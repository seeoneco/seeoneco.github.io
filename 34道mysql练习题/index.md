# 34道MySql练习题



## 34道MySql练习题
### 前言
在实习的时候发现基础部分就是写sql语句，大部分都是增删改查。但是水平实在是一言难尽，故此重新复习一遍。
### SQL文件及描述
```sql
DROP TABLE IF EXISTS EMP;
DROP TABLE IF EXISTS DEPT;
DROP TABLE IF EXISTS SALGRADE;
 
CREATE TABLE DEPT
       (DEPTNO int(2) not null ,
    DNAME VARCHAR(14) ,
    LOC VARCHAR(13),
    primary key (DEPTNO)
    );
CREATE TABLE EMP
       (EMPNO int(4)  not null ,
    ENAME VARCHAR(10),
    JOB VARCHAR(9),
    MGR INT(4),
    HIREDATE DATE  DEFAULT NULL,
    SAL DOUBLE(7,2),
    COMM DOUBLE(7,2),
    primary key (EMPNO),
    DEPTNO INT(2) 
    )
    ;
 
CREATE TABLE SALGRADE
      ( GRADE INT,
    LOSAL INT,
    HISAL INT );
 
INSERT INTO DEPT ( DEPTNO, DNAME, LOC ) VALUES ( 
10, 'ACCOUNTING', 'NEW YORK'); 
INSERT INTO DEPT ( DEPTNO, DNAME, LOC ) VALUES ( 
20, 'RESEARCH', 'DALLAS'); 
INSERT INTO DEPT ( DEPTNO, DNAME, LOC ) VALUES ( 
30, 'SALES', 'CHICAGO'); 
INSERT INTO DEPT ( DEPTNO, DNAME, LOC ) VALUES ( 
40, 'OPERATIONS', 'BOSTON'); 
commit;
  
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7369, 'SMITH', 'CLERK', 7902,  '1980-12-17'
, 800, NULL, 20); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7499, 'ALLEN', 'SALESMAN', 7698,  '1981-02-20'
, 1600, 300, 30); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7521, 'WARD', 'SALESMAN', 7698,  '1981-02-22'
, 1250, 500, 30); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7566, 'JONES', 'MANAGER', 7839,  '1981-04-02'
, 2975, NULL, 20); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7654, 'MARTIN', 'SALESMAN', 7698,  '1981-09-28'
, 1250, 1400, 30); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7698, 'BLAKE', 'MANAGER', 7839,  '1981-05-01'
, 2850, NULL, 30); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7782, 'CLARK', 'MANAGER', 7839,  '1981-06-09'
, 2450, NULL, 10); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7788, 'SCOTT', 'ANALYST', 7566,  '1987-04-19'
, 3000, NULL, 20); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7839, 'KING', 'PRESIDENT', NULL,  '1981-11-17'
, 5000, NULL, 10); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7844, 'TURNER', 'SALESMAN', 7698,  '1981-09-08'
, 1500, 0, 30); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7876, 'ADAMS', 'CLERK', 7788,  '1987-05-23'
, 1100, NULL, 20); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7900, 'JAMES', 'CLERK', 7698,  '1981-12-03'
, 950, NULL, 30); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7902, 'FORD', 'ANALYST', 7566,  '1981-12-03'
, 3000, NULL, 20); 
INSERT INTO EMP ( EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM,
DEPTNO ) VALUES ( 
7934, 'MILLER', 'CLERK', 7782,  '1982-01-23'
, 1300, NULL, 10); 
commit;
  
INSERT INTO SALGRADE ( GRADE, LOSAL, HISAL ) VALUES ( 
1, 700, 1200); 
INSERT INTO SALGRADE ( GRADE, LOSAL, HISAL ) VALUES ( 
2, 1201, 1400); 
INSERT INTO SALGRADE ( GRADE, LOSAL, HISAL ) VALUES ( 
3, 1401, 2000); 
INSERT INTO SALGRADE ( GRADE, LOSAL, HISAL ) VALUES ( 
4, 2001, 3000); 
INSERT INTO SALGRADE ( GRADE, LOSAL, HISAL ) VALUES ( 
5, 3001, 9999); 
commit;
```
### 1、取得每个部门最高薪水的人员名称
思路：

1. 取得每个部门最高薪水（按部门编号分组，找出每一组最大值）

```sql
select deptno,max(sal) as maxsal from emp group by deptno;

+--------+---------+
| deptno | maxsal  |
+--------+---------+
|     10 | 5000.00 |
|     20 | 3000.00 |
|     30 | 2850.00 |
+--------+---------+
```
2. 将以上的表作为一张临时表`t`，`t`和`emp`表连接，条件是`t.deptno=e.deptno and t.maxsal=e.sal`

完整SQL：
```sql
select
    e.ename,t.*
  from
    emp e
  join
    (select deptno,max(sal) as maxsal from emp group by deptno)t
  on 
    t.deptno=e.deptno and t.maxsal=e.sal;


+-------+--------+---------+
| ename | deptno | maxsal  |
+-------+--------+---------+
| BLAKE |     30 | 2850.00 |
| SCOTT |     20 | 3000.00 |
| KING  |     10 | 5000.00 |
| FORD  |     20 | 3000.00 |
+-------+--------+---------+
```

### 2、哪些人的薪水在平均薪水之上

思路：

1. 求出每个部门的平均薪水
```sql
select deptno,AVG(sal)as avgsal from emp group by deptno;

+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
|     20 | 2175.000000 |
|     30 | 1566.666667 |
+--------+-------------+
```
2. 将上面的查询结果当做`t`表，`t`和`emp`表连接，条件部门编号相同，`t.avgsal < e.SAL`

完整SQL：
```sql
SELECT
	e.ename,
	e.SAL 
FROM
	emp e
	JOIN ( SELECT deptno, AVG( sal ) AS avgsal FROM emp GROUP BY deptno ) t 
ON t.avgsal < e.SAL AND t.deptno = e.deptno;


+-------+---------+
| ename | SAL     |
+-------+---------+
| ALLEN | 1600.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| SCOTT | 3000.00 |
| KING  | 5000.00 |
| FORD  | 3000.00 |
+-------+---------+
```
### 3、取得部门中（所有人的）平均的薪水等级 

区别：

+ 平均的薪水等级：先计算每个薪水的等级，然后找出薪水等级的平均值。
+ 平均薪水的等级：先计算平均薪水，再找出每个平均薪水的等级。

思路：

1. 根据部门找出每个人的薪水等级

   `epm e`和`salgrade s`表连接
   
   连接条件：`e.sal between s.losal and s.hisal` 
   
   我们可以在后面加上 `order by e.deptno`排序一下

```sql
select 
	e.ename,e.sal,e.deptno,s.grade as sgrade
from 
	emp e
join 
	salgrade s 
on e.sal between s.losal and s.hisal;

+--------+---------+--------+--------+
| ename  | sal     | deptno | sgrade |
+--------+---------+--------+--------+

| MILLER | 1300.00 |     10 |      2 |
| CLARK  | 2450.00 |     10 |      4 |
| KING   | 5000.00 |     10 |      5 |

| SCOTT  | 3000.00 |     20 |      4 |
| SMITH  |  800.00 |     20 |      1 |
| ADAMS  | 1100.00 |     20 |      1 |
| JONES  | 2975.00 |     20 |      4 |
| FORD   | 3000.00 |     20 |      4 |

| BLAKE  | 2850.00 |     30 |      4 |
| ALLEN  | 1600.00 |     30 |      3 |
| TURNER | 1500.00 |     30 |      3 |
| WARD   | 1250.00 |     30 |      2 |
| JAMES  |  950.00 |     30 |      1 |
| MARTIN | 1250.00 |     30 |      2 |
+--------+---------+--------+--------+
```
我们需要求的平均的薪水等级，就把每个相同部门的人的等级相加，然后就平均值即可。
2. 基于上面的表按照deptno分组，

```sql
select 
	e.deptno,avg(s.grade) as avggrade
from
	emp e
join 
	salgrade s 
on e.sal between s.losal and s.hisal
group by 
	e.deptno;
	
+--------+----------+
| deptno | avggrade |
+--------+----------+
|     10 |   3.6667 |
|     20 |   2.8000 |
|     30 |   2.5000 |
+--------+----------+
```

我们会发现，上面的那个表并不需要成为一张临时表，我们仅仅通过`deptno`把上面的表做一次分组就好了。但是分组之后出现了一个问题，如果在`select`语句中仍然存在`e.ename`,那么会报错，这是什么原因呢。使用`group by `分组聚合，那么就把原先的相同的部门聚合成了一组，这个时候，他们相同的字段只有部门编号，而名字在聚合中是不同的，我们提取不了不同的值。

### 4、不准用组函数（Max），取得最高薪水（给出两种解决方案）

+ 第一种：使用`order by sal`,通过`sal`进行排序，`desc`降序，再使用`limit`分页，取第一条数据即可。【升序(`ASC`)或降序(`DESC`)】
```sql
select ename,sal from emp order by sal desc limit 1;

+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
+-------+---------+
```
+  第二种：（不讲规矩）直接使用max，最简单的方法。
```sql
select max(sal) from emp;
```
+ 第三种：表的自连接+去重+not in
  1. 把同一个emp表做两份，进行自连接，判断条件a表的sal与b表的sal的大小比较。
  2. 去重，通过比较，留不下的是5000，因为5000最大，条件不成立。
  3. 把上面的表当做一个临时表，使用where进行条件判断，查出不在临时表中数据。

```sql
select sal from emp where sal not in (select distinct a.sal from emp a join emp b on a.sal < b.sal);

+---------+
| sal     |
+---------+
| 5000.00 |
+---------+
```

### 5、取得平均薪水最高的部门的部门编号（至少给出两种解决方案）
+ 第一种：使用排序，limit1

第一步： 找出每个部门的平均薪水
```sql
select 
	e.deptno,avg(e.sal)
from 
	emp e 
group by deptno;

+--------+-------------+
| deptno | avg(e.sal)  |
+--------+-------------+
|     10 | 2916.666667 |
|     20 | 2175.000000 |
|     30 | 1566.666667 |
+--------+-------------+
```

第二步：根据平均薪水进行降序排序，取第一个数值

```sql
select 
	e.deptno,avg(e.sal) as avgsal
from 
	emp e 
group by deptno
order by avgsal desc limit 1;

+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
```

+ 第二种：使用max

第一步同上，
```sql
select e.deptno,avg(e.sal) as avgsal from emp e group by deptno;
```
第二步使用max，不能直接在上面套一个`max`,`max(avg(e.sal))`写法书错误的，
```sql
select max(t.avesal) from t;
```
拿到最大值：
```sql
select max(t.avgsal) from (select e.deptno,avg(e.sal) as avgsal from emp e group by deptno)t

+---------------+
| max(t.avgsal) |
+---------------+
|   2916.666667 |
+---------------+
```
第三步使用having与第一步的表进行连接
```sql
select 
	e.deptno,avg(e.sal) as avgsal 
from emp e 
group by 
	deptno 
having avgsal=(select max(t.avgsal) from (select e.deptno,avg(e.sal) as avgsal from emp e group by deptno)t);

+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
+--------+-------------+
```
附上自己错误的用法：  
![](https://gitee.com/lonercci/picbed/raw/master/img/202110192218757.png)


> **Tips: Having的用法**：
>
> having字句可以让我们筛选成组后的各种数据，where字句在聚合前先筛选记录，也就是说作用在group by和having字句前。而 having子句在聚合后对组记录进行筛选。


