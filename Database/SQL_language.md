<a name="tjCEt"></a>
# 基本类型
1. char(n)：固定长度的字符串，用户指定长度n。也可以使用全称character。
1. varchar(n)：可变长度的字符串。用户指定最大长度n，等价于全称character varying。
1. int：整数类型。等价于全称integer。
1. smallint：小整数类型。
1. numeric(p, d)：定点数，精度由用户指定。这个数有p位数字（加上一个符号位），其中d位数字在小数点右边。所以在一个这种类型的字段上，numeric(3,1)可以精确到44.5，但不能精确存储444.5或0.32。
1. real，double，precision：浮点数与双精度浮点数，精度与机器相关。
1. float(n)：精度至少为n位的浮点数。

<a name="YuKwu"></a>
# 完整性约束
```sql
primary key (ID)

foreign key (dept_name) references department
```
<a name="oElTv"></a>
# 修改删除表
```sql
drop table student 
--删除表

delete from student
--删除表的所有内容，但是保留表

alter table r add A D
-- A是属性名，D是A的domain

alter table r drop A
--A是关系r的属性名
```
<a name="eC79N"></a>
# 基本查询语言
```sql
select distinct dept_name
from instructor
--忽略重复的

select all dept_name
from instructor
--不删除重复的

select *
from instructor
--选择此关系所有属性

select ID, name, salary/12
from instructor
--select字句可以包含+-*/
```
<a name="SKoOL"></a>
# Joins
```sql
select *
from instructor, teaches
--生成每一个可能的instructor-teacher pair，包含两表的所有属性

 select name, course_id
 from instructor, teaches
 where instructor.ID = teaches.ID
 
 select section.course_id, semester, year, title
 from section, course
 where section.course_id = course.course_id and dept_name = 'Comp.Sci.'
```
<a name="dWugO"></a>
# Natural Join
```sql
select name, course_id
from instructor, teaches
where instructor.ID = teaches.ID;
--等价于下面
select name, course_id
from instructor natural join teaches;
/*自然连接：如果两表有属性的名称相同，那么就将两个表中此属性值相同的tuple找出来做笛卡尔积
并且将属性名相同的两列合并成一列。
这里就是找到两个表都有命名为ID的属性，就找到两个表ID相同的值做连接*/

--使用natural join时注意：同名的不相关属性被错误地做相等判断，如：
select name, title
from instructor natural join teaches natural join course
--这里将使得course.dept_name = instructor.dept_name
--显然是让没有关系的两列做了无意义的错误连接，正确版本如下：
select name, title
from instructor natural join teaches, course
where teaches.course_id = course.course_id;
--或者
select name, title
from (instructor natural join teaches)                                            
join course using(course_id);
```
<a name="gtAnH"></a>
# Rename Operation
```sql
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Comp. Sci.'
--as可以省略

--self join
SELECT e1.name AS employee_name, e1.salary, e2.name AS manager_name, e2.salary
FROM employee AS e1, employee AS e2
WHERE e1.manager_id = e2.id
AND e1.salary > e2.salary
```
<a name="aW5dj"></a>
# String Operation
```sql
--SQL标准中字符串的相等操作是大小写敏感的，但是如MySQL和SQLserver就是不敏感的。
% --代表匹配的子串
_ --代表匹配的字符
'Intro%' --matches any string beginning with “Intro”.
'%Comp%' --matches any string containing “Comp” as a substring.
'_ _ _' --matches any string of exactly three characters.___没有空格吧
'_ _ _ %' --matches any string of at least three characters.

--escape转义字符
like '100 \%' escape '\ '
--匹配‘100%’的字符

--其他操作
|| --拼接字符串
upper(s)
lower(s)
trim(s)
```
<a name="OgtrF"></a>
# 顺序
```sql
select distinct name
from instructor
order by name

--顺序可以指定
order by name desc

order by dept_name, name

--希望按照salary的降序排列instructor关系，如果salary相同则按名字的升序排
select *
from instructor
order by salary desc, name asc
```
<a name="U3Dfj"></a>
# where字句
```sql
--where 指明了结果必须满足的情况
--包括逻辑连接 or、and、not、between and
```
<a name="EDYgY"></a>
# Set Operation
```sql
--集合操作包括union，intersect，except这些对关系的操作。
--上述操作自动忽略了重复的，如果需要保留这些重复的，则需要使用
union all
intersect all
except all

--Find courses that ran in Fall 2009 or in Spring 2010
(select course_id from section where sem = 'Fall' and year = 2009)
union
(select course_id from section where sem = 'Spring' and year = 2010)

```
<a name="VkSrJ"></a>
# Null
```sql
--null 参与运算的结果是null，参与比较的结果是unknown
--可以用is null来判断是否为null，is unknown来判断是否为unknown

/*unknown的三值逻辑
OR: (unknown or true)   = true,
(unknown or false)  = unknown
(unknown or unknown) = unknown

AND: (true and unknown)  = unknown,    
(false and unknown) = false,
(unknown and unknown) = unknown

NOT:  (not unknown) = unknown*/

--如果where字句里面是unknown，则视作false

--null = null 判断为unknown
--{('A',null),('A',null)}两个元组被认为相同的，select distinct就丢掉了
```
<a name="DTaMt"></a>
# 聚集函数
```sql
/*
avg(attr):   return the average attr value
min(attr):   return the minimum attr value
max(attr):   return the maximum attr value
sum(attr):   return the sum of values in attr
count(attr): return the number of values for attr
*/
select avg (salary)
from instructor
where dept_name= 'Comp.Sci.';

select count (distinct ID)
from teaches
where semester = 'Spring' and year = 2010

select count (*)
from course;
--注意count(distinct *)是错误表达

select dept_name, avg (salary)
from instructor
group by dept_name;
--group by字句中的所有属性取值相同的元组将被分在一个组中

select dept_name, ID, avg(salary)
from instructor
group by dept_name;
--错，select中的属性只能是group by里面的属性，或者是放在聚合函数里。此处ID错

/*
having字句中的为此在group by形成分组之后产生作用
having中出现的属性必须出现在group by中或者放在聚集函数里。
1.先根据from字句来计算出一个关系
2.如果出现了where，则where字句的为此将应用到from字句的结果关系上，产生一个结果
3.如果出现了group by，则把where的结果进行分组。否则where整体就是一个分组。
4.如果出现了having，就将它应用到分组上，，不满足having的分组将被抛弃
5.select将剩下的分组继续查询，在每个分组上应用聚集函数得到单个结果元组
*/
select dept_name, avg (salary)
from instructor
group by dept_name
having avg (salary) > 42000;
--查找平均薪资高于42000的所有部门的名称和平均薪资

--sum等操作忽略null，若属性取值全为null时结果为null，count是取0
--count(*)是计算null的
```



