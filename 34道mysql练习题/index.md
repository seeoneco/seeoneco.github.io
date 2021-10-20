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
第二步使用max，不能直接在上面套一个`max`,`max(avg(e.sal))`写法是错误的，
```sql
select max(t.avgsal) from t;
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
> 1. having字句可以让我们**筛选**成组后的各种数据，where字句在聚合前先筛选记录，也就是说作用在group by和having字句前。而 having子句在聚合后对组记录进行筛选。
> 2. HAVING 只能与 SELECT 语句一起使用。
>    HAVING 通常在 GROUP BY 子句中使用。
>    如果不使用 GROUP BY 子句，则 HAVING 的行为与 WHERE 子句一样。
>
> 3. https://bbs.csdn.net/topics/390162668

+ 使用max的第二种思路，与`dept`表连接（这是视频里一个同学的写法，但是看弹幕里mysql8.0以后好像会报错）

```sql
select 
	d.dname,d.deptno
from
	(select e.deptno,avg(e.sal) avgsal from emp e group by deptno)t
join
	dept d
on
	t.deptno=d.deptno having max(avgsal);
```

### 6、取得平均薪水最高的部门的部门名称 

+ 按`deptno`分组，排序，`limit1`只选最大值。再与`dept`表按`t.deptno = d.deptno`连接
```sql
---------------第一种写法--------------
select 
t.*,d.dname
from 
	(select deptno,avg(sal) as avgsal
	 from emp
	 group by deptno 
	 order by avgsal desc 
	 limit 1) t
join 
   dept d
on 
   t.deptno = d.deptno;
  
+--------+-------------+------------+
| deptno | avgsal      | dname      |
+--------+-------------+------------+
|     10 | 2916.666667 | ACCOUNTING |
+--------+-------------+------------+

---------------第二种写法--------------
select 
	d.dname,avg(e.sal) as avgsal
from
	emp e
join
	dept d
on 
e.deptno = d.deptno
group by 
	d.dname
order by
	avgsal desc
limit 1;

+------------+-------------+
| dname      | avgsal      |
+------------+-------------+
| ACCOUNTING | 2916.666667 |
+------------+-------------+
```

这里的第二种写法通过d表的dname进行分组聚合，那么在select中就可以直接使用d.dname字段了。算是一种讨巧的方法。

### 7、求平均薪水的等级最低的部门的部门名称 

我的思路：先求部门的平均薪水等级，（正好前面第三题与这个类似）再连表升序选第一个搞定。

```sql
select d.dname
from
	(select e.deptno as deptno,avg(s.grade) as avgsal
	 from emp e
     join salgrade s
     on e.sal between s.losal and s.hisal
     group by deptno)t
join dept d
on d.deptno= t.deptno
order by t.avgsal asc 
limit 1;

+-------+
| dname |
+-------+
| SALES |
+-------+
```

> 我们上面的思路实际上是求**平均的薪水等级**按照grade求平均数。
>
> 而题目让求的是**平均薪水的等级**使用sal求平均数。
>
> 但是这样看似得到了答案，但是却不严谨。我们来思考两个问题。
>
> 1. 平均薪水最低，那么薪水等级一定是最低。
>
> 2. 平均薪水不是最低，但也有可能是等级最低。  
>
> 打个比方，薪资在1000-1500,的属于薪资等级1，1500-2000的属于薪资等级2。那么有三个员工，工资分别是1100,1300,1800。毫无疑问，工资最低的1100,薪水等级也是最低的1。而工资不是最低的1300，薪资等级却也是1。  
>
> 因此上面的sql语句是错误的，而且不够严谨。正确思路如下，

思路：
第一步：找出每个部门的平均薪水(按照部门分组求平均值)

```sql
select deptno,avg(sal) as avgsal from emp group by deptno;

+--------+-------------+
| deptno | avgsal      |
+--------+-------------+
|     10 | 2916.666667 |
|     20 | 2175.000000 |
|     30 | 1566.666667 |
+--------+-------------+
```

第二步：找出每个部门的平均薪水等级。以上t表和salgrade表连接，条件是`t.avgsal between s.losal and s.hisal`，查出此表之后，我们还缺一个该如何查出最低的等级。

```sql
select t.*,s.grade
from (select deptno,avg(sal) as avgsal from emp group by deptno ) t
join salgrade s
on t.avgsal between s.losal and s.hisal;

+--------+-------------+-------+
| deptno | avgsal      | grade |
+--------+-------------+-------+
|     10 | 2916.666667 |     4 |
|     20 | 2175.000000 |     4 |
|     30 | 1566.666667 |     3 |
+--------+-------------+-------+
```

我们可以想到，**求最低等级 ==> 平均薪水最低的等级一定是最低的**，因此我们做的是**通过最低的平均薪水来查出最低的薪水等级**。这样做很严谨，因为等级是一个区间，就算有两个相同的最低等级也能查的出来，而不是使用limit1只能查出来一个。
1. 步骤一：查到最低的平均薪水。
```sql
select avg(sal) as avgsal from emp group by deptno order by avgsal asc limit 1;
+-------------+
| avgsal      |
+-------------+
| 1566.666667 |
+-------------+
```

2. 步骤二： 将上表加入salgrade，通过最低的平均薪水查到最低的薪水等级。 条件是 avgsal between losal and hisal
```sql
select grade from salgrade s where (1566.666667) between s.losal and s.hisal
+-------+
| grade |
+-------+
|     3 |
+-------+
```
  我们把数值替换成sql语句
```sql
select grade from salgrade s where (select avg(sal) as avgsal from emp group by deptno order by avgsal asc limit 1) between s.losal and s.hisal;

