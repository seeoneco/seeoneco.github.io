# SSM-框架整合全记录



# SSM-框架整合全记录


>   此篇文章主要为了记录 SSM 框架的学习过程，方便以后查阅复习

## 关于SSM

顾名思义，Spring + SpringMVC +Mybatis 整合在一起就是SSM了。说到整合，那么不可避免的就是把他们三个框架综合在一起，使用 Spring 来管理所有的bean。下面我们通过一个小项目来详细记录过程。

## 数据库环境

使用MySQL：

```mysql
CREATE DATABASE `ssmbuild`;
USE `ssmbuild`;
DROP TABLE IF EXISTS `books`;
CREATE TABLE `books` (
`bookID` INT(10) NOT NULL AUTO_INCREMENT COMMENT '书id',
`bookName` VARCHAR(100) NOT NULL COMMENT '书名',
`bookCounts` INT(11) NOT NULL COMMENT '数量',
`detail` VARCHAR(200) NOT NULL COMMENT '描述',
KEY `bookID` (`bookID`)
) ENGINE=INNODB DEFAULT CHARSET=utf8
INSERT INTO `books`(`bookID`,`bookName`,`bookCounts`,`detail`)VALUES
(1,'Java',1,'从入门到放弃'),
(2,'MySQL',10,'从删库到跑路'),
(3,'Linux',5,'从进门到进牢');
```

## 基本环境搭建

### 一、新建一个Maven项目，并添加Web项目支持

### 二、导入相关的pom依赖

安装依赖包：

```xml
<dependencies>
        <!--Junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <!--数据库驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.47</version>
        </dependency>
        <!-- 数据库连接池 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.3</version>
        </dependency>
        <!--Servlet - JSP -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!--Mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.2</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            <version>2.0.2</version>
        </dependency>
        <!--Spring-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.1.9.RELEASE</version>
        </dependency>
        <!--AOP支持,后来没有用上-->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
        <!--偷懒，使用lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.10</version>
        </dependency>
</dependencies>
```

设置资源过滤器：很重要！
```xml
<!--资源过滤器-->
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

### 三、建立基本结构和配置框架

#### 1、建立四层结构

把这四个包创建好，里面先不急着写东西

+ com.lone.pojo

    pojo层是对应的数据库表的实体类。

+ com.lone.dao

    Dao层主要是做数据持久层的工作，负责与数据库进行联络的一些任务都封装在此。Dao层的设计首先是设计Dao的接口，然后在Spring的配置文件中定义此接口的实现类，然后就可在模块中调用此接口来进行数据业务的处理，而不用关心此接口的具体实现类是哪个类，显得结构非常清晰，Dao层的数据源配置，以及有关数据库连接的参数都在Spring的配置文件中进行配置。


+ com.lone.service

    Service层主要负责业务模块的逻辑应用设计。同样是首先设计接口，再设计其实现的类，接着再Spring的配置文件中配置其实现的关联。这样我们就可以在应用中调用Service接口来进行业务处理。Service层的业务实现，具体要**调用到已定义的Dao层的接口**，封装Service层的业务逻辑有利于通用的业务逻辑的独立性和重复利用性，程序显得非常简洁。


+ com.lone.controller

    Controller层负责具体的业务模块流程的控制，在此层里面要调用Service层的接口来控制业务流程，控制的配置也同样是在Spring的配置文件里面进行，针对具体的业务流程，会有不同的控制器，我们具体的设计过程中可以将流程进行抽象归纳，设计出可以重复利用的子单元流程模块，这样不仅使程序结构变得清晰，也大大减少了代码量。
    
    
#### 2、写配置文件

创建好mybatis和spring的配置文件，把基础内容写上
+ mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```

+ applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
```

好，到目前为止，我们基本上算是完成了一个空白项目的雏形。

### 四、Mybatis层

#### 1、创建数据库配置文件 database.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssmbuild?useSSL=true&useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=123456
```

在后面，我们会读取这个文件里数据库的连接配置

#### 2、使用IDEA关联数据库