<a name="NMcqJ"></a>

# 嵌套子查询

```sql
select A1, A2, ..., An
from r1, r2, ..., rm
where P
--ri可以为任何有效的子查询
--P可以为一个 属性<operator>(子查询) 的一种格式
--Ai可以为一个生成单个值的子查询
```

<a name="WEdLw"></a>

## 设置成员资格

```sql
in, not in

--选择2009年秋季与2010件春季都有的课程
select distinct course_id 
from section
where semester = 'Fall' and year= 2009 and 
  course_id in (select course_id
                from section
                where semester = 'Spring' and year= 2010);

--选择2009年秋有但是2010年春没有的课程
select distinct course_id 
from section
where semester = 'Fall' and year= 2009 and 
  course_id not in (select course_id
                from section
                where semester = 'Spring' and year= 2010);
                
                
--计算ID10101的老师教的distinct学生数
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in
          (select course_id, sec_id, semester, year
           from teaches
           where teaches.ID= 10101);
--理解成学生选的课程是教师10101开设课程的子集，
--满足这个条件的学生就是满足查询要求的学生
```

<a name="IsCs1"></a>

## 集合的比较

```sql
some, all

--查找比biology系的至少一个教师的工资要高的老师
select distinct T.name
from instructor as T, instructor as S
where T.salary > S.salary and S.dept_name = 'Biology';

-- >some：比集合中的某些元素大
select name
from instructor
where salary > some (select salary
                     from instructor
                     where dept_name = 'Biology');

-- =some 等价于 in, <>some 确不等价于 not in

--查询比生物系的所有老师的工资都高的老师
select name
from instructor
where salary > all (select salary
                    from instructor
                    where dept_name = 'Biology');
-- <>all 等价于 not in, =all 不等价于in

--找出平均工资最高的系
select dept_name
from instructor
group by dept_name
having avg(salary) >= all( select avg(salary)
                           from instructor
                           group by dept_name );
```

<a name="B5h4i"></a>

## 空关系测试

