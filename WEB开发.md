###软件准备
1. eclipse
2. node（view）
https://juejin.cn/post/7135687603419873317
3. tomcat(idea内嵌)
4. 依赖：web mybatis Mysql驱动 lombok
5. postman接口测试

##数据库表结构设计

ER图和关系模型是常用的数据库设计工具，ER图可以通过图形化的方式表示实体、属性和它们之间的关系，而关系模型则是通过关系表的方式来表示数据。

将ER图转换为关系模型需要遵循一定的规则，通常有以下步骤：

- 确定实体和实体属性 首先需要识别ER图中的实体，每个实体应该被转换成一个关系表。实体属性应该成为该表的列。
- 识别关系 在ER图中，关系是指实体之间的关系，例如一对多、多对多等。每个关系都应该被转换成一个关系表。如果关系包含属性，则应该添加相应的列。
- 确定主键 每个关系表都需要一个主键，用于唯一标识每个记录。通常情况下，主键可以是单个列或多个列的组合。主键不能为null，也不能重复。
- 确定外键 如果一个关系表依赖于另一个关系表，那么它必须包含另一个表的主键作为外键。外键建立了两个表之间的连接。
- 规范化 规范化是指将关系模型设计为最小化重复和数据冗余的过程。它包括分解表和创建新表以消除冗余数据。
###pom.xml声明依赖
每个依赖节点\<dependency>都由三个子节点组成：

\<groupId> ： 该依赖库所属的组织名称
\<artifactId> ： 依赖的库名
\<version> ： 依赖的库版本
在POM 4中，\<dependency> 中还引入了\<scope> ，它主要管理依赖的部署。目前\<scope> 可以使用5个值：

compile，缺省值，适用于所有阶段，会随着项目一起发布。
provided，类似compile，期望JDK、容器或使用者会提供这个依赖。如servlet.jar。
runtime，只在运行时使用，如JDBC驱动，适用运行和测试阶段。
test，只在测试时使用，用于编译和运行测试代码。不会随项目发布。
system，类似provided，需要显式提供包含依赖的jar，Maven不会在Repository中查找它。
运行mvn compile或者mvn package，Maven会自动下载相关依赖。

###application
引入mybatis信息

###注解
web三层：
controller（@RestController）  service(@Service)  mapper（接口、实现类 @Mapper） 

实体类：
@Data
@NoArgsConstructor
@AllArgsConstructor

controller传参：
@RequestParam(defaultvalue = '')
@RequestBody(json自动封装为JAVA实体)
@PathVariable（路径参数）

##插件
分页插件PageHelper  依赖pagehelper-spring-boot-starter
