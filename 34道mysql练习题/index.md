# 34道MySql练习题



## 34道MySql练习题
### 前言
在web中最基础的部分就是写sql语句，大部分都是增删改查。但是水平实在是一言难尽，故此重新复习一遍。

### 难度星级之攻略

都是自己亲身体验过的，随便打几颗星星玩（可能不太准确，笑）。有些知识点不熟悉所有显得有点难。还有一些题目不算难但是过程很绕或者是表述很绕，为了出题而出题，题目看懂了就还好。

PS：第13题是面试题，有单独的SQL文件。与其余的33道题的sql没有关联。

|     难度系数     | 题目序号                            |
| :--------------: | ----------------------------------- |
| ★☆☆☆（有手就行） | 18、21、23、27、30                  |
| ★★☆☆（基础部分） | 1、2、9、10、11、17、23、25、29     |
| ★★★☆（还可以吧） | 3、4、5、6、8、12、14、15、16、25、 |
| ★★★★（阅读理解） | 7、13、19、20、22、24、26、28、33   |

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

日期降序排列。。。。从最近的日期到最远的日期。

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

### 13、面试题

#### SQL 文件

```sql
CREATE TABLE SC
(
SNO VARCHAR (200),
CNO VARCHAR(200),
SCGRADE VARCHAR (200)
);

CREATE TABLE S
(
SNO VARCHAR(200),
SNAME VARCHAR (200)
);

CREATE TABLE C
(
CNO VARCHAR (200),
CNAME VARCHAR (200),
CTEACHER VARCHAR (200) 
);

INSERT INTO C ( CNO, CNAME, CTEACHER ) VALUES ( '1','语文','张');
INSERT INTO C ( CNO, CNAME, CTEACHER ) VALUES ( '2','政治','王');
INSERT INTO C ( CNO, CNAME, CTEACHER ) VALUES ( '3','英语','李');
INSERT INTO C ( CNO, CNAME, CTEACHER ) VALUES ( '4','数学','赵');
INSERT INTO C ( CNO, CNAME, CTEACHER ) VALUES ( '5','物理','黎明');
commit;

INSERT INTO S ( SNO, SNAME ) VALUES ('1','学生1');
INSERT INTO S ( SNO, SNAME ) VALUES ('2','学生2');
INSERT INTO S ( SNO, SNAME ) VaLUES ('3','学生3');
INSERT INTO S ( SNO, SNAME ) VALUES ('4','学生4');
commit;

INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '1','1','40');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '1','2','30');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '1','3','20');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '1','4','80');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '1','5','60');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '2','1','60');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '2','2','60');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '2','3','60');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '2','4','60');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '2','5','40');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '3','1','60');
INSERT INTO SC ( SNO, CNO, SCGRADE ) VALUES ( '3','3','80');
commit ;
```

#### 描述

有3个表S(学生表)，C（课程表），SC（学生选课表）

S（SNO，SNAME）代表（学号，姓名）  

C（CNO，CNAME，CTEACHER）代表（课号，课名，教师）

SC（SNO，CNO，SCGRADE）代表（学号，课号，成绩） 

#### 问题1：找出没选过“黎明”老师的所有学生姓名。

```sql
select sname
from S
where sname not in (select s.sname
from SC sc
join C c
on sc.cno = c.cno
join S s
on sc.sno = s.sno
where c.cteacher='黎明');

+-------+
| sname |
+-------+
| 学生3  |
| 学生4  |
+-------+
```

#### 问题2：列出2门以上（含2门）不及格学生姓名及平均成绩。

```sql
-- 查询2门及以上的未及格的学生
select sc.sno,count(sc.sno) as count
from SC sc
join S s
on sc.sno = s.sno
where sc.scgrade<60
group by sc.sno
having count>=2;
+------+-------+
| sno  | count |
+------+-------+
| 1    |     3 |
+------+-------+

-- 与S表,SC表连接，条件t.sno = s.sno 
select s.sname,avg(sc.scgrade)
from S s
join SC sc
on s.sno = sc.sno
join ()t
on t.sno = s.sno
group by s.sname;

-- 填入
select s.sname,avg(sc.scgrade)
from S s
join SC sc
on s.sno = sc.sno
join ( select sc.sno,count(sc.sno) as count
	   from SC sc
       join S s
       on sc.sno = s.sno
       where sc.scgrade<60
       group by sc.sno
       having count>=2)t
on t.sno = s.sno
group by s.sname;

+-------+-----------------+
| sname | avg(sc.scgrade) |
+-------+-----------------+
| 学生1 |              46 |
+-------+-----------------+
```

