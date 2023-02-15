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

##DQ（原表数据不会修改）

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
is null
and or 
in (,,) not in(,,)
not not
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
trim(" XXX) 去空格
str_to_date
date_format 格式化日期
format 设置千分位
round(字面量，保留小数位数) 四舍五入
rand() 生成随机数
ifnull(空的数据，替换值) 将null转为具体值
case .. when .. then .. when .. then .. else .. end 

###分组函数（多行处理函数）
**必须先分组在使用分组函数，一张表默认为一组**
*分组函数自动忽略null；分组函数不能直接使用在where语句h后*

count(字段名) 统计不为null的  count(*) 统计表的总行数
sum(字段名)
avg(字段名)
max(字段名)
min(字段名)

###分组查询
```
select
    ...
from
    ...
where
    ... 
group by
    ...
order by
    ...
having
    ...
```
 
**执行顺序：from, where, group by, habing, select, order by**
select语句中如果有分组，select后只能加分组的字段和分组函数
使用having可以对分组后的数据进一步过滤，但不能单独使用，不能替代where

**去除重复记录**
select distinct ... from ...
distinct 只能放在所有字段的前端，表示联合去重

###连接查询
1. 内连接
   >避免笛卡尔积现象；把完全能匹配上这个连接的数据查询出来，两种表没有主次关系
   - 等值连接
    ```
    //找出员工的薪资等级
    select
        e.ename, d.dname
    from
        emp e
    (inner) join
        dept d  //给表起别名加效率
    on
        e.deptno = d.deptno   //e 和 d的连接体条件
    ```
   - 非等值连接
     on e.salary between s.losa and a.hisa
   - 自连接
    ```
    //找出员工的上级
    select
        a.ename as '员工名', b.ename as '领导名'
    from
        emp a
    right join
        emp b  //给表起别名加效率
    on
        e.mgr = b.empno   //e 和 d的连接条件
    ```
2. 外连接
   - 右外连接 将join右边的表中数据全部查询出来，捎带着左边的次表
    ```
    //找出员工的上级
    select
        e.ename, d.dname
    from
        emp e
    right outer join
        dept d  //给表起别名加效率
    on
        e.deptno = d.deptno   //e 和 d的连接体条件
    ```
   - 左外连接 left outer

    ```
    //多张表的连接
    select
        ...
    from
        a
    (inner) join
        b
    on
        b 和 b的连接条件
    join
        c
    on
        a 和 c的连接条件
    right join
        d
    on
        a 和 d的连接条件
    ```

3. 全连接


##主键PK

每张表都必须有，主键值不为NULL且不能重复，一般是数字（int bigint char），定长，不需要有意义；一张表只能添加一个主键约束
单一主键
id int primary key
复合主键（表级约束）
primary key (id,name)

主键自增，从1开始                   
id int primary key auto_increment

自然主键（自然数，使用更多），业务主键

###外键约束FK
子表中的外键引用的父表中某个字段不一定是主键，但必须具有唯一性
外键值可以是NULL
cno int,
foreigh key(cno) references t_class(classno)

##数据库设计的三范式
避免表中数据冗余，空间浪费。
1. 任何一张表必须有主键，每一个字段原子性不可再分（字段要精细）；
2. 所有非主键字段完全依赖主键，不要产生部份依赖（例如学生教师，复合主键数据冗余。**多对多，三张表，关系表两个外键**）；
3. 所有非主键字段直接依赖依赖，不要产生传递依赖。（例如学生班级。**一对多，两张表，多的表加外键**）

一张表拆分：一对一，外键唯一(fk+unique)
有时候为了满足用户需求，会拿冗余换速度（表连接多速度慢），并且开发人员的编写难度也会降低

#Mysql连接