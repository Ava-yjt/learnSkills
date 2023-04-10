###软件准备
1. eclipse
2. node（view）
https://juejin.cn/post/7135687603419873317

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