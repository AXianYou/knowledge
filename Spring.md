# 一、介绍

##  1.1优点

- Spring是一个开源的免费的框架（容器）
- Spring是一个轻量级、非侵入式的框架
- 控制反转（IOC）、面向切面编程（AOP）
- 支持事务处理，对框架整合的支持

Spring 就是一个轻量级的 控制反转（IOC）和面型切面编程（AOP）的框架

# 二、IOC理论推导、

Spring 中 beans.xml 股价

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    
    
</beans>
```

1、UserDao 接口

2、UserDaoImpl 接口实现类

3、UserService 业务接口

4、UserServiceImpl 业务实现类

**总结：对象由Spring 来进行创建、管理和装配**

# 三、IOC创建对象的方式

1、使用无参构造创建对象，默认！

```xml
<bean id="user" class="com.zhou.pojo.User">
    <property name="name" value="你好"/>
</bean>
```

2、假设使用有参构造创建对象时

- 第一种：下标赋值

```xml
<bean id="user" class="com.zhou.pojo.User">
    <constructor-arg index="0" value="nihao"/>
</bean>
```

- 第二种：直接通过参数名进行赋值

```xml
<bean id="user" class="com.zhou.pojo.User">
    <constructor-arg name="name" value="你好"/>
</bean>
```

**总结：在配置文件加载的时候，容器中管理的对象就已经开始初始化了**

# 四、依赖注入：

1、构造器注入

beans.xml  头文件约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

2、**set 方式注入（重点）**

- 依赖注入：set注入
  - 依赖  bean对象的创建依赖容器
  - 注入：bean对象的属性依赖容器注入



完善注入信息：

```xml
 <bean id="address" class="com.zhou.pojo.Address"/>
    <bean id="student" class="com.zhou.pojo.Student">
<!--        第一种  普通值注入，  value-->
        <property name="name" value="周博"/>
<!--        第二种   Bean注入，ref -->
        <property name="address" ref="address"/>

<!--        数组注入-->
        <property name="books">
            <array>
                <value>《红楼梦》</value>
                <value>《三国演义》</value>
            </array>
        </property>

<!--        List-->
        <property name="hobbies">
            <list>
                <value>听歌</value>
                <value>敲代码</value>
            </list>
        </property>

<!--        Map-->
        <property name="card">
            <map>
                <entry key="身份证" value="123456742342423"/>
                <entry key="银行卡" value="1234143243253245"/>
            </map>
        </property>

<!--        set  -->
        <property name="games">
            <set>
                <value>振刀</value>
                <value>lol</value>
            </set>
        </property>

<!--        null-->
        <property name="wife">
            <null/>
        </property>

<!--        properties-->
        <property name="info">
            <props>
                <prop key="学号">12344</prop>
                <prop key="性别">男</prop>
            </props>
        </property>
    </bean>
```



3、拓展方式注入

使用xml文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    p命名空间注入，可以直接注入属性的值： property-->
    <bean id="user" class="com.zhou.pojo.User" p:name="周博" p:age="18"/>

<!--   c命名空间注入，通过构造器进行注入：construct-args -->
    <bean id="user2" class="com.zhou.pojo.User" c:name="zhoubo" c:age="20"/>
</beans>
```

注意点：p命名和c命名不能直接使用，需要导入xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
xmlns:c="http://www.springframework.org/schema/c"
```

# 五、bean作用域：

1、单例模型（Spring默认机制）：

```xml
 <bean id="user" class="com.zhou.pojo.User" p:name="周博" p:age="18" scope="singleton"/>
```

2、原型模型：每次从容器中get的时候，都会产生一个新对象

```xml
 <bean id="user" class="com.zhou.pojo.User" p:name="周博" p:age="18" scope="prototype"/>
```

3、其余的request 、session、 application、这些只能在web开发中使用到

# 六、Bean的自动装配

Spring中的三种装配方式：

- 在xml中显示的装配
- 在java中显示装配
- 隐式的自动装配bean 【重点】

## 6.1 测试

- 环境搭建：一个人有两只宠物

## 6.2 自动装配

```xml
<bean id="dog" class="com.zhou.pojo.Dog"/>
<bean id="cat" class="com.zhou.pojo.Cat"/>
<!--    byName： 会自动在容器上下文中查找，和自己对象set方法后面的值对性的bean id-->
<!--    byType： 会自动在容器上下文中查找，和自己对象属性类型相同的bean-->
<bean id="people" class="com.zhou.pojo.People" autowire="byName">
    <property name="name" value="秦将"/>