```sql
exists, not exists

--查询2009年秋和2010年春同时开课的所有课程，另一种方法
select course_id
from section as S
where semester = 'Fall' and year = 2009 and
    exists ( select *
             from section as T
             where semester = 'Spring' and year = 2010 and
                S.course_id = T.course_id );
--这说明外层查询的一个相关名称(如S)可以用在where自居的子查询中。
--使用了来自外层查询相关名称的子查询被称作相关子查询correlated subquery


--exists r 等价于 r不为空
--not exists r 等价于 r为空

--灵活应用： X - Y = 空集 等价于 X 属于 Y
--所以 not exists ( X except Y ) 等价于 X 属于 Y
--查询找出选修了biology系开设的所有课程的学生
select distinct S.ID, S.name
from student as S
where not exists ( (select course_id
                    from course
                    where dept_name = 'Biology') --生物系开设的所有课程
                    except
                    (select T.course_id
                     from takes as T
                     where S.ID = T.ID) ); --学生选的课
--这样，外层select对每个学生测试其选修的所有课成绩和是否包含biology系开设的所有课程集合
```

<a name="ESSGG"></a>

## 重复元组存在性测试

```sql
---查询找出所有在2009年最多开设一次的课程
 select T.course_id
 from course as T
 where unique (select R.course_id
               from section as R
               where T.course_id= R.course_id  --外键约束
               and R.year = 2009);
--布尔函数unique，它的参数的子查询结果如果没有重复元素，则返回true
--如果子查询为空的结果，unique谓词在空集上计算出的结果为true
--因为如果关系中存在着两个元组t1和t2的某个域为空，判断t1=t2为假，所以unique可能为真

--上述查询等价的表达
select T.course_id
from course as T
where 1 >= ( select count(r.course_id)
             from section as R
             where T.course_id = R.course_id and R.year = 2009 );
```

<a name="MwQ5i"></a>

## From字句中的子查询

```sql
--关系模型时特别强调关系运算的结果依然是一个关系，关系就是一个数据集合。
--而from子句后面的参数就是某个数据集合

--找出系的平均工资超过42000的系中的教师的平均工资
select dept_name, avg_salary
from (select dept_name, avg (salary) as avg_salary
      from instructor
      group by dept_name)
where avg_salary > 42000;
--不需要having，因为from自居中的子查询计算出了每个系的平均工资
--早先在having字句中使用的谓词现在出现在外层查询的where字句中

--另一个方法
select dept_name, avg_salary
from (select dept_name, avg (salary)
      from instructor
      group by dept_name) 
          as dept_avg (dept_name,  avg_salary) --as字句给此子查询的结果关系取了个名字
where avg_salary > 42000;  

```

<a name="EI2Qa"></a>

## with字句

```sql
--With子句提供了一种定义临时关系的方法，这个临时关系可以用于查询，但是只对包含with子句的查询有效
with max_budget (value) as --临时关系记录了最大预算值
  (select max(budget)
   from department)
select budget
from department, max_budget
where department.budget = max_budget.value;

--查询所有的工资总额大于所有系平均工资总额的系
with dept _total (dept_name, value) as
        (select dept_name, sum(salary)
         from instructor
         group by dept_name),
     dept_total_avg(value) as
        (select avg(value)
         from dept_total)
select dept_name
from dept_total, dept_total_avg
where dept_total.value >= dept_total_avg.value;
```

<a name="TU99Z"></a>

## 标量子查询（scalar subquery）

```sql
--SQL允许子查询出现在返回单个值的表达式可以出现的任何地方
--只要子查询只返回包含单个记录属性的单个元组

--列出所有系以及他们的教师数
select dept_name, (select count(*)
                    from instructor 
                    where department.dept_name =  
                        instructor.dept_name)  as num_instructors
from department;
--编译时不能判断一个子查询返回的结果是否又多个元素，所以执行可能产生运行时错误
```


<a name="uTtGx"></a>

# 数据库的修改

<a name="oIncp"></a>

## 删除

```sql
delete from instructor
where dept_name = 'Finance';

delete from instructor
where dept_name in (select dept_name
                    from department
                    where building = 'Watson');

--delete只能作用域一个关系。如果想从多个关系中删除元组，必须每个关系单独用一条delte
--where字句可以为空

delete from instructor
where salary < (select avg (salary) from instructor);
--首先测试instructor关系的每一个元组，检查其工资是否小于大学教师的平均工资
--然后再删除所有符合条件的元组。
```

<a name="Yp4Rn"></a>

## 插入

```sql
insert into student
values ('3003', 'Green', 'Finance', null);

--应用insert into语句实现数据入库操作时，插入的数据可能来自另一个数据集的查询结果
--这时候select查询语句全部执行完成之后再进行insert操作。
 insert into student
 select ID, name, dept_name, 0
 from instructor
```

