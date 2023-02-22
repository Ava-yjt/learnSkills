#Mysql语法

Mysql默认启动端口号是3306

##常用命令
（命令不区分大小写）
net stop MySQL
net start MySQL
mysql -u root -p
show databases
create database XXX
use database XXX
exit
source 路径xxx.sql  批量执行sql脚本

#DQL（原表数据不会修改）

单表查询

###查看表
select * from XXX
只查看表的数据结构
desc XXX

###查字段（可以四则运算）
select 字段1,字段2 as 别名 from 表
select 字段1,字段2 '别名（含空格或中文）' from 表

###条件查询
select 字段1,字段2
from 表
where 条件;

条件
<> != >= <= between ... and ... （闭包）
is null  not null
and or 
in (,,) not in(,,)
not
like '_A%'  模糊查询：查找第二个字母是A的

###排序（最后执行）
select 字段1,字段2/字面量
from 表
order by 字段名 （asc默认升序）;
order by 字段名 dsc（指定降序）;
order by 字段1, 字段2 (字段1相同才用2排序)

###数据（单行处理）处理函数
*有Null参与的运算都是null*
upper lower
substr(被截取的字符串, 起始下标, 截取长度)
concat(,) 字符串拼接
length()
trim(" XXX") 去空格
str_to_date
date_format 格式化日期
format 设置千分位
round(字面量，保留小数位数) 四舍五入
rand() 生成随机数
ifnull(空的数据，替换值) 将null转为具体值
case .. when .. then .. when .. then .. else .. end 

##分组函数（多行处理函数）
**必须先分组在使用分组函数，一张表默认为一组**
*分组函数自动忽略null；分组函数不能直接使用在where语句后*

count(字段名) 统计不为null的  count(*) 统计表的总行数
sum(字段名)
avg(字段名)
max(字段名)
min(字段名)

##通用分页limit
limit：分页查询中，将查询结果的一部分显示出来；在order by之后执行
```sql
//select嵌套
select
    ...
from
    ...
order by
    ...
limit 起始下标（默认为0.可缺省）,长度
```
第pageNo页
limit (pageNo - 1) * pageSize, pageSize

##分组查询顺序
```sql
select
    ...
distinct  //去除重复记录
    ...
from
    ...
where
    ... 
group by
    ...
order by
    ...
having    //筛选分组后的各组数据
    ...
limit
    ...
```
 
**执行顺序：from, where, group by, having, select, order by, limit**
select语句中如果有分组，select后只能加分组的字段和分组函数
使用having可以对分组后的数据进一步过滤，但不能单独使用，不能替代where
**去除重复记录**
select distinct ... from ...
distinct 只能放在所有字段的前端，表示联合去重

##连接查询
当需要从多个表中检索数据时，使用连接查询。
需要查找两张表同时存在的数据，使用内连接;
需要查找两张表中一张表存在，另一张表不存在的时候使用左外链接 或 右外链接.
1. 内连接
   >避免笛卡尔积现象；把完全能匹配上这个连接的数据查询出来，两种表没有主次关系

    - 等值连接
    ```sql
    //找出员工的薪资等级
    select
        e.ename, d.dname
    from
        emp e
    (inner) join
        dept d  //给表起别名加效率
    on
        e.deptno = d.deptno;   //e 和 d的连接体条件
    ```

    - 非等值连接
     on e.salary between s.losa and a.hisa
   - 自连接
    ```sql
    //找出员工的上级
    select
        a.ename as '员工名', b.ename as '领导名'
    from
        emp a  //把一张表看成两张表查询
    right join
        emp b  //给表起别名加效率
    on
        a.mgr = b.empno   //a 和 b的连接条件
    ```

1. 外连接
   - 右外连接：将join右边的表中数据全部查询出来，捎带着左边的次表
    ```sql
    //找出员工的上级
    select
        e.ename, d.dname
    from
        emp e
    right outer join
        dept d  //给表起别名加效率
    on
        e.deptno = d.deptno   //e 和 d的连接体条件
    ```
    - 左外连接 left outer
    - 多张表的连接

    ```sql
    //多张表的连接
    select
        ...
    from
        a
    (inner) join
        b
    on
        a 和 b的连接条件
    join
        c
    on
        a 和 c的连接条件
    right join
        d
    on
        a 和 d的连接条件
    ```

##子查询
```sql
//select嵌套
select
    ...
from
    ...
where
    (select ... from ...)

```

```sql
//from后的嵌套可以当作一张临时表，起别名
select
    ...
from
    (select ... from ...) t  （//别名t）
where
    ...

```