</bean>
```

小结：

- byName时，需要保证所有的bean的id唯一，并且这个bean需要和自动注入的属性的set方法的值一致
- byType时，需要保证所有bean的class唯一，并且这个bean需要和自动注入的属性的类型一致

## 6.3、使用注解实现自动装配

使用注解须知：

1、导入约束

2、配置注解的支持:  <context:annotation-config/>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

    <beans/>
```

**@Aytowired**

注意：

- 直接在属性上使用即可，也可以在set方式上使用
- 使用Autowired我们可以不用编写Set方法，前提为这个自动装配的属性在IOC（Spring）容器中存在且符合名字byType

​		科普：

```xml
@Nullable  字段标记了这个注解，说明这个字段可以为null
```

如果@Autowired自动装配的环境比较复杂时，自动装配无法通过一个注解@Autowired完成时，我们可以使用@Qualifier(value="xxx")  去配合@Autowired进行使用，完成指定的bean对象注入

使用：

```java
public class People {
    @Autowired
    @Qualifier(value = "dog222")
    private Dog dog;
    @Autowired
    private Cat cat;
    private String name;
}
```

# 七、使用注解开发

1、创建maven项目，导入相关的依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <context:annotation-config/>

</beans>
```



**小结：**

- xml与注解：

  - xml更加万能，适用于任何场合，维护简单方便
  - 注解 不是自己类是用不了，维护相对复杂

  xml与注解最佳实践：

  - xml用来管理bean
  - 注解只负责完成属性的注入

# 八、代理模式：

### 8.1、静态代理：

​	![image-20220302162753808](../../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20220302162753808.png)

角色分析：

- 抽象角色：一般使用接口或抽象类来解决
- 真实角色：被代理的角色
- 代理角色：代理真是角色，代理真实角色后我们一般会做一些附属操作
- 客户：访问代理对象的人



代码步骤：

​	1、接口

```java
// 租房
public interface Rent {
    public void rent();
}
```

​	2、真实角色

```java
//房东
public class Host implements Rent{
    public void rent(){
        System.out.println("房东出租房");
    }

}
```

3、代理角色：

```java
//中介
public class Proxy implements Rent{
    private Host host;

    public Proxy() {
    }

    public Proxy(Host host) {
        this.host = host;
    }

    @Override
    public void rent() {
        host.rent();
    }

    /**
     *功能描述:
     *   中介费
     * @author zhoubo
     * @date 2022/3/2
    */

    public void money(){
        System.out.println("收取中介费");
    }

    public void seeHouse(){
        System.out.println("中介带你看房");
    }
}
```

4、客户端访问角色

```java
//租客
public class Client {
    public static void main(String[] args) {
        Host host = new Host();
        Proxy proxy = new Proxy(host);
        proxy.rent();
    }
}
```



代理模式的好处：

- 可以使真正的角色操作更加纯粹，不用关注一些公共的业务
- 公共业务交给了代理角色，实现了业务的分工
- 公共业务发生扩展时，方便集中管理

缺点：

- 一个真实角色就会产生一个代理角色；代码量会翻倍，开发效率会变低

## 8.2、动态代理：

- 动态代理和静态代理角色一样
- 动态代理的代理类是动态生成的，不由我们直接写好
- 动态代理分为两大类：基于接口的动态代理，基于类的动态代理
  - 基于接口 ---JDK 动态代理
  - 基于类  cglib
  - java字节码实现 ： javasist



了解两个类 ： Proxy（代理类）  InvocationHandler （调用处理程序）

# 九、AOP：

面向切面编程

## 9.1：Aop在Spring中的作用：

**提供声明式事务，允许用户自定义界面**

## 9.2：使用Spring实现Aop

**重点：使用AOP需要导入一个依赖包**

```xml
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.8</version>
        </dependency>