![](https://gitee.com/lonercci/picbed/raw/master/img/20210726230235.png)

#### 3、编写MyBatis的核心配置文件

mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 配置数据源，交给Spring去做 -->

    <typeAliases>
        <package name="com.lone.pojo"/>
    </typeAliases>

</configuration>
```
+ `typeAliases` : 类型别名。`<typeAliases>`扫描实体类的包，它的默认别名就为这个类的 类名，首字母小写。

#### 4、编写数据库对应的实体类

com.lone.pojo.Books：

使用lombok插件，简化实体类的有参无参构造等

```java
package com.lone.pojo;

import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@AllArgsConstructor 
@NoArgsConstructor
public class Books {
    private int bookID;
    private String bookName;
    private int bookCounts;
    private String detail;
}

```

`@AllArgsConstructor ` ：有参构造

`@NoArgsConstructor` : 无参构造 

#### 5、编写Dao层的接口（接口名一般为xxxMapper）

我们要实现增删改查功能，需要使用接口来实现这些方法，这样后面的类就可以重写这些方法，实现复用。接口不需要返回值

```java
package com.lone.dao;

import com.lone.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;


public interface BookMapper {
    /*
    这里实现的是一个BookMapper接口，里面定义了5个方法，传递的是不同类型的参数

    由于增，删，更新数据库返回的是受影响的行数 所以可以使用int类型
    查询一本书，返回的是对象，这个对象是books实体类，可见在写Mapper之前就要把实体类创建好
    查询全部的书，使用的是列表，<Books>代表泛型,意味着只有Books对象才能放入列表
     */
    //增加一本书
    int addBook(Books books);
    //删除一本书,这里使用@Param注解，告诉BookMapper.xml传递一个参数，中SQL语句#{}直接使用
    int deleteBookById(@Param("bookID") int id);
    //更新一本书
    int updateBook(Books books);
    //查询一本书
    Books queryBookById(@Param("bookID") int id);
    //查询全部的书
    List<Books> queryAllBook();
}
```

这里实现的是一个`BookMapper`接口，里面定义了`5`个方法。
传递的是不同类型的参数，
由于增，删，更新数据库返回的是受影响的行数 所以可以使用int类型
查询一本书，返回的是对象，这个对象是`books`实体类，（可见在写`Mapper`之前就要把实体类创建好）
查询全部的书，使用的是列表，`<Books>`代表泛型,意味着只有`Books`对象才能放入列表
使用@Param注解，就能告诉BookMapper.xml传递的参数是谁，在参数多于一个的时候，必须要加上。

#### 6、编写与接口对应的 Mapper.xml 文件
需要导入MyBatis的包
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- 一个mapper对应一个接口，所以每个mapper都需要一个namespace -->
<mapper namespace="com.lone.dao.BookMapper">
    <!--
    每一个标签，对照接口里面的方法名，id为方法名，parameterType是传入的参数类型
    最好写的时候，对照着接口写
    -->

    <insert id="addBook" parameterType="Books">
        /*这里不用添加主键，主键设置了自增，bookID会自己添加*/
        insert into ssmbuild.books(bookName, bookCounts, detail)
        /*这个对应实体类中的字段*/
        values (#{bookName},#{bookCounts},#{detail});
    </insert>

    <delete id="deleteBookById" parameterType="int">
        delete from ssmbuild.books where bookID = #{bookID}
    </delete>

    <update id="updateBook" parameterType="Books">
        update ssmbuild.books
        set bookName = #{bookName},
            bookCounts = #{bookCounts},
            detail = #{detail}
        where bookID = #{bookID} ;
    </update>

    <select id="queryBookById" resultType="Books">
        select *
        from ssmbuild.books
        where bookID = #{bookID};
    </select>

    <select id="queryAllBook" resultType="Books">
        select *
        from ssmbuild.books;
    </select>

    <!--在这些写完之后，赶紧把mapper绑定到mybatis-config.xml配置文件中,称之为 注册 -->
</mapper>
```
由于`Mapper`接口与这里`.xml`是一一对应的关系，所以写`xml`的时候，最好这两个对照着写。
`mapper namespace="com.lone.dao.BookMapper"`进行绑定。
每一个标签，对照接口里面的方法名，`id`为方法名，`parameterType`是传入的参数类型。
在查询方法中，最常用的是`resultType`,其余则是`parameterType`。
SQL语句写完了，赶紧把这里的`.xml`文件绑定到资源目录下的`mybatis-config.xml`，于是配置文件就成了这个样子
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 配置数据源，交给Spring去做 -->

    <typeAliases>
        <package name="com.lone.pojo"/>
    </typeAliases>

    <!--两个名字一样，就使用class,不一样的使用resource-->
    <mappers>
        <mapper class="com.lone.dao.BookMapper"/>
    </mappers>

</configuration>
```
`mappers`：映射器，对我们刚在dao层写好的.xml文件进行绑定
有两种绑定方式：  
【方式一】
```xml
<mappers>
    <mapper resource="com/lone/dao/BookMapper.xml"/>
</mappers>
```
【方式二】
```xml
<mappers>
    <mapper class="com.lone.dao.BookMapper"/>
</mappers>
```
使用方式一idea没有代码提示，使用方式二在接口和xml文件同名的情况下很方便。

#### 7、编写Service层的接口和实现类

接口：与dao.BookMapper里的一样

```java
package com.lone.service;

import com.lone.pojo.Books;
import org.apache.ibatis.annotations.Param;

import java.util.List;

public interface BookService {
    //增加一本书
    int addBook(Books books);
    //删除一本书
    int deleteBookById(int id);
    //更新一本书
    int updateBook(Books books);
    //查询一本书
    Books queryBookById(int id);
    //查询全部的书
    List<Books> queryAllBook();
}

```

接口的实现类：

```java
package com.lone.service;

import com.lone.dao.BookMapper;
import com.lone.pojo.Books;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;


public class BookServiceImpl implements BookService{
    // 业务层调dao层 （service层调dao层）组合Dao
    private BookMapper bookMapper;
    // 首先来一个set方法，等会Spring就可以直接接管了，spring注入

    public void setBookMapper(BookMapper bookMapper) {
        this.bookMapper = bookMapper;
    }


    public int addBook(Books books) {
        return bookMapper.addBook(books);
    }

    public int deleteBookById(int id) {
        return bookMapper.deleteBookById(id);
    }

    public int updateBook(Books books) {
        return bookMapper.updateBook(books);
    }

    public Books queryBookById(int id) {
        return bookMapper.queryBookById(id);
    }

    public List<Books> queryAllBook() {
        return bookMapper.queryAllBook();
    }
}
```

在`BookServiceImpl`继承了`BookService`接口之后，会重写接口内的方法

这里我们需要**调用dao层代码**，拿到接口对象 `bookMapper`，返回重写的方法。

那我们怎么把这个类交给Spring接管呢？ 简单，只要给`bookMapper`一个`set`方法就好了，即可实现Spring的注入。

>   **到此，我们底层的部分就全部写完了，剩下的就要交给Spring+SpringMVC了**，看看我们这个阶段都做了什么事情：导入数据库 => 建立新项目=>配置空白的Mybatis+Spring配置文件 => 编写实体类 => 编写dao层的接口 => 编写dao的xml文件 => 绑定到mybatis配置文件 => 编写service层代码

### 五、Spring层

#### 1、配置Spring整合MyBatis

我们这里数据源使用c3p0连接池

#### 2、编写Spring整合Mybatis的相关的配置文件

 spring-dao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 1、关联数据库配置文件 -->
    <context:property-placeholder location="classpath:database.properties"/>

    <!-- 2、连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!-- c3p0连接池的私有属性 -->
        <property name="maxPoolSize" value="30"/>
        <property name="minPoolSize" value="10"/>
        <!-- 关闭连接后不自动commit -->
        <property name="autoCommitOnClose" value="false"/>
        <!-- 获取连接超时时间 -->
        <property name="checkoutTimeout" value="10000"/>
        <!-- 当获取连接失败重试次数 -->
        <property name="acquireRetryAttempts" value="2"/>
    </bean>

    <!-- 3、SQLSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!-- 4.配置dao接口的扫描包，动态的实现了Dao接口可以注入到Spring容器中 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--注入 sqlSessionFactory -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <!--扫描Dao包-->
        <property name="basePackage" value="com.lone.dao"/>
    </bean>
</beans>

```

在`spring-dao.xml`文件中，关联数据库文件 => 连接池连接数据库配置 => SQLSessionFactory => 配置dao接口的扫描包，实现动态注入。

#### 3、 Spring整合service层

#### 4、编写Spring整合Mybatis的相关的配置文件

spring-service.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 1、扫描service下包里面的注解 -->
    <context:component-scan base-package="com.lone.service"/>

    <!-- 2、将我们所有的业务类，注入到Spring，可以配置或者注解实现 -->
    <bean id="bookServiceImpl" class="com.lone.service.BookServiceImpl">
        <!--[1]看底部注释-->
        <property name="bookMapper" ref="bookMapper"/>
    </bean>

    <!--3、声明式事务配置-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源-->
        <property name="dataSource" ref="dataSource"/>
    </bean>
</beans>
```

【注】

+   这里能使用`ref="bookMapper"`引用是因为在`spring-dao.xml`中,配置了`dao`接口扫描包，会将`dao`下的接口自动注入到`spring`容器，并且引用名称默认是接口的首字母小写
+   但是前提是必须把applicationContext.xml，spring-dao.xml，spring-service.xml放在同一个ApplicationContext下，即idea自动帮你把几个spring的配置整合了，否则会爆红,还有几种方法则是使用import导入前面的配置文件：
    【方法一】在此文件前面加上`<import resource="classpath:spring-dao.xml"/>`
    【方法二】整合到`applicationContext.xml`
    【方法三】使用注解`@Service`,`@Autowired`

```java
@Service
public class BookServiceImpl implements BookService{
    private BookMapper bookMapper;
    @Autowired
    public void setBookMapper(BookMapper bookMapper) {
         this.bookMapper = bookMapper;
       }
```
#### 5、整合几个Spring配置到一个applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">


    <import resource="classpath:spring-dao.xml"/>
    <import resource="classpath:spring-service.xml"/>
    <import resource="classpath:spring-mvc.xml"/>

</beans>

```

>   至此，我们的Spring层也就搞定了，实质上Spring就相当于一个大杂烩的容器，啥都可以放进去


