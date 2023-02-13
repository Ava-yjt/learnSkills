#Mysql

Mysql默认启动端口号是3306


命令不区分大小写
net stop MySQL
net start MySQL
mysql -u root -p
show databases
create database XXX
use database XXX
exit

##DQL

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
*先分组再处理，一张表默认为一组分组函数自动忽略null；分组函数不能直接使用在where语句*
count(字段名) 统计不为null的  count(*) 统计表的总行数
sum(字段名)
avg(字段名)
max(字段名)
min(字段名)
