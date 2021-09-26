# 二刷Spring

## Spring 5
### 框架概述
1. Spring 是轻量级的开源的 JavaEE 框架
2. Spring 可以解决企业应用开发的复杂性
3. 特点
	+ 方便解耦，简化开发
	+ 方便解耦，简化开发
	+ 方便程序测试
	+ 方便和其他框架进行整合
	+ 方便进行事务操作
	+ 降低 API 开发难度
4. **核心：IOC，AOP**
	- IOC：控制反转，把创建对象过程交给 Spring 进行管理
	- Aop：面向切面，不修改源代码进行功能增强

### IOC概念和原理
#### 什么是IOC
+ 控制反转，把对象创建和对象之间的调用过程，交给 Spring 进行管理.
+ 使用 IOC 目的：为了耦合度降低

#### IOC 底层原理
xml 解析、工厂模式、反射

#### IOC原理案例场景
我们现在试想一个场景，有两个类，分别是`UserService`,`UserDao`。  
![](https://gitee.com/lonercci/picbed/raw/master/img/202109242200727.png)  
如今我们想在`UserService`中调用`UserDao`中的`add`方法，该怎么做呢。

##### 原始方法
使用new关键字新建一个对象实例，这样实例中就可以使用add方法了。但耦合度太高。  
![](https://gitee.com/lonercci/picbed/raw/master/img/202109242209619.png)

##### 工厂模式
使用工厂模式，把`UserService`,`UserDao`做到了解耦，但是却和`UserFactory`有了耦合度。  
![](https://gitee.com/lonercci/picbed/raw/master/img/202109242215073.png)  
实际上，我们会发现不管怎么设计，都会存在一定的耦合度。因此，我们需要做的就是怎么把耦合度降到最低。

##### IOC模式（使用xml文件解析+反射）
+ 第一步：创建xml配置文件，配置创建的对象
```xml
<bean id="dao" class="com.lone.UserDao"></bean>
```
+  第二步：有service类和dao类，创建工厂类，以下是用伪代码实现的过程
```java
public class UserFactory {
    public static UserDao getDao(){
        // 1.xml解析,得到class属性值即全路径+类名
        String classvalue = class属性值;
        // 2.通过反射，newInstance方法创建对象
        Class clazz = Class.forName(classvalue);
        return (UserDao) clazz.newInstance();
    }
}
```
我们会发现，这时候我们想要更改路径，只需要更改xml文件即可，对比前2种方法，进一步降低了耦合度。

#### IOC的一些接口
IOC 思想基于 IOC 容器完成，IOC 容器底层就是对象工厂。
Spring 提供 IOC 容器实现的两种方式：
1. BeanFactory
    IOC 容器基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用。 **加载配置文件时候不会创建对象，在获取对象（使用）才去创建对象**
2. ApplicationContext
    是BeanFactory 接口的子接口，提供更多更强大的功能，一般由开发人
    员进行使用。没错，这个就是我们经常在写测试类的时候使用的的那个接口。**加载配置文件时候就会把在配置文件对象进行创建**  
    ![](https://gitee.com/lonercci/picbed/raw/master/img/202109251524576.png)  

`ApplicationContext` 接口的实现类
第一个是系统中的绝对路径读取bean.xml，第二个是相对路径（src下的路径）  
![](https://gitee.com/lonercci/picbed/raw/master/img/202109251533024.png)  

### IOC操作Bean管理
什么是Bean 管理？Bean 管理是指两个操作：
1. Spring 创建对象（xml文件）
2. Spirng 注入属性
#### Bean 管理操作 - 基于 xml 配置文件方式实现
##### 基于 xml 方式创建对象
```xml
<!--1 配置User对象创建-->
<bean id="user" class="com.lone.User"></bean>
```
+ 在 spring 配置文件中，使用 bean 标签，标签里面添加对应属性，就可以实现对象创建
+ 在 bean 标签有很多属性，介绍常用的属性
  - id 属性：唯一标识
  - class 属性：类全路径（包类路径）
+ 创建对象时候，默认也是执行无参数构造方法完成对象创建
##### 基于 xml 方式注入属性
DI：依赖注入，就是注入属性  
![](https://gitee.com/lonercci/picbed/raw/master/img/202109251703463.png)  

+ 第一种注入方式：**使用 set 方法进行注入**

1. 创建类，定义属性和对应的 set 方法

```java
/**
 * 演示使用set方法进行注入属性
 */
public class Book {
    //创建属性
    private String bname;
    private String bauthor;
    //创建属性对应的set方法
    public void setBname(String bname) {
        this.bname = bname;
    }
    public void setBauthor(String bauthor) {
        this.bauthor = bauthor;
    }
}
```

2. 在 spring 配置文件配置对象创建，配置属性注入

```xml
<!--2 set方法注入属性-->
    <bean id="book" class="com.lone.Book">
        <!--使用property完成属性注入
            name：类里面属性名称
            value：向属性注入的值
        -->
        <property name="bname" value="Java"></property>
        <property name="bauthor" value="Vue"></property>
   </bean>
```

+ 第二种注入方式：**使用有参数构造进行注入**
1. 创建类，定义属性，创建属性对应有参数构造方法
```java
/**
 * 使用有参数构造注入
 */
public class Orders {
    //属性
    private String oname="";
    private String address;
    //有参数构造
    public Orders(String oname,String address) {
        this.oname = oname;
        this.address = address;
    }
    public void ordersTest() {
        System.out.println(oname+"::"+address);
    }
}

```
2. 在 spring 配置文件中进行配置
```java
<!--3 有参数构造注入属性-->
<bean id="orders" class="com.lone.Orders">
     <constructor-arg name="oname" value="电脑"></constructor-arg>
     <constructor-arg name="address" value="China"></constructor-arg>
</bean>
```

+ 第三种注入方式：**p 名称空间注入**
使用 p 名称空间注入，可以简化基于 xml 配置方式
1. 添加 p 名称空间在配置文件中
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```
2. 进行属性注入，在 bean 标签里面进行操作
```java
<bean id="book" class="com.lone.Book" p:bname="Java" p:bauthor="Lone"></bean>
```

##### xml注入其他类型属性
1. 字面量
```xml
<!--null 值-->
<property name="address">
	<null/>
</property>
```
2. 属性值包含特殊符号
```xml
<!--属性值包含特殊符号
 1 把<>进行转义 &lt; &gt;
 2 把带特殊符号内容写到 CDATA
-->
<property name="address">
     <value><![CDATA[<<南京>>]]></value>
</property>
```
3. 注入属性 - 外部 bean

    ![](https://gitee.com/lonercci/picbed/raw/master/img/202109251751812.png)

  1. 创建两个类 service 类和 dao 类

  2. 在 service 调用 dao 里面的方法

  3. 在 spring 配置文件中进行配置

== 代码部分 ==
UserService：
```java
public class UserService {

    //创建UserDao类型属性，生成set方法
    private UserDao userDao;
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add() {
        System.out.println("service add...............");
        userDao.update();
    }
}
```

UserDao:
```java
public interface UserDao {
    public void update();
}
```

UserDaoImpl：
```java
public class UserDaoImpl implements UserDao {
    @Override
    public void update() {
        System.out.println("dao update...........");
    }
}
```
bean.xml:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--1 service和dao对象创建-->
    <bean id="userService" class="com.lone.service.UserService">
        <!--注入userDao对象
            name属性：类里面属性名称
            ref属性：创建userDao对象bean标签id值
        -->
        <property name="userDao" ref="userDaoImpl"></property>
    </bean>
    <bean id="userDaoImpl" class="com.lone.dao.UserDaoImpl"></bean>
</beans>
```

4. 注入属性-内部 bean（举例）
  + 一对多关系：部门和员工
  + 一个部门有多个员工，一个员工属于一个部门，部门是一，员工是多
  + 在实体类之间表示一对多关系，员工表示所属部门，使用对象类型属性进行表示

Dept.java
```java
//部门类
public class Dept {
    private String dname;
    public void setDname(String dname) {
        this.dname = dname;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "dname='" + dname + '\'' +
                '}';
    }
}
```

Emp.java
```java
//员工类
public class Emp {
    private String ename;
    private String gender;
    //员工属于某一个部门，使用对象形式表示
    private Dept dept;

    public void setDept(Dept dept) {
        this.dept = dept;
    }
    public void setEname(String ename) {
        this.ename = ename;
    }
    public void setGender(String gender) {
        this.gender = gender;
    }
    public void add() {
        System.out.println(ename+"::"+gender+"::"+dept);
    }
}

```

bean.xml（内部Bean）
```xml
<!--内部 bean-->
<bean id="emp" class="com.lone.bean.Emp">
   <!--设置两个普通属性-->
    <property name="ename" value="lucy"></property>
    <property name="gender" value="女"></property>
    <!--设置对象类型属性-->
    <property name="dept">
    <bean id="dept" class="com.lone.bean.Dept">
        <property name="dname" value="安保部"></property>
    </bean>
    </property>
</bean>
```
4. 注入属性 - 级联赋值
+ 第一种写法
```xml
<!--级联赋值-->
<bean id="emp" class="com.lone.bean.Emp">
    <!--设置两个普通属性-->
    <property name="ename" value="lucy"></property>
    <property name="gender" value="女"></property>
    <!--级联赋值-->
    <property name="dept" ref="dept"></property>
</bean>
<bean id="dept" class="com.lone.bean.Dept">
    <property name="dname" value="财务部"></property>
</bean>

```
+ 第二种写法  
![](https://gitee.com/lonercci/picbed/raw/master/img/202109252133214.png)

```xml
<!--级联赋值-->
<bean id="emp" class="com.lone.bean.Emp">
    <!--设置两个普通属性-->
    <property name="ename" value="lucy"></property>
    <property name="gender" value="女"></property>
    <!--级联赋值-->
    <property name="dept" ref="dept"></property>
    <property name="dept.dname" value="技术部"></property>
</bean>
<bean id="dept" class="com.lone.bean.Dept">
    <property name="dname" value="财务部"></property>
</bean>
```

##### xml 注入集合属性
+ 注入数组类型属性
+ 注入 List 集合类型属性
+ 注入 Map 集合类型属性

**代码案例**
1. 创建类，定义数组、list、map、set 类型属性，生成对应 set 方法
```java
public class Stu {
    //1 数组类型属性
    private String[] courses;
    //2 list集合类型属性
    private List<String> list;
    //3 map集合类型属性
    private Map<String,String> maps;
    //4 set集合类型属性
    private Set<String> sets;

    public void setCourseList(List<Course> courseList) {
        this.courseList = courseList;
    }

    public void setSets(Set<String> sets) {
        this.sets = sets;
    }
    public void setList(List<String> list) {
        this.list = list;
    }
    public void setMaps(Map<String, String> maps) {
        this.maps = maps;
    }
}

```
2. 在 spring 配置文件进行配置
```xml
<!--1 集合类型属性注入-->
<bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
    <!--数组类型属性注入-->
    <property name="courses">
        <array>
            <value>java课程</value>
            <value>数据库课程</value>
        </array>
    </property>
    <!--list类型属性注入-->
    <property name="list">
        <list>
            <value>张三</value>
            <value>小三</value>
        </list>
    </property>
    <!--map类型属性注入-->
    <property name="maps">
        <map>
            <entry key="JAVA" value="java"></entry>
            <entry key="PHP" value="php"></entry>
        </map>
    </property>
    <!--set类型属性注入-->
    <property name="sets">
        <set>
            <value>MySQL</value>
            <value>Redis</value>
        </set>
    </property>
    <!--注入list集合类型，值是对象-->
    <property name="courseList">
        <list>
            <ref bean="course1"></ref>
            <ref bean="course2"></ref>
        </list>
    </property>
</bean>
```

##### xml 注入集合 - (在集合里面设置对象类型值)

```xml
<!--创建多个course对象-->
<bean id="course1" class="com.lone.collectiontype.Course">
    <property name="cname" value="Spring5框架"></property>
</bean>
<bean id="course2" class="com.lone.collectiontype.Course">
    <property name="cname" value="MyBatis框架"></property>
</bean>

<!--注入list集合类型，值是对象-->
<property name="courseList">
    <list>
        <ref bean="course1"></ref>
        <ref bean="course2"></ref>
    </list>
</property>
```

##### xml 注入集合 - (把集合注入部分提取出来)
1. 在 spring 配置文件中引入名称空间 util
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
```
2. 使用 util 标签完成 list 集合注入提取
```xml
<!--1 提取list集合类型属性注入-->
<util:list id="bookList">
    <value>易筋经</value>
    <value>九阴真经</value>
    <value>九阳神功</value>
</util:list>

<!--2 提取list集合类型属性注入使用-->
<bean id="book" class="com.lone.collectiontype.Book" scope="prototype">
    <property name="list" ref="bookList"></property>
</bean>
```

##### FactoryBean
1. Spring 有两种类型 bean，一种普通 bean，另外一种工厂 bean（FactoryBean）
2. 普通 bean：在配置文件中定义 bean 类型就是返回类型
3. 工厂 bean：在配置文件定义 bean 类型可以和返回类型不一样

第一步 创建类，让这个类作为工厂 bean，实现接口 FactoryBean
第二步 实现接口里面的方法，在实现的方法中定义返回的 bean 类型

```java
public class MyBean implements FactoryBean<Course> {

    //定义返回bean
    @Override
    public Course getObject() throws Exception {
        Course course = new Course();
        course.setCname("abc");
        return course;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

```xml
<bean id="myBean" class="com.lone.factorybean.MyBean">
```

```java
@Test
public void test3() {
    ApplicationContext context =
        new ClassPathXmlApplicationContext("bean3.xml");
    Course course = context.getBean("myBean", Course.class);
    System.out.println(course);
}
```
##### bean 作用域
1. 在 Spring 里面，设置创建 bean 实例是单实例还是多实例

2. 在 Spring 里面，默认情况下，bean 是单实例对象

3. 如何设置单实例还是多实例
   （1）在 spring 配置文件 bean 标签里面有属性（scope）用于设置单实例还是多实例

   （2）scope 属性值：

   + 在 spring 配置文件 bean 标签里面有属性（scope）用于设置单实例还是多实例
   + scope 属性值
```xml
<bean id="book" class="com.lone.collectiontype.Book" scope="prototype">
    <property name="list" ref="bookList"></property>
</bean>
```
4. singleton 和 prototype 区别
   + singleton 单实例，prototype 多实例
   + 设置 scope 值是 singleton 时候，加载 spring 配置文件时候就会创建单实例对象
   + 设置 scope 值是 prototype 时候，不是在加载 spring 配置文件时候创建 对象，在调用 getBean 方法时候创建多实例对象

##### bean 生命周期
1. 生命周期
   （1）从对象创建到对象销毁的过程
   
2. bean 生命周期
   （1）通过构造器创建 bean 实例（无参数构造） 
   （2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法） 
   （3）调用 bean 的初始化的方法（需要进行配置初始化的方法） 
   （4）bean 可以使用了（对象获取到了） 
   （5）当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法）

##### xml 自动装配
1. 什么是自动装配

   （1）根据指定装配规则（属性名称或者属性类型），Spring 自动将匹配的属性值进行注入

2. 演示自动装配过程

   + （1）根据属性名称自动注入
   + （2）根据属性类型自动注入

#### Bean 管理操作 - 基于注解方式实现