#### 问题3：即学过1号课程又学过2号课所有学生的姓名。

```sql
-- 同时学过1,2号课程的
select sno from sc where cno = 1 and sno in(select sno from sc where cno = 2);
+------+
| sno  |
+------+
| 1    |
| 2    |
+------+

-- 与sc表连接 t.sno = s.sno
select s.sname
from S s
join (select sno from sc where cno = 1 and sno in(select sno from sc where cno = 2))t
on t.sno = s.sno;

+-------+
| sname |
+-------+
| 学生1 |
| 学生2 |
+-------+
```

### 14、列出所有员工及其领导的姓名

```sql
select a.ename as '员工',b.ename as '领导'
from emp a
left join emp b
on a.mgr = b.empno;

+--------+-------+
| 员工   | 领导  |
+--------+-------+
| SMITH  | FORD  |
| ALLEN  | BLAKE |
| WARD   | BLAKE |
| JONES  | KING  |
| MARTIN | BLAKE |
| BLAKE  | KING  |
| CLARK  | KING  |
| SCOTT  | JONES |
| KING   | NULL  |
| TURNER | BLAKE |
| ADAMS  | SCOTT |
| JAMES  | BLAKE |
| FORD   | JONES |
| MILLER | CLARK |
+--------+-------+
```

> 小Tips: 
>
> left join（左联接）：返回左表中的所有记录以及和右表中的联接字段相等的记录。
>
> right join（右联接）：返回右表中的所有记录以及和左表中的联接字段相等的记录。
>
> inner join（等值联接，join）：只返回两个表中联接字段相等的记录。
>
> 相关文章：https://segmentfault.com/a/1190000017369618



### 15、列出受雇日期早于其直接上级的所有员工的编号,姓名,部门名称 

思路：

1. `emp a` 员工表
2. `emp b` 领导表
3. 受雇日期早于领导 ==>  `a.mgr = b.empno and a.hiredate<b.hiredate`

```sql
select a.empno as empno,a.ename as ename,d.dname
from emp a
join dept d
on a.deptno = d.deptno
join emp b 
on a.mgr = b.empno 
where a.hiredate<b.hiredate;
```

> 小Tips：多表连接，多次使用join连接

### 16、列出部门名称和这些部门的员工信息,同时列出那些没有员工的部门 

```sql
select d.dname,e.*
from emp e
right join dept d
on e.deptno = d.deptno
order by e.deptno;
+------------+-------+--------+-----------+------+------------+---------+---------+--------+
| dname      | EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+------------+-------+--------+-----------+------+------------+---------+---------+--------+
| OPERATIONS |  NULL | NULL   | NULL      | NULL | NULL       |    NULL |    NULL |   NULL |
| ACCOUNTING |  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
| ACCOUNTING |  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
| ACCOUNTING |  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
| RESEARCH   |  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
| RESEARCH   |  7369 | SMITH  | CLERK     | 7902 | 1980-12-17 |  800.00 |    NULL |     20 |
| RESEARCH   |  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
| RESEARCH   |  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
| RESEARCH   |  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
| SALES      |  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
| SALES      |  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
| SALES      |  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
| SALES      |  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
| SALES      |  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
| SALES      |  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
+------------+-------+--------+-----------+------+------------+---------+---------+--------+
```

> 小Tips：使用右外连接，`right join`

### 17、列出至少有5个员工的所有部门 

```sql
select d.dname,count(*)
from emp e
join dept d
on e.deptno = d.deptno
group by d.dname
having count(*)>=5;

+----------+----------+
| dname    | count(*) |
+----------+----------+
| RESEARCH |        5 |
| SALES    |        6 |
+----------+----------+

```

### 18、列出薪金比"SMITH"多的所有员工信息 