<a name="bRc0S"></a>

## 更新

```sql
--工资超过100000的教师涨3%,其余教师涨5%
update instructor
set salary = case
        when salary <= 100000 then salary * 1.05
        else salary * 1.03
    end

--标量子查询
--把每个student元组的tot_cred属性设置为改学生成功完成的课程学分的综合
update student S 
set tot_cred = ( select sum(credits)
                 from takes natural join course
                 where S.ID= takes.ID and 
                    takes.grade <> 'F' and
                    takes.grade is not null );

```



<a name="Kwx6J"></a>

# join

- join操作奖两个关系结合返回一个新的关系，通常用于子查询的from子句中
- join type包括inner join, outer join, left outer join, right outer join, full outer join
- join match定义了两个关系应该匹配哪个元组以及结果中有那些属性，包括natural，on<predicate>，using(A1,A2....An)

```sql
select * from students join takes on student.ID=takes.ID  

select * from students, takes  where student.ID=takes.ID 

select * from students natural join takes

--效果是都一样，不过前两者的结果中有两个ID，natural则将同名的合并为一个
--且on和where的表现不一样
```

<a name="XzU9t"></a>

## outer join

- join可能会因为关键字找不到匹配而丢失
- 通过在结果中创建包含空值元组的方式，保留了在连接中丢失的元组，这些元组的连接后的其他属性被设置为null
- left outer join：只保留出现在此连接运算之前左边的关系中的元组
- right outer join：只保留右边关系中的元组
- full outer join：保留出现在两个关系中的元组
- inner join：之前我们学习的不保留未匹配元组的连接运算叫内连接

```sql
--左外连接:找出所有的学生，他们一门课都没有选修
SELECT ID
FROM student NATURAL LEFT OUTER JOIN takes
WHERE course_id IS NULL

--右外连接和左外连接是对称的
SELECT ID
FROM takes NATURAL RIGHT OUTER JOIN student
WHERE course_id IS NULL
```

<a name="LS7vZ"></a>

## 连接的类型和条件

- 内连接inner关键词可选
- 任何的连接形式可以和任意的连接调节进行组合

<a name="Dav9r"></a>

# 视图view

- with字句定义的临时关系无法抑制有效，如果之后的查询还需要用到此关系，with就不可用了。
- 在一些应用中只允许部分用户看到部分数据，符合特定用户直觉的个人化的关系集合
- 不是真是存在的关系，但是又对用户可见。SQL允许通过查询来定义“虚关系”，并不预先计算存储，而是使用时才通过执行查询被计算出来。不是逻辑模型的一部分，但作为虚关系对用户可见的关系称为**view**
- create view 名字 as <查询表达式>
- 一旦定义了view，view名字就可以作为虚拟关系名来引用。视图名可以出现在关系名可以出现的任何地方。

```sql
CREATE VIEW faculty AS
SELECT ID, name, dept_name
FROM instructor

select name
from faculty
where dept_name = 'Biology'
```

<a name="QzPK4"></a>

## 物化视图

- 特定数据库系统允许存储视图关系。如果用于定义视图的实际关系改变，视图也跟着修改。
- 如果视图物化，结果就会存放在数据库中。在物化视图维护（materialized view maintenance）时，不同数据库可能在关系更新或者视图被访问时或者周期进行执行维护
  <a name="iQzyl"></a>

## 视图更新

```sql
insert into faculty values ('30765', 'Green', 'Music');

/*对实际关系instructor进行插入，元组必须把salary赋值为null插入
  或者选择报错拒绝插入*/
  
create view instructor_info as
  select ID, name, building
  from instructor, department
  where instructor.dept_name= department.dept_name;

insert into instructor_info values ('69987', 'White', 'Taylor');
/*假设没有ID为69987的教师，也灭有位于Taylor大楼的系。那么向instructor
  和department关系中插入元组的唯一可能方法时：向instructor插入
  ('69987','White',null,null)向department插入(null,'Taylor',null)
  但是instructor_info依旧不包含我们刚才插入的元组。因此利用null来更新
  instructor和department关系来得到对instructor_info的更新的不科学的*/
```

- 一般不允许对视图进行修改，除非以下条件都满足（不同数据库的指定条件也不同）

