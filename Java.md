#Java基础
##基础语法
###类、方法定义

类名首字母大写。一个Java源文件可以包含多个类的定义，但只能定义一个public类，且public类名必须与文件名一致。

定义方法的语法是：
```
修饰符 方法返回类型 方法名(方法参数列表) {
    若干方法语句;
    return 方法返回值;
}
```

###访问修饰符
1. public（能够被任何其他类访问）
2. private（只能被所属类访问）
3. protected（同一包的继承树内部） 
4. default（同一包内可见）
###非访问修饰符
1. static 修饰符，静态变量只有一份拷贝,局部变量不能被声明为 static 变量。静态方法从参数列表得到数据,不能使用类的非静态变量。

2. final 修饰符，**final 修饰的类不能够被继承**，修饰的方法不能被继承类重新定义（覆写），修饰的变量为常量，是不可修改的。

3. abstract 修饰符，用来创建抽象类和抽象方法。

4. synchronized 和 volatile 修饰符，主要用于线程的编程.



```java
/**
 * 可以用来自动创建文档的注释
 */
package sample; // 申明包名sample

public class Hello {
    public static void main(String[] args) {
        // 向屏幕输出文本:
        System.out.println("Hello, world!");
        /* 多行注释开始
        注释内容
        注释结束 */
    }
} // class定义结束
```


###实例化方法
```java
Object referenceVariable = new Constructor();
//引用类型 变量 = XX类型的实例
```

数组声明定义
```java
double[] list = {0,0,0};
String [][] s = new String[2][];
```

内部类的实例化
```java
Outer.Inner inner = outer.new Inner();
```

输入
```java
Scanner scanner = new Scanner(System.in); // 创建Scanner对象
String name = scanner.nextLine(); // 读取一行输入并获取字符串
```

输出
```java
System.out.println()
System.out.print()
```
通过使用占位符% 格式化输出`System.out.printf()`，

判断
```java
if (条件) {
    // 条件满足时执行
}
```
switch分支
```java
switch (option) {
case 3:
    ...
    break;
case 2:
    ...
    break;
case 1:
    ...
    break;
}
```

while条件循环
```java
while (条件表达式) {
    循环语句
}
// 继续执行后续代码
```

```java
do {
    执行循环语句
} while (条件表达式);
```

for循环
```java
for (初始条件; 循环检测条件; 循环后更新计数器) {
    // 执行语句
}
```
##面向对象
###构造方法
```java

public class Main {
    public static void main(String[] args) {
        Person p = new Person(); // 编译错误:找不到这个构造方法。
    }
}

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name) {
        this(name, 18); // 调用另一个构造方法Person(String, int)
    }
}
```
实例在创建时通过new操作符会调用其对应的构造方法，构造方法用于初始化实例；

没有定义构造方法时，编译器会自动创建一个默认的无参数构造方法；自定义构造方法后编译器就不再自动创建默认构造方法；

可以定义多个构造方法，编译器根据参数自动判断；

可以在一个构造方法内部通过this调用另一个构造方法，便于代码复用

###继承（代码复用）
extends
如果父类没有默认的构造方法，子类就必须显式调用super()并给出参数
```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```

```java
//向上转型
Student s = new Student();
Person p = s
//向下转型
Student s = new Student();
s instanceof Person;
//先通过instanceof判断一个变量所指向的实例是否是指定类型或这个类型的子类，如果是自动转为该类型
```

###重载(Overload)
重载(overloading) 是在一个类里面，方法名字相同，功能类似，而**参数不同**。返回类型可以相同也可以不同。

每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。

最常用的地方就是构造器的重载。

###覆写（Override）
在继承关系中，子类如果定义了一个与父类方法名字相同，**参数列表和返回值类型都相同**的方法，被称为覆写（Override）。
构造方法不能被重写。
当需要在子类中调用父类的被重写方法时，要使用 super 关键字。

###多态
多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。
###抽象
```java
public class Main {
    public static void main(String[] args) {
        Person p = new Student();
        p.run();
    }
}

abstract class Person {
    public abstract void run();
}

class Student extends Person {
    @Override
    public void run() {
        System.out.println("Student.run");
    }
}
```
把一个方法声明为abstract，表示它是抽象方法，本身没有实现任何方法语句。
抽象方法是无法执行的，包含抽象方法的类也要声明为abstract，并且无法被实例化。

面向抽象编程的本质就是：
上层代码只定义规范（例如：abstract class Person）；
不需要子类就可以实现业务逻辑（正常编译）；
具体的业务逻辑由不同的子类实现，调用者并不关心。

###接口
interface 类名 ；implements
在抽象类中，抽象方法本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现。
```java
interface Person {
    void run();
    String getName();
}

class Student implements Person {
    private String name;

    public Student(String name) {
        this.name = name;
    }

    @Override
    public void run() {
        System.out.println(this.name + " run");
    }

    @Override
    public String getName() {
        return this.name;
    }
}

class Students implements Person, Hello { // 实现了两个interface
    ...
}
```

一个类可以实现多个接口；

接口可以向上转型和向下转型；

接口的所有方法都是抽象方法，不能定义实例字段；

接口可以定义default方法，不必在子类中覆写。

###static静态字段、方法
静态字段属于所有实例“共享空间”的字段，实际上是属于class的字段；
无论修改哪个实例的静态字段，所有实例的静态字段都被修改；

通过类名就可以调用静态方法，不需要实例；
无法访问this，但可以访问静态字段和其他静态方法；

静态方法常用于工具类和辅助方法，如main。

###classpath
JVM通过环境变量classpath决定搜索class的路径和顺序。
在Windows系统上，用;分隔，\连接，带空格的目录用""括起来；Linux系统上，用:分隔，/连接。
推荐在启动JVM时设置classpath变量，不推荐在系统环境变量中设置classpath。
```
java -cp bin（路径） com.itranswarp.sample.Hello（public类名） 
java abc.xyz.Hello
//当前目录可直接省略-cp和bin
```
运行时使用哪个JDK版本，编译时就尽量使用同一版本编译源码。

###jar包
用于存放class的容器，jar包相当于目录，可以包含很多.class文件；
创建方法：右键选择“发送到”，“压缩(zipped)文件夹”，然后把后缀从.zip改为.jar。
jar包里的第一层目录，不能是bin。
MANIFEST.MF文件可以提供jar包的信息，如Main-Class，这样可以直接运行jar包。
执行jar包的class：
```
java -cp ./hello.jar abc.xyz.Hello
java -jar hello.jar
```
###模块
命名规范与包相同
只有module-info.java声明的导出的包，外部代码才被允许访问
```java
module hello.world {            //关键字module 模块的名称
    exports com.itranswarp.sample;  //导出给其他模块使用


    requires java.base;          //依赖的其他模块
	requires java.xml;
}
```

##异常处理

```java
try (resource declaration) {   // 使用的资源
  //代码块
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```
finally 关键字用来创建在 try 代码块后面执行的代码块。
无论是否发生异常，finally 代码块中的代码总会被执行。
在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

try-with-resources 语句可以关闭所有实现 AutoCloseable 接口的资源。