```sql
select e.*
from emp e
where e.sal>(select sal from emp where ename="SMITH");

+-------+--------+-----------+------+------------+---------+---------+--------+
| EMPNO | ENAME  | JOB       | MGR  | HIREDATE   | SAL     | COMM    | DEPTNO |
+-------+--------+-----------+------+------------+---------+---------+--------+
|  7499 | ALLEN  | SALESMAN  | 7698 | 1981-02-20 | 1600.00 |  300.00 |     30 |
|  7521 | WARD   | SALESMAN  | 7698 | 1981-02-22 | 1250.00 |  500.00 |     30 |
|  7566 | JONES  | MANAGER   | 7839 | 1981-04-02 | 2975.00 |    NULL |     20 |
|  7654 | MARTIN | SALESMAN  | 7698 | 1981-09-28 | 1250.00 | 1400.00 |     30 |
|  7698 | BLAKE  | MANAGER   | 7839 | 1981-05-01 | 2850.00 |    NULL |     30 |
|  7782 | CLARK  | MANAGER   | 7839 | 1981-06-09 | 2450.00 |    NULL |     10 |
|  7788 | SCOTT  | ANALYST   | 7566 | 1987-04-19 | 3000.00 |    NULL |     20 |
|  7839 | KING   | PRESIDENT | NULL | 1981-11-17 | 5000.00 |    NULL |     10 |
|  7844 | TURNER | SALESMAN  | 7698 | 1981-09-08 | 1500.00 |    0.00 |     30 |
|  7876 | ADAMS  | CLERK     | 7788 | 1987-05-23 | 1100.00 |    NULL |     20 |
|  7900 | JAMES  | CLERK     | 7698 | 1981-12-03 |  950.00 |    NULL |     30 |
|  7902 | FORD   | ANALYST   | 7566 | 1981-12-03 | 3000.00 |    NULL |     20 |
|  7934 | MILLER | CLERK     | 7782 | 1982-01-23 | 1300.00 |    NULL |     10 |
+-------+--------+-----------+------+------------+---------+---------+--------+
```

### 19、列出所有"CLERK"(办事员)的姓名及其部门名称,部门的人数

```sql
select e.ename,d.dname
from emp e
join dept d
on e.deptno = d.deptno
where e.job="CLERK";

+--------+------------+
| ename  | dname      |
+--------+------------+
| SMITH  | RESEARCH   |
| ADAMS  | RESEARCH   |
| JAMES  | SALES      |
| MILLER | ACCOUNTING |
+--------+------------+
```

到这一步很容易，那么后面的问题来了，怎么把部门的人数连上？  

如果直接在使用group by d.dname分组，那么select中就无法使用e.name字段，还是查不出来。因此还是老老实实想办法用临时表吧。

在上表中再加一个字段，d.deptno。这样在对emp表通过deptno做分组得出数量的同时有了deptno字段。既然有了相同的字段，那么就可以愉快的使用连表进行join了。

```sql
-- 第一步：
select e.ename,d.dname,e.deptno as deptno
from emp e
join dept d
on e.deptno = d.deptno
where e.job="CLERK";
+--------+------------+--------+
| ename  | dname      | deptno |
+--------+------------+--------+
| SMITH  | RESEARCH   |     20 |
| ADAMS  | RESEARCH   |     20 |
| JAMES  | SALES      |     30 |
| MILLER | ACCOUNTING |     10 |
+--------+------------+--------+
-- 第二步： 对emp单独做分组，查询数量
select deptno,count(*) as deptcount from emp group by deptno;
+--------+-----------+
| deptno | deptcount |
+--------+-----------+
|     10 |         3 |
|     20 |         5 |
|     30 |         6 |
+--------+-----------+

-- 第三步：好了，现在直接通过deptno 进行连表操作。
select t.*,tt.deptcount
from ()t
join ()tt
on t.deptno = tt.deptno;

-- 第四步：填入上面的两个语句。
select t.*,tt.deptcount
from (select e.ename,d.dname,e.deptno as deptno
from emp e
join dept d
on e.deptno = d.deptno
where e.job="CLERK")t
join (select deptno,count(*) as deptcount from emp group by deptno)tt
on t.deptno = tt.deptno;

+--------+------------+--------+-----------+
| ename  | dname      | deptno | deptcount |
+--------+------------+--------+-----------+
| SMITH  | RESEARCH   |     20 |         5 |
| ADAMS  | RESEARCH   |     20 |         5 |
| JAMES  | SALES      |     30 |         6 |
| MILLER | ACCOUNTING |     10 |         3 |
+--------+------------+--------+-----------+
```