1. FROM子句只有一个数据库关系
1. SELECT子句只包含关系的属性名，不包含任何表达式、聚集函数或DISTINCT声明
1. 任何没有出现在SELECT子句中的属性可以取空值，即没有NOT NULL约束，也不构成主码的一部分
1. 查询中不含有GROUP BY或HAVING子句

```sql
--但是就算可更新的情况下依旧可能存在问题
CREATE VIEW history_instructor AS
SELECT *
FROM instructor
WHERE dept_name='history'

insert into history_instructor values ('25566','brown','biology',10000);
--但是这条操作不符合where的条件，所以不会出现在视图history_instructor中

/*可以在视图定义的末尾包含 WITH CHECK OPTION来定义视图，
  这样插入不满足where条件的元组时数据库会报错*/
```

<a name="xebjl"></a>

# 事务

- 事务由查询和更新语句组成
- SQL规定当一条SQL语句被执行就隐式地开始了一个事务。
- 事务必须以以下的语句结束
  - commit work：提交当前事务，将事务做的更新在数据库里持久保存。在事务被提交后，自动开始一个新的事务
  - rollback work：回滚当前事务，撤销该事务中所有SQL语句对数据库的更新。数据库恢复到执行该事务第一条语句之前的状态。一旦commit，不能rollback。某条SQL错误、断电和系统崩溃等异常时若未commit则将rollback
- 原子性：事务完成所有步骤后提交其行为，不能成功完成所有动作的情况下回滚所有动作，要么都有，要么都没有，通过这种方式数据库提供了对事务具有原子性的抽象，即不可分割性
- 默认情况下每个SQL语句自成一个事务，一旦完成就提交。如果一个事务要执行多条SQL语句，则需要关系单独SQL语句的自动提交。

<a name="C1XmQ"></a>

# 完整性约束

- 保证授权用户对数据库的修改不会破坏数据的一致性。防止对数据的意外破坏。如：
  - 姓名不能为null
  - 两个ID不能相同
  - course关系中的每个系名必须在department关系中有对应
  - 一个系的预算必须大于0
- 一个完整性约束可以是属于数据库的任意谓词，但是检测任意谓词的代价太高。所以带多数数据库系统允许用户指定那些只需极小开销就可以检测的完整性约束
- 完整性约束可以通过create table命令的一部分被声明，也可以通过ALTER TABLE table_name ADD constraint施加，在修改数据库表的时候被检验
- 完整性约束的类型
  - key和foreign key：引用完整性
  - 基于值的约束：特定属性的约束
  - 基于元组的约束：属性之间的约束
  - 断言：一般指数据库中所有的表需要满足的约束，检测代价很大
    <a name="HlMCc"></a>

## 单个关系的约束：

   - not null
   - primary key
   - unique
   - check(P谓词)

```sql
--not null
name   varchar(20) not null
budget  numeric(12,2) not null
--空值是所有域的成员，默认SQL的每个属性的合法值
--声明属性为主码，则不必显示地声明not null

--unique
UNIQUE(A1, A2, A3....,An)
/*声明指出以上属性群形成了一个候选码（candidate key)，在关系中没有两个
  元组能在上述属性群上取值相同。候选码的属性可以为null（空值不等于任何值）*/

--check(P)
create table section (
    course_id   varchar (8),
    sec_id   varchar (8),
    semester   varchar (6),
    year   numeric (4,0),
    building  varchar (15),
    room_number   varchar (7),
    time slot id   varchar (4), 
    primary key (course_id, sec_id, semester, year),
    check (semester in ('Fall', 'Winter', 'Spring', 'Summer'))
);
--关系中的每个元组都必须满足谓词P
--上述check只当了semester必须是春夏秋冬的一个予以限制
```

<a name="UvIXy"></a>

## 参照完整性

- 保证在一个关系中给定属性集合上的取值也在另一关系的特定属性集的取值中出现
- 又叫子集依赖（subset dependency）
- 默认情况下，SQL中外码参照的是被参照表的主码属性。也可以支持显式指定被参照关系的属性列表的references子句，这个指定的属性列表必须声明为被参照关系的候选码，使用primary key或者unique约束

```sql
dept_name varchar(20) references department
```

- 违反参照完整性约束时通常是拒绝执行，但也可指定一些操作来恢复

```sql
create table course (
  …
  dept_name varchar(20),
  foreign key (dept_name) references department
  on delete cascade
  on update cascade,
  …
)
/*外码依赖链
  由于有与外码声明相关联的on delete cascade子句，如果删除department中的
  元组导致此参照完整性的约束被违反，则对course关系做级联删除，
  如果更新参照字段违反了约束，则将course中参照元组dept_name字段也改为新值，
  除了cascade，还可以指明set null动作（约束被违反时将参照域dept_name置为null），
  或者set default置为默认值*/
```