```

- 方式一：使用Spring api接口  【主要是SpringAPI接口实现】

applicationContext.xml

```xml
<!--    方式一：  使用原生的的Spring API接口-->
<!--    配置AOP-->
    <aop:config>
<!--        切入点 : expression : 表达式 ： 要执行的位置-->
        <aop:pointcut id="poincut" expression="execution(* com.zhou.service.UserServiceImpl.*(..))"/>

<!--        执行环绕增加-->
        <aop:advisor advice-ref="log" pointcut-ref="poincut"/>
        <aop:advisor advice-ref="afterLog" pointcut-ref="poincut"/>
    </aop:config>
```

- 方式二：自定义来实现AOP【主要是切面定义】

applicationContext.xml文件

```xml
<!--    方式二-->
    <bean id="diy" class="com.zhou.diy.Diy"/>

    <aop:config>
<!--        自定义切面，ref  接要引用的类-->
        <aop:aspect ref="diy">
<!--            切入点-->
            <aop:pointcut id="point" expression="execution(* com.zhou.service..UserServiceImpl.*(..))"/>
<!--            通知-->
            <aop:before method="before" pointcut-ref="point"/>
            <aop:after method="after" pointcut-ref="point"/>

        </aop:aspect>
    </aop:config>
```

Div.class文件：

```java
public class Diy {
    public void before(){
        System.out.println("方法执行前");
    }

    public void after(){
        System.out.println("执行后");
    }
}
```

# 十、整合Mybatis：

步骤：

​	1、导入相关jar包

- - junit
  - mybatis
  - mysql数据库
  - spring相关的
  - aop植入
  - mybatis-spring 【new】

​	2、编写配置文件

​	3、测试



- 导入相关依赖：

Junit

```xml
<dependency>
   <groupId>junit</groupId>
   <artifactId>junit</artifactId>
   <version>4.12</version>
</dependency>
```

mybatis

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis</artifactId>
   <version>3.5.2</version>
</dependency>
```

mysql-connector-java

```xml
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
   <version>5.1.47</version>
</dependency>
```

spring相关

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-jdbc</artifactId>
   <version>5.1.10.RELEASE</version>
</dependency>
```

aspectJ AOP 织入器

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

mybatis-spring整合包【重点】

```xml
<dependency>
   <groupId>org.mybatis</groupId>
   <artifactId>mybatis-spring</artifactId>
   <version>2.0.2</version>
</dependency>
```

配置Maven 静态资源过滤问题

```xml
<build>
   <resources>
       <resource>
           <directory>src/main/java</directory>
           <includes>
               <include>**/*.properties</include>
               <include>**/*.xml</include>
           </includes>
           <filtering>true</filtering>
       </resource>
   </resources>
</build>
```

## 10.1、回忆mybatis

步骤：

​	1、编写实体类

```java
@Data
public class User {
    private int id;
    private String name;
    private String phone;
}
```

​	2、编写核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <typeAliases>
        <package name="com.zhou.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis_01?useSSL=false&amp;useUnicode=true&amp;characterEncoding=utf8"/>
                <property name="username" value="root"/>
                <property name="password" value="zhoubo120011."/>
            </dataSource>
        </environment>
    </environments>

    <mappers>
        <package name="com.zhou.mapper"/>
    </mappers>
</configuration>
```

​	3、编写接口

```java
public interface UserMapper {
    List<User> selectUser();
}
```

​	4、编写Mapper.xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
	namespace：命名空间，它的作用就是对SQL进行分类化管理，可以理解为SQL隔离
	注意：使用mapper代理开发时，namespace有特殊且重要的作用
 -->

<mapper namespace="com.zhou.mapper.UserMapper">
    <select id="selectUser" resultType="User">
        select * from user
    </select>
</mapper>
```

​	5、测试

```java
public class MyTest {
    @Test
    public void test() throws IOException {
        String resources = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resources);

        SqlSessionFactory build = new SqlSessionFactoryBuilder().build(inputStream);
        SqlSession sqlSession = build.openSession();

        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.selectUser();

        for (User user : userList) {
            System.out.println(user);
        }
        sqlSession.close();

    }
}
```

## 10.2、 Mybatis-Spring 