### 20、列出最低薪金大于1500的各种工作及从事此工作的全部雇员人数 

```sql
select e.job,count(*) from emp e group by e.job having min(sal)>1500;
+-----------+----------+
| job       | count(*) |
+-----------+----------+
| ANALYST   |        2 |
| MANAGER   |        3 |
| PRESIDENT |        1 |
+-----------+----------+
```

### 21、列出在部门"SALES"<销售部>工作的员工的姓名,假定不知道销售部的部门编号 

。。。。想复杂了，以为是没有deptno字段，那样就没法做到连表了。

```sql
select ename from emp where deptno = (select deptno from dept where dname = "SALES");

+--------+
| ename  |
+--------+
| ALLEN  |
| WARD   |
| MARTIN |
| BLAKE  |
| TURNER |
| JAMES  |
+--------+
```

### 22、列出薪金高于公司平均薪金的所有员工,所在部门,上级领导,雇员的工资等级

```sql
select e.ename as '员工',d.dname as '部门', l.ename as '领导',s.grade as '等级'
from emp e
join dept d
on e.deptno = d.deptno
left join emp l
on e.mgr = l.empno
join salgrade s
on e.sal between s.losal and s.hisal
where e.sal>(select avg(sal) from emp);

+-------+------------+-------+-------+
| 员工   |  部门      | 领导   | grade |
+-------+------------+-------+-------+
| JONES | RESEARCH   | KING  |     4 |
| BLAKE | SALES      | KING  |     4 |
| CLARK | ACCOUNTING | KING  |     4 |
| SCOTT | RESEARCH   | JONES |     4 |
| KING  | ACCOUNTING | NULL  |     5 |
| FORD  | RESEARCH   | JONES |     4 |
+-------+------------+-------+-------+
```

### 23、列出与"SCOTT"从事相同工作的所有员工及部门名称

```sql
select e.job,e.ename,d.dname
from emp e
join dept d
on e.deptno = d.deptno
where e.job = (select job from emp where ename="SCOTT")
and e.ename <> 'SCOTT';

+---------+-------+----------+
| job     | ename | dname    |
+---------+-------+----------+
| ANALYST | FORD  | RESEARCH |
+---------+-------+----------+
```

> 小Tips：要排除SCOTT自己，所以需要加上一个and条件

### 24、列出薪金等于部门30中员工的薪金的其他员工的姓名和薪金 

```sql
-- 去重求出部门为30的所有薪金
select distinct sal from emp where deptno = '30';

-- 筛选薪金是上表中的人
select ename,sal from emp where sal in ()

-- 放进去,且不是这个部门的人
select ename,sal from emp where sal in (select distinct sal from emp where deptno = '30') and deptno <> '30';

空白表！
Empty set (0.00 sec)
```

### 25、列出薪金高于在部门30工作的所有员工的薪金的员工姓名和薪金.部门名称

```sql
-- 求出部门30员工的最高薪资
select max(sal) from emp group by deptno having deptno="30";

+----------+
| max(sal) |
+----------+
|  2850.00 |
+----------+

-- 大于最高薪资就好了
select e.ename,e.sal,d.dname
from emp e
join dept d
on d.deptno = e.deptno
where d.deptno <>'30' and e.sal>(select max(sal) from emp group by deptno having deptno="30");

+-------+---------+------------+
| ename | sal     | dname      |
+-------+---------+------------+
| JONES | 2975.00 | RESEARCH   |
| SCOTT | 3000.00 | RESEARCH   |
| KING  | 5000.00 | ACCOUNTING |
| FORD  | 3000.00 | RESEARCH   |
+-------+---------+------------+
```

> 小Tips: 不等于可以写成 `<>`

### 26、列出在每个部门工作的员工数量,平均工资和平均服务期限