<a name="GBXxC"></a>

## 事务中违反完整性约束

- 一个事务可能包含多个步骤，在某一步之后可能暂时违反完整性约束，但是可能后面一步就会消除此违反
- 假设person关系中有name和spouse属性，spouse是person的一个外码。假设插入John和Mary且彼此为配偶，无论先插入哪一个都是违反完整性约束
- 可以把initially deferred子句加到约束声明，这样就会在事务结束的时候检查约束，执行**set constraints **constraint-list** deferred**作为事务的一部分。
- 如果可以为空，则都先设spouse为null插入，之后再更新spouse的值
  <a name="agR3L"></a>

## 断言

- 一个断言是一个为此，表达了我们希望数据库总能满足的一个条件
- 和check一样，代价很大
- **create assertion **<assertion-name> **check** <predicate>;

```sql
/*对于student关系的每个元组，他在属性tot_cred上的取值必须等于
  该同学所成功修完课程的学分总和*/
CREATE ASSERTION credits_earned_constraint CHECK(
    NOT EXISTS ( 
        SELECT ID
        FROM student
        WHERE tot_cred <> (
            SELECT SUM(credits)
            FROM takes NATRUAL JOIN course
            WHERE student.ID = takes.ID
            AND grade IS NOT NULL AND grade <> 'F'
        )
    )
)
--没有for all X, P(X)的结构，所以用not exists代替
```

<a name="RSh7v"></a>

# 数据类型

<a name="kI9YM"></a>

## 日期和时间类型

- **date**：日期，包括年（四位）、月、日，如：2004-7-24
- **time**：一天中的时间，包括小时、分、秒，如：09:00:30.75。可以用time(p)来表示小数点后的数字位数（默认为0），通过指定time with timezone可以把时区信息一起存储。
- **timestamp**：date+time。timestamp(p)来表示秒的小数点后的数字位数（默认为6），如：2001-04-25 10:25:01.45
- **cast **e **as ** t，将字符串e转化成类型t（date、time和timestamp）
- **extract **(field **from ** d)，从date、time或者timestamp的值里提取出单独的field（包括**year**，**month**，**day**，**hour**，**minute**，**second**，**timezone_hour**，**timezone_minute**等）
- SQL定义了一些函数来获取当前日期和时间。**current_date**：当前日期，**current_time**：当前时间（带有时区），**localtime**：本地时间（不带时区），**current_timestamp**和**localtimestamp**：带/不带 时区的时间戳
- **interval**：在date，time和timestamp进行计算，得到两个日期之间的天数。也可以在时间上加interval来获得新的时间
  <a name="GqSay"></a>

## 索引

- 数据库读取每条记录并一一检查ID是否匹配是很低小的
- index是在关系的属性上所创建的一种数据结构，让数据库系统高效地找到关系中那些在索引属性上去给定值的元组而不用扫描全部元组
- **create index ** studentID_index **on **student(ID)
- 如果SQL查询可以从索引的使用中获益，则处理器会自动使用索引
  <a name="FNxXL"></a>

## 大对象

- 字符数据的大对象数据类型 **clob**: book_review clob(10kb)
- 二进制数据的大对象数据类型**blob**: image blob(10Mb)
- 查询语句返回的是pointer
- 把大对象放入内存中非常低效，一个应用通常用SQL查询出此大对象的“定位器（locator）”并在宿主语言中用定位器来操纵对象。
- 其实也可以用文件系统来代替大对象的一些功能
  <a name="Yap1J"></a>

## 用户定义的类型

- 独特类型（distinct type）：人名和系名的比较无意义，英镑和美元的比较会出错

```sql
create type Dollars as numeric (12,2) final --final可以不用管
create table department(
  budget Dollars
)
--强类型检查，department.budget+20因为是不同类型所以会报错
--可以转换到另一个域：cast (department.budget to numeric(12,2))

--drop type 和 alter type可以删除或者修改以前创建的类型

--domain和type相似但有所不同，它可以施加完整性约束，定义默认值
--域不是强类型，因此只要基本类型相同，一个域的类型值可以赋值给另一个域
create domain person_name char(20) not null

--check字句应用到域上时允许设计者指定一个谓词，让该域的任何变量都必须满足此谓词
create domain YearlySalary numeric(8,2)
  constraint salary_value_test check (value >= 29000.00)
  
  --虽然SQL标准是这样，但不同的数据库可能对domain和type的支持不一样
```