##union合并
```sql
select ...
union
select ...
```

在减少匹配次数的情况下，可以完成两个结果集（要求列一致）的拼接

#数据类型
1. varchar（可变长，最长255）char（定长）
2. int（最长11） bigint
3. float double 
4. date（短日期） datetime（长日期）
5. clob（超过255的字符大对象，4G） blob（二进制大对象，图片】声音、视频等媒体类型，需使用IO流）

#DML（对表中数据增删改）
##表的创建
表名_t_或者tb1_开始；所有标识符用_连接，小写。
```sql
create table 表名（
    字段名1 数据类型 defualt 默认值，
    字段名1 数据类型
）；

//快速创建表
create table 表名 as select ... from ...; 
//删除表
drop 表名;  
drop table if 表名 exits;
```

##insert插入数据
```sql
insert into 表名(字段名1,字段名2) values(值1,值2)；  //没有指定的字段为Null；字段名可以省略，等价于写上了所有字段名
//插入多条记录
insert into 表名(字段名1,字段名2) values(值1,值2),(),()；  
//快速插入（要符合表的结构）
insert into 表名 select ... from ...; 
```

**插入日期**
date默认格式 y%-m%-d%
date默认格式 y%-m%-d% %h:%i:%s

str_to_date('01-10-1990,'d%-m%-y% ）将varchar转成date
date_format(日期类型数据,'日期格式 ） 将date转换成特定格式的字符串，通常用在查询日期设置展示的格式
now()获取当前时间,datetime类型

##update修改
```sql
update 表名 set 字段名1 = 值1, 字段名2 = 值2 where ...;
```
##delete  删除数据
```sql
delete from 表名 where ...;
```
表中数据被删除，但是在硬盘上的真实空间不会被释放，可回滚

#DDL（改表结构）
create
drop
alter（添加删除修改字段）

##truncate  快速删除数
```sql
truncate table 表名;
```
表中数据被物理删除，效率高，不支持回滚

#约束（保证数据的有效性）
##非空约束 not null
```sql
字段 类型 not null;
```
##唯一性约束 unique
```sql
字段 类型 unique;   //列级约束
unique(字段名1, 字段名2); //表级约束，联合唯一
```
unique可以都为null

not null和unique可以联合，MYSQL中默认为索引

##主键约束 PK
primary key
每张表都必须有主键，主键值**不为NULL**且不能重复
一般是数字（int bigint char），定长，不需要有意义；
一张表只能添加一个主键约束
- 单一主键
id int primary key
- 复合主键（表级约束）
primary key (id,name)

- 主键自增，从1开始                   
id int primary key auto_increment
- 自然主键（自然数，使用更多），
- 业务主键


##外键约束 FK
foreigh key
子表中的外键引用的父表中某个字段不一定是主键，但**必须具有唯一性**
外键值可以是NULL
```sql
fk_cno int,
foreigh key(fk_cno) references t_class(classno)
//增添外键
ALTER TABLE 表名
ADD FOREIGN KEY (字段名)
//删除外键
ALTER TABLE 表名
DROP FOREIGN KEY 字段名
```
先创父表在建子表，先删子表再删父表

*存储引擎
mysql默认存储引擎engine=InnoDB，默认字符编码charset=utf8，建表时指定*

#事务transaction（DML)
事务：一个完整的业务逻辑，一个最小的工作单元，不可分。本质上是多条DML语句同时成功，同时失败。
关闭mysql自动提交机制： start transaction
手动提交 commit
回滚rollback 回到上次的提交点

##事务的特性
- 原子性A
- 一致性C
- 隔离性I
  隔离级别
  - 读未提交（最低级别）：存在脏读
  - 读已提交：只能读取另一个事务已提交的数据，解决了脏读，问题不可重复读取数据
  - 可重复读（mysql默认）：事务开启后，不管多久，读取到的数据都是一致的，可能会出现幻影读
  - 序列化/串行化：事务排队，不能并发
- 持久性D

#数据库设计的三范式
避免表中数据冗余，空间浪费。
1. 任何一张表必须有主键，每一个字段原子性不可再分（字段要精细）；
2. 所有非主键字段完全依赖主键，不要产生部分依赖（例如学生教师，复合主键会造成数据冗余。**多对多，三张表，关系表两个外键**）；
3. 所有非主键字段直接依赖主键，不要产生传递依赖。（例如学生班级。**一对多，两张表，多的表加外键**）

一张表拆分：一对一，外键唯一(fk+unique)
有时候为了满足用户需求，会拿冗余换速度（表连接多速度慢），并且开发人员的编写难度也会降低