```sql
select d.*,count(e.ename),avg(e.sal)
from emp e
right join dept d
on e.deptno = d.deptno
group by d.deptno,d.dname,d.loc;

+--------+------------+----------+----------------+-------------+
| DEPTNO | DNAME      | LOC      | count(e.ename) | avg(e.sal)  |
+--------+------------+----------+----------------+-------------+
|     10 | ACCOUNTING | NEW YORK |              3 | 2916.666667 |
|     20 | RESEARCH   | DALLAS   |              5 | 2175.000000 |
|     30 | SALES      | CHICAGO  |              6 | 1566.666667 |
|     40 | OPERATIONS | BOSTON   |              0 |        NULL |
+--------+------------+----------+----------------+-------------+

-- 为了不输出NULL,改一下
select d.*,count(e.ename),ifnull(avg(e.sal),0)
from emp e
right join dept d
on e.deptno = d.deptno
group by d.deptno,d.dname,d.loc;
+--------+------------+----------+----------------+----------------------+
| DEPTNO | DNAME      | LOC      | count(e.ename) | ifnull(avg(e.sal),0) |
+--------+------------+----------+----------------+----------------------+
|     10 | ACCOUNTING | NEW YORK |              3 |          2916.666667 |
|     20 | RESEARCH   | DALLAS   |              5 |          2175.000000 |
|     30 | SALES      | CHICAGO  |              6 |          1566.666667 |
|     40 | OPERATIONS | BOSTON   |              0 |             0.000000 |
+--------+------------+----------+----------------+----------------------+

-- 服务期限
select d.*,count(e.ename),ifnull(avg(e.sal),0),avg(timestampdiff(YEAR,hiredate,now()) ) as avgyear
from emp e
right join dept d
on e.deptno = d.deptno
group by d.deptno,d.dname,d.loc;

+--------+------------+----------+----------------+----------------------+---------+
| DEPTNO | DNAME      | LOC      | count(e.ename) | ifnull(avg(e.sal),0) | avgyear |
+--------+------------+----------+----------------+----------------------+---------+
|     10 | ACCOUNTING | NEW YORK |              3 |          2916.666667 | 39.3333 |
|     20 | RESEARCH   | DALLAS   |              5 |          2175.000000 | 37.4000 |
|     30 | SALES      | CHICAGO  |              6 |          1566.666667 | 39.8333 |
|     40 | OPERATIONS | BOSTON   |              0 |             0.000000 |    NULL |
+--------+------------+----------+----------------+----------------------+---------+

```

> 小Tips:
>
> 1. 分组函数group，可以有多个字段共同分组。这样在select中就可以查询多个字段
> 2. ifnull(,)这个函数可以把空值替换成想要的值。
> 3. TimeStampDiff(间隔类型，前一个日期，后一个日期)

### 27、列出所有员工的姓名、部门名称和工资 

```sql
select e.ename,d.dname,e.sal
from emp e
join dept d
on e.deptno = d.deptno
order by d.dname;

+--------+------------+---------+
| ename  | dname      | sal     |
+--------+------------+---------+
| CLARK  | ACCOUNTING | 2450.00 |
| MILLER | ACCOUNTING | 1300.00 |
| KING   | ACCOUNTING | 5000.00 |
| FORD   | RESEARCH   | 3000.00 |
| SMITH  | RESEARCH   |  800.00 |
| SCOTT  | RESEARCH   | 3000.00 |
| JONES  | RESEARCH   | 2975.00 |
| ADAMS  | RESEARCH   | 1100.00 |
| BLAKE  | SALES      | 2850.00 |
| ALLEN  | SALES      | 1600.00 |
| WARD   | SALES      | 1250.00 |
| TURNER | SALES      | 1500.00 |
| MARTIN | SALES      | 1250.00 |
| JAMES  | SALES      |  950.00 |
+--------+------------+---------+
```

### 28、列出所有部门的详细信息和人数 

```sql
select d.deptno,d.dname,d.loc,count(e.ename)
from dept d
left join emp e
on e.deptno = d.deptno
group by d.deptno,d.dname,d.loc;

+--------+------------+----------+----------------+
| deptno | dname      | loc      | count(e.ename) |
+--------+------------+----------+----------------+
|     10 | ACCOUNTING | NEW YORK |              3 |
|     20 | RESEARCH   | DALLAS   |              5 |
|     30 | SALES      | CHICAGO  |              6 |
|     40 | OPERATIONS | BOSTON   |              0 |
+--------+------------+----------+----------------+
```