<a name="bGCw8"></a>

## create table 扩展

```sql
--创建与某个表的模式相同的表
create table temp_instructor LIKE instructor

--把查询结果存储到临时的新表
CREATE TABLE t1 AS(
  SELECT *
  FROM instructor
  WHERE dept_name = 'Music'
)WITH DATA
--没有with data则会创建表但不会载入数据，但又很多数据库有默认加载了数据
```

<a name="Lbbqn"></a>

## 模式、目录与环境

没看懂

<a name="cCZ9b"></a>

# 授权

- 对数据的授权包括：增删改查权限
- 数据库模式上的权限：创建、修改、删除关系
- 访问对象：数据表或view或数据库模式
  <a name="Y8m6J"></a>

## 权限的授予与收回

```sql
GRANT <权限列表>
ON <关系名或视图名>
TO <用户/角色列表>

GRANT SELECT ON department TO Amit,Satoshi

--update 权限允许用户只在某些属性上被授予
GRANT UPDATE(budget) ON department TO Amit,Satoshi

--insert 同理

--用户名public指系统所有当前用户和将来用户，所以对public授权就是对全部用户授权

--拥有某个权限的用户可以进一步把权限授予给其他用户

--SQL不允许对一个关系的指定元组进行授权

--收回权限
REVOKE <权限列表>
ON <关系名或者视图名>
FROM <用户/角色列表>
```

<a name="uvJPe"></a>

## 角色

- 给每个人分配权限很繁琐，可以先给创建角色，给角色分配权限，再给用户分配角色

```sql
CREATE ROLE instructor;
GRANT instructor TO Amit;

GRANT SELECT ON takes TO instructor;

--角色可以分配给用户或者其他角色
CREATE ROLE teaching_assistant
GRANT teaching_assistant TO instructor;
--instructor继承了teaching_assistant的所有权限
```

<a name="U4X55"></a>

## 视图的授权

```sql
create view geo_instructor as(
  select *
  from instructor
  where dept_name = ’Geology’
);
GRANT SELECT ON geo_instructor TO geo_staff
```

<a name="PUqo4"></a>

## 其他：待续



<a name="pEHXr"></a>

# 函数和过程

<a name="hEzgd"></a>

## 声明和调用SQL函数和过程

```sql
--给定一个系的名字，返回该系的教师数目
CREATE FUNCTION dept_count (dept_name varchar(20))
  RETURNS integer
    BEGIN
    DECLARE d_count integer;
        select count (* ) INTO d_count
        from instructor
        where instructor.dept_name = dept_name
    RETURN d_count;
    END
    
------

--调用
select dept_name, budget
from department
where dept_count (dept_name ) > 12
```

- **表函数**：返回的结果是关系

```sql
--以表为值的函数可以被看作带参数的视图，它通过允许参数把视图的概念更加一般化
CREATE FUNCTION instructors_of (dept_name char(20))
		RETURNS TABLE (
      ID  varchar(5),
      name varchar(20),
      dept_name varchar(20),
      salary  numeric(8,2))
RETURN TABLE
  (select ID, name, dept_name, salary
   from  instructor
   where instructor.dept_name = instructors_of.dept_name)
--??使用函数的参数时需要加上函数名作为前缀：instructor_of.dept_name

------

select *
from table (instructors_of ('Music'))
```

- 过程

```sql
--IN和OUT分别表示待赋值的参数和为返回结果而在过程中设置值的参数
CREATE PROCEDURE dept_count_proc (IN dept_name varchar(20),OUT d_count integer)
  BEGIN
	  select count(*) INTO d_count
    from instructor
    where instructor.dept_name = dept_count_proc.dept_name
  END

------

--可以从一个SQL过程或者嵌入式SQL里使用call语句调用过程
DECLARE d_count integer;
CALL dept_count_proc('Physics', d_count);
```

<a name="DBE2Q"></a>

## 支持过程和函数的语言构造

- 变量用过**declare**语句进行声明，可以是任意的合法SQL类型。使用**se**t语句进行赋值
- 一个复合语句有begin...end的形式，begin和end之间会包含复杂的SQL语句
- 可以在复合语句中声明局部变量
- **begin atomic**...**end**的符合语句可以确保其包含的所有语句作为单一事务

