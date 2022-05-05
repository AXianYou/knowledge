# 一、mybatis配置：

要使用 MyBatis， 只需将 [mybatis-x.x.x.jar](https://github.com/mybatis/mybatis-3/releases) 文件置于类路径（classpath）中即可。

如果使用 Maven 来构建项目，则需将下面的依赖代码置于 pom.xml 文件中：

```java
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

![image-20220222100657861](D:/java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20220222100657861.png)



# 步骤：

## 一、搭建环境：

​	1·、创建数据库

​	2、创建Maven项目

​	3、导入依赖    在pom.xml中导入相应的依赖

```xml
<!--    导入依赖-->
    <dependencies>
<!--        mysql驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.25</version>
        </dependency>

<!--        mybatis-->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
    </dependencies>


<!--        末尾加上-->

<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>

            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
            </resource>
        </resources>
    </build>
```

## 二、创建一个模块

- 编写mybatis核心文件

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <!--核心配置文件-->
  <configuration>
      <environments default="development">
          <environment id="development">
              <transactionManager type="JDBC"/>
              <dataSource type="POOLED">
                  <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                  <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                  <property name="username" value="root"/>
                  <property name="password" value="zhoubo120011."/>
              </dataSource>
          </environment>
      </environments>
  
  <!--   每一个Mapper.XML 都需要在Mybatis核心配置文件中注册！ -->
  <mappers>
      <mapper resource="com/zhou/dao/UserMapper.xml"></mapper>
  </mappers>
  </configuration>
  ```

- 编写Mybatis工具类

  ```java
  public class MybatisUtils {
      private static SqlSessionFactory sqlSessionFactory;
  
      static{
          try {
              // 使用Mybatis 获取sqlSessionFactory对象
              String resource = "mybatis-config.xml";
              InputStream inputStream = Resources.getResourceAsStream(resource);
              sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
      //    既然有了 SqlSessionFactory，顾名思义，
  //    我们可以从中获得 SqlSession 的实例。
  //    SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
      public static SqlSession getSqlSession(){
          return sqlSessionFactory.openSession();
      }
  
  
  }
  ```

- 编写代码：

  - 实体类

  ```java
  public class User {
      private int id;
      private String name;
      private String pwd;
  
      public User() {
      }
  
      public User(int id, String name, String pwd) {
          this.id = id;
          this.name = name;
          this.pwd = pwd;
      }
  
      public int getId() {
          return id;
      }
  
      public void setId(int id) {
          this.id = id;
      }
  
      public String getName() {
          return name;
      }
  
      public void setName(String name) {
          this.name = name;
      }
  
      public String getPwd() {
          return pwd;
      }
  
      public void setPwd(String pwd) {
          this.pwd = pwd;
      }
  
      @Override
      public String toString() {
          return "User{" +
                  "id=" + id +
                  ", name='" + name + '\'' +
                  ", pwd='" + pwd + '\'' +
                  '}';
      }
  }
  ```
  
  - Dao接口
  
  ```java
  public interface UserMapper {
      /**
       *功能描述: 查询用户集合
       * @return java.util.List<com.zhou.entity.User>
       * @author jiaoqianjin
       * @date 2021/3/3
       */
      List<User> selectUser();
  }
  ```
  
  
  
  - 接口实现类
  
  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
  <!--绑定一个对应的Dao/ Mapper接口-->
  <mapper namespace="com.zhou.dao.UserMapper">
  
  <!--    查询语句-->
      <select id="getUserList" resultType="com.zhou.pojo.User">
          select * from mybatis.user
      </select>
  </mapper>
  ```
  
  - 测试类

```java
public class UserMapperTest {
    @Test
    public void selectUser() {
        SqlSession session = MybatisUtils.getSqlSession();
        UserMapper mapper = session.getMapper(UserMapper.class);
        // 调用SqlSession操作数据库
        List<User> users = mapper.selectUser();

        for (User user: users){
            System.out.println(user);
        }
        // 关闭 SqlSession
        session.close();
    }
}
```









# 二、CRUD

```xml
<!--绑定一个对应的Dao/ Mapper接口-->
<mapper namespace="com.zhou.dao.UserMapper">

<!--    查询语句-->
    <select id="selectUser" resultType="com.zhou.pojo.User">
        select * from mybatis.user
    </select>
</mapper>
```



## 1、namespace：

namespace中的包名要和 Dao/mapper 接口的包名一致

## 2、select、

​	选择、查询语句：

- id ： 就是对应的namespace中的方法名
- resultType ： Sql语句执行的返回值

1