> 小Tips: 在查询部门的数量的时候，一定要使用left join。因为只有dept表中使用了编号为40的部门，而emp表中没有。如果使用join是内连接（交集），就会丢失一个deptno=40的值。这里使用左外连接，leftjoin,取dept表的全集。

### 29、列出各种工作的最低工资及从事此工作的雇员姓名

```sql
-- 求出工作的最低工资
select job,min(sal) as minsal
from emp
group by job;

+-----------+---------+
| job       | minsal  |
+-----------+---------+
| ANALYST   | 3000.00 |
| CLERK     |  800.00 |
| MANAGER   | 2450.00 |
| PRESIDENT | 5000.00 |
| SALESMAN  | 1250.00 |
+-----------+---------+

-- emp与上表连接
select e.ename,t.*
from emp e
join (select job,min(sal) as minsal
from emp
group by job)t
on e.job = t.job and e.sal = t.minsal;
+--------+-----------+---------+
| ename  | job       | minsal  |
+--------+-----------+---------+
| SMITH  | CLERK     |  800.00 |
| WARD   | SALESMAN  | 1250.00 |
| MARTIN | SALESMAN  | 1250.00 |
| CLARK  | MANAGER   | 2450.00 |
| SCOTT  | ANALYST   | 3000.00 |
| KING   | PRESIDENT | 5000.00 |
| FORD   | ANALYST   | 3000.00 |
+--------+-----------+---------+
```

### 30、列出各个部门的MANAGER(领导)的最低薪金

真坑啊，这里的MANAGER指的是工作岗位。。。

```sql
select deptno,min(sal)
from emp
where job = 'MANAGER'
group by deptno;

+--------+----------+
| deptno | min(sal) |
+--------+----------+
|     10 |  2450.00 |
|     20 |  2975.00 |
|     30 |  2850.00 |
+--------+----------+
```

### 31、列出所有员工的年工资,按年薪从低到高排序 

```sql
select ename,(sal+ifnull(comm,0))*12 as yearsal
from emp
order by yearsal asc;

+--------+----------+
| ename  | yearsal  |
+--------+----------+
| SMITH  |  9600.00 |
| JAMES  | 11400.00 |
| ADAMS  | 13200.00 |
| MILLER | 15600.00 |
| TURNER | 18000.00 |
| WARD   | 21000.00 |
| ALLEN  | 22800.00 |
| CLARK  | 29400.00 |
| MARTIN | 31800.00 |
| BLAKE  | 34200.00 |
| JONES  | 35700.00 |
| SCOTT  | 36000.00 |
| FORD   | 36000.00 |
| KING   | 60000.00 |
+--------+----------+
```

### 32、求出员工领导的薪水超过3000的员工名称与领导名称

```sql
select a.ename as 'ename',b.ename as 'boss'
from emp a
join emp b
on a.mgr = b.empno
where b.sal>3000;
+-------+------+
| ename | boss |
+-------+------+
| JONES | KING |
| BLAKE | KING |
| CLARK | KING |
+-------+------+

```

### 33、求出部门名称中,带'S'字符的部门员工的工资合计、部门人数 

```sql
select d.deptno,d.ename,d.loc,count(e.ename),ifnull(sum(e.sal),0)
from emp e
right join dept d
on e.deptno = d.deptno
where d.dname like '%S%'
group by d.deptno,d.dname,d.loc;

+--------+------------+---------+----------------+----------------------+
| deptno | dname      | loc     | count(e.ename) | ifnull(sum(e.sal),0) |
+--------+------------+---------+----------------+----------------------+
|     20 | RESEARCH   | DALLAS  |              5 |             10875.00 |
|     30 | SALES      | CHICAGO |              6 |              9400.00 |
|     40 | OPERATIONS | BOSTON  |              0 |                 0.00 |
+--------+------------+---------+----------------+----------------------+
```

### 34、给任职日期超过30年的员工加薪10% 

```sql
update emp set sal = sal*1.1 where timestampdiff(YEAR,hiredate,now());
```

`Rows matched: 14  Changed: 14  Warnings: 0`