+-------+
| grade |
+-------+
|     3 |
+-------+
```
3. 步骤三：将上面大致思路的第二步中再加入一个where条件即加入上面这张表

```sql
select t.*,s.grade
from (select deptno,avg(sal) as avgsal from emp group by deptno ) t
join salgrade s
on t.avgsal between s.losal and s.hisal
where s.grade=(select grade from salgrade s where (select avg(sal) as avgsal from emp group by deptno order by avgsal asc limit 1) between s.losal and s.hisal);

+--------+-------------+-------+
| deptno | avgsal      | grade |
+--------+-------------+-------+
|     30 | 1566.666667 |     3 |
+--------+-------------+-------+
```

至此，我们基本已经完成了。但是想要查询的字段不是部门编号，而是名称。只需要在这行中连接一下dept表。`from (select deptno,avg(sal) as avgsal from emp group by deptno ) t`

```sql
select t.*,s.grade
from (select d.dname,avg(e.sal) as avgsal from emp e join dept d on e.deptno=d.deptno group by d.dname ) t
join salgrade s
on t.avgsal between s.losal and s.hisal
where s.grade=(select grade from salgrade s where (select avg(sal) as avgsal from emp group by deptno order by avgsal asc limit 1) between s.losal and s.hisal);

+-------+-------------+-------+
| dname | avgsal      | grade |
+-------+-------------+-------+
| SALES | 1566.666667 |     3 |
+-------+-------------+-------+
```

### 8、取得比普通员工(员工代码没有在mgr字段上出现的)的最高薪水还要高的领导人姓名 

```sql
select distinct mgr from emp;
+------+
| mgr  |
+------+
| 7902 |
| 7698 |
| 7839 |
| 7566 |
| NULL |
| 7788 |
| 7782 |
+------+
```

员工编号没有在以上范围内的都是普通员工。

思路：

步骤一： 找出普通员工的最高薪水。
```sql
select max(sal) from emp where 普通员工
```
```sql
select max(sal) from emp where empno not in (select distinct mgr from emp)
```
会发现查出来的是NULL。Tips: **not in在使用的时候，后面的小括号记得排除NULL。**
```sql
select max(sal) from emp where empno not in (select distinct mgr from emp where mgr is not null)
+----------+
| max(sal) |
+----------+
|  1600.00 |
+----------+
```

步骤二：找出高于1600的

```sql
select e.ename,e.sal 
from emp e
where e.sal>(select max(sal) from emp where empno not in (select distinct mgr from emp where mgr is not null));

+-------+---------+
| ename | sal     |
+-------+---------+
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| CLARK | 2450.00 |
| SCOTT | 3000.00 |
| KING  | 5000.00 |
| FORD  | 3000.00 |
+-------+---------+
```

### 9、取得薪水最高的前五名员工 

```sql
select ename,sal
from emp
order by sal desc 
limit 5;

+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| FORD  | 3000.00 |
| SCOTT | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+
```

### 10、取得薪水最高的第六到第十名员工 

```sql
select ename,sal
from emp
order by sal desc
limit 5,5;

+--------+---------+
| ename  | sal     |
+--------+---------+
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
| WARD   | 1250.00 |
+--------+---------+
```

### 11、取得最后入职的5名员工 

日期降序排列。。。。

```sql
select ename,hiredate from emp order by hiredate desc limit 5;

+--------+------------+
| ename  | hiredate   |
+--------+------------+
| ADAMS  | 1987-05-23 |
| SCOTT  | 1987-04-19 |
| MILLER | 1982-01-23 |
| JAMES  | 1981-12-03 |
| FORD   | 1981-12-03 |
+--------+------------+
```

### 12、取得每个薪水等级有多少员工 

第一步：找出每个员工的薪水等级

```sql
select e.ename,e.sal,s.grade 
from emp e 
join salgrade s 
on e.sal 
between s.losal and s.hisal;

+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SMITH  |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+
```

第二步：继续分组统计数量

```sql
select s.grade,count(*)
from emp e 
join salgrade s 
on e.sal 
between s.losal and s.hisal
group by s.grade;

+-------+----------+
| grade | count(*) |
+-------+----------+
|     1 |        3 |
|     2 |        3 |
|     3 |        2 |
|     4 |        5 |
|     5 |        1 |
+-------+----------+
```

> 小Tips: 如果存在分组函数，比如这里`group by s.grade`那么在select 中只能写分组的字段和分组函数。