```sql
DECLARE n integer DEFAULT 0;
		WHILE n < 10 do
		    SET n = n + 1
		END WHILE
    
		REPEAT
        SET n = n  – 1
		UNTIL n = 0
		END REPEAT

------

/*
查找位于Watson大楼的所有院系的budget总和，
其中n是局部变量，初始化为0，for循环执行查询，并做累加计算
*/
--for循环，对查询到的所有结果重复执行
DECLARE n integer DEFAULT 0;
FOR r AS
  select budget 
  from department
  where building='Watson'
DO
  SET n = n + r.budget
  END FOR
--程序每次获得查询结果的一行并存入for循环变量（r）中
--leave语句可以跳出循环，iterate表示跳过剩余语句从循环的开始进入下一个元组（break和continue？）

-------

--if-then-else
IF 布尔表达式
  THEN 复合语句
ELSEIF 布尔表达式
  THEN 复合语句
ELSE 复合语句
END IF

-----

--SQL支持case语句

-----

--SQL支持发信号通知异常条件，声明句柄来处理异常
DECLARE out_of_classroom_seats CONDITION
DECLARE EXIT HANDLER FOR out_of_classroom_seats
BEGIN
  …
END
/*
  begin和end之间的语句可以执行signal out_of_classroom_seat来引发一个异常。
  如果异常发生将会采取行动终止begin end中的语句。
  continue可以继续从引发异常语句的下一句开始执行
*/
```

<a name="VQkbm"></a>

# 触发器

- 之名什么条件下执行触发器。分为引起触发器被检测的事件和一个触发器执行必须满足的条件
- 指明触发器执行的动作
- 例如教材的例子，time_slot表记录了课程可能的每一个时段，section表记录了课程开设的时间和地点信息，其中开课时段time_slot_id和time_slot表中的的time_slot_id保持一致性，但是又无法用外键来约束。这时候可以用trigger的机制

```sql
/*
  当对section表进行insert操作时，
  如果增加的新的纪录的time_slot_id不在time_slot表中，
  那么这个这个insert操作无法成功执行，插入的数据要删掉，
  所有对数据库的操作rollback（回滚），恢复插入之前的数值。
*/
CREATE TRIGGER timeslot_check1 AFTER INSERT ON section
  REFERENCING NEW ROW AS nrow  --建立一个临时变量存储新增的数据，是一个临时表
  FOR EACH ROW   --一个SQL插入语句可以像关系中插入多个元组。此可以现实地在每个被插入的行上进行迭代
  WHEN (nrow.time_slot_id NOT IN ( --condition
    SELECT time_slot_id
    FROM time_slot))
  BEGIN
    ROLLBACK --action
  END;

-----

/*检查被删除元组的time_slot_id要么还在time_slot中，要么section里不存在
包含这个time_slot_id值的元组，否则将违背参照完整性*/
CREATE TRIGGER timeslot_check2 AFTER DELETE ON timeslot
  REFERENCING OLD ROW AS orow  --建立临时变量存储删掉或修改行的旧数据
  FOR EACH ROW
  WHEN (
    orow.time_slot_id NOT IN (
      SELECT time_slot_id
      FROM time_slot) AND 
    orow.time_slot_id IN (
      select time_slot_id
      FROM section))
  BEGIN
    rollback
  END;

------

CREATE TRIGGER credits_earned AFTER UPDATE OF takes ON (grade)
   REFERENCING NEW ROW AS nrow
   REFERENCING OLD ROW AS orow
   FOR EACH ROW
   WHEN nrow.grade <> 'F' AND nrow.grade IS NOT NULL
     AND (orow.grade = 'F' OR orow.grade IS NULL)
   BEGIN ATOMIC
     UPDATE student
     SET tot_cred = tot_cred + 
       (SELECT credits
        FROM course
        WHERE course.course_id = nrow.course_id)
      WHERE student.id = nrow.id
    END;

------

--可以在event之前触发
CREATE TRIGGER setnull_trigger BEFORE UPDATE OF takes
  REFERENCING NEW ROW AS nrow
  FOR EACH ROW
  WHEN (nrow.grade = '')
    BEGIN ATOMIC
      SET nrow.grade = null;
    END;

-----

--可以设置为有效或者无效
ALTER TRIGGER 名字 DISABLE
或者
DISABLE TRIGGER 名字
```

<a name="lFRBp"></a>

## 何时不用trigger

- trigger时要特别小心，因为运行期间一个trigger错误会导致引发该trigger的action语句失败，而且一个trigger的动作可以引发另一个触发器，最坏情况下，会导致一个无限的触发链
- 如果有其他替代方法最好不用trigger，可以用存储过程来实现相应的功能
