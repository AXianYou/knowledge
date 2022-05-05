# 一、JDBC和数据库连接池：

## 1.JDBC：

- JDBC为访问不同数据库提供了统一的接口，为使用者屏蔽了细节问题
- java程序员使用JDBC，可以连接任何提供了JDB驱动程序的数据库系统，从而完成对数据库的各种操作
- 原理示意图 ：

![image-20211215094027332](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211215094027332.png)

## 2、JDBC API：

### 2.1、

- JDBC API 是一系列的接口，它统一和规范了应用程序和数据库的连接、执行SQL语句，并得到返回结果等各类操作，相关类和接口在java.sql与javaxx.sql包中

### 2.2、小结：

![image-20211217093749748](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211217093749748.png)

![image-20211217094114039](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211217094114039.png)

## 3、JDBC程序编写步骤：

3.1、注册驱动 - 加载Driver 类

3.2、获取链接 - 得到 Connection

3.3、执行增删改查 - 发送相关的SQL 命令给 mysql 执行

3.4、 释放资源 - 关闭连接

> 连接方式有五种，详情查看https://www.bilibili.com/video/BV1fh411y7R8?p=826

- 使用Classname进行自动完成注册

- ```java
      @SuppressWarnings({"all"})
      public static void main(String[] args) throws SQLException, ClassNotFoundException {
          //  1、注册驱动
          Class.forName("com.mysql.cj.jdbc.Driver");
          //
          String url = "jdbc:mysql://localhost:3306/zb_db02?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8";
          // 前面两个是固定的   后面的根据实际情况进行确定
          String user = "root";
          String password = "zhoubo120011.";
          //  2、得到链接
          Connection connection = DriverManager.getConnection(url, user, password);
  //        3 、得到Statement
          Statement statement = connection.createStatement();
          // 4.组织语言
          String sql = "select id,name,sex,borndate from actor";
          ResultSet resultSet = statement.executeQuery(sql);
          // 5、 使用while循环取出结果
          while(resultSet.next()){   // 光标向下移动，如果没有行  则输出false
              int id = resultSet.getInt(1);  // 获取该行第一列
              String name = resultSet.getString(2);      // 获取该行第二列
              String sex = resultSet.getString(3);
              Date date = resultSet.getDate(4);
              System.out.println("id = " + id + "name = " + name + "sex = " + sex + "date = " + date);
          }
          // 6. 关闭连接
          resultSet.close();
          statement.close();
          connection.close();
      }
  ```

## 4、ResultSet【结果集】：

### 4.1、基本介绍：

- 表示数据库结果集的数据表，通常通过执行查询数据库的语句生成

- ResultSet对象保持一个光标指向其当前的数据行，。最初，光标位于第一行之前

- next方法将光标移动到下一行，并且由于在ResultSet对象中没有更多行返回false，因此可以在while循环中使用循环来遍历结果集

```java
 // 5、 使用while循环取出结果
while(resultSet.next()){   // 光标向下移动，如果没有行  则输出false
	int id = resultSet.getInt(1);  // 获取该行第一列
    String name = resultSet.getString(2);      // 获取该行第二列
    String sex = resultSet.getString(3);
    Date date = resultSet.getDate(4);
    System.out.println("id = " + id + "name = " + name + "sex = " + sex + "date = " + date);
        }
```

## 5、Statement：

### 4.1、基本介绍：

1、Statement对象用于执行静态sql语句并返回其生成的结果的对象

2、在建立连接以后，需要对数据库进行访问，执行命名或是sql语句，可以通过

- Statement【存在sql注入】
- PreparedStatement 【预处理】
- CallableStatement  【存储过程】 

3、Statement对象执行sql语句，存在sql注入风险

```sql
CREATE TABLE IF NOT EXISTS `actor` (
	`id` INT(20) NOT NULL AUTO_INCREMENT COMMENT '演员id',
	`name` VARCHAR(10) NOT NULL DEFAULT ' ' COMMENT '演员姓名',
	`sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
	`borndate` DATETIME,
	`phone` VARCHAR(15) COMMENT '电话',
	PRIMARY KEY(`id`)
)ENGINE = INNODB DEFAULT CHARSET = utf8

SELECT * FROM `actor`
INSERT INTO `actor` VALUES(NULL,'刘德华','男','1990-10-20','11222')
INSERT INTO `actor` VALUES(NULL,'jack','男','1990-11-20','119')

-- sql注入
SELECT * FROM actor WHERE NAME = '1' OR' AND sex = 'OR '1' = '1'
```

4、sql注入是利用某些系统没有对用户输入数据进行充分的检查，而在用户输入数据中注入非法的SQL语句或者命令，恶意攻击数据库

5、要防范SQL注入，只要用PreparedStatement（从Statement扩展而来）取代Statement就可以了

java中的sql注入

```java
public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");

        String url = "jdbc:mysql://localhost:3306/zb_db02?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8";
        Scanner sc = new Scanner(System.in);
        System.out.println("输入名字：");
        String username = sc.nextLine();
        System.out.println("输入密码：");
        String pwd = sc.nextLine();

        String user = "root";
        String password = "zhoubo120011.";

        Connection connection = DriverManager.getConnection(url, user, password);

        String sql = "select name,pwd from admin where name ='"
                + username +"'and pwd = '" + pwd + "'" ;

        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery(sql);

        if (resultSet.next()){
            System.out.println("Login Success!");
            String name = resultSet.getString(1);
            String pwd_ = resultSet.getString(2);
            System.out.println(name + pwd_);
        }else{
            System.out.println("fail!");
        }
        resultSet.close();
        statement.close();
        connection.close();
    }



/*   结果
输入名字：
1' or	// 输入name
输入密码：
or '1'= '1 	//输入pwd
Login Success!
tom123
*/
```

## 5、PrepareStatement：

### 5.1、基本介绍：

- PrepareStatement执行的SQL语句中的参数用问好（？）来表示，调用PrepareStatement的对象的setXxx() 方法来设置这些参数。setXxx() 方法有两个参数，第一个参数是要设置的SQL语句中的参数索引（从1开始），第二个是设置的SQL语句中的参数的值
- 调用executeQuery() ，返回REsultSet对象
- 调用executeUpdate() :   执行更新，包括增，删，修改

### 5.2、预处理好处：

- 不再使用 + 拼接sql语句，减少语法错误
- 有效的解决了sql注入问题
- 大大减少了编译次数，效率较高

```java
@SuppressWarnings({"all"})
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        // 注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/zb_db02?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8";
        String user = "root";
        String password = "zhoubo120011.";

        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = scanner.nextLine();
        System.out.println("请输入密码：");
        String pwd = scanner.nextLine();

        Connection connection = DriverManager.getConnection(url, user, password);
        // 3. 得到prepareStatement
        // 组织sql语言
        String sql = "select name,pwd from admin where name =? and pwd =?";

        // 3.1	PreparedStatement 对象实现了PreparedStatement 接口的实现类的对象
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        // 给sql语言中的第一个？赋值
        preparedStatement.setString(1,username);
        // 给sql中第二个？赋值
        preparedStatement.setString(2,pwd);

        //4. 执行select语句时使用executeQuery()
        //   只是dml（增删改查）时  使用 executUpdate()
        //   这里执行sql语句时  不需	要在写sql
        ResultSet resultSet1 = preparedStatement.executeQuery();
        if (resultSet1.next()){
            System.out.println("登陆成功");
        }else{
            System.out.println("用户名或密码错误！");
        }

        resultSet1.close();
        preparedStatement.close();
        connection.close();
    }
```

test:

```java
public static void main(String[] args)throws ClassNotFoundException, SQLException {
        Class.forName("com.mysql.cj.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/zb_db02?useUnicode=true&characterEncoding=UTF-8&userSSL=false&serverTimezone=GMT%2B8";

        String user = "root";
        String password = "zhoubo120011.";

        Scanner scanner = new Scanner(System.in);
        System.out.println("请输入您的用户名：");
        String username = scanner.nextLine();
        System.out.println("请输入您的密码：");
        String pwd_ = scanner.nextLine();
        Connection connection = DriverManager.getConnection(url, user, password);

        String sql = "select name,pwd from admin where name = ? and pwd = ?";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setString(1,username);
        preparedStatement.setString(2,pwd_);

        ResultSet resultSet = preparedStatement.executeQuery();
    //  如果能查询到   执行
        if (resultSet.next()){
            System.out.println("登陆成功!");
            System.out.println("请输入您想增加的用户名：");
            String insertName = scanner.nextLine();
            System.out.println("请输入您的密码：");
            String insertPwd_ = scanner.nextLine();
            String sql2 = "insert into admin(name,pwd) values (?,?)";
            PreparedStatement preparedStatement1 = connection.prepareStatement(sql2);
            preparedStatement1.setString(1,insertName);
            preparedStatement1.setString(2,insertPwd_);

            int i = preparedStatement1.executeUpdate();
            if (i == 1){
                // 查询到人员之后  进行增加人员
                System.out.println("增加成功！");
                System.out.println("是否想要查询人员：");
                Boolean selectName = Boolean.valueOf(scanner.nextLine());

                if (selectName == true){
                    System.out.println("请输入您的名字：");
                    String selectUser = scanner.nextLine();
                    System.out.println("请输入您的密码：");
                    String selectPwd_ = scanner.nextLine();
                    String sql3 = "select name,pwd from admin where name = ? and pwd = ?";
                    PreparedStatement preparedStatement2 = connection.prepareStatement(sql3);
                    preparedStatement2.setString(1,selectUser);
                    preparedStatement2.setString(2,selectPwd_);

                    ResultSet resultSet1 = preparedStatement2.executeQuery();

                    if (resultSet1.next()){
                        String selectName_ = resultSet1.getString(1);
                        String selectPwd = resultSet1.getString(2);
                        System.out.println(selectName_ + selectPwd);
                    }else{
                        System.out.println("人员不存在！");
                    }
                }else{
                    System.out.println("再见！");
                }
            }else{
                System.out.println("增加失败！");
            }


        }
        preparedStatement.close();
        resultSet.close();
        connection.close();

    }
```

### 6、封装JDBCUtils：

- 说明：在jdbc操作中，获取链接和释放资源是经常使用到的，可以将其封装到jdbc连接的工具类JDBCUtils	

```java
public class JDBCUtils {
    private static String user;
    private static String password;
    private static String url;
    private static String driver;

    // 获取相关信息   ,在static  代码块中进行初始化
    static{
        try {
//            Class.forName("")
            Properties properties = new Properties();
            properties.load(new FileInputStream("src\\mysql.properties"));
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            url = properties.getProperty("url");
            driver = properties.getProperty("driver");
        } catch (IOException e) {
            // - 将编译异常 转成运行异常
            // - 调用者，可以选择捕获该异常，也可以选择默认处理该异常，比较方便
            throw new RuntimeException(e);
        }
    }

    // 获取连接
    public static Connection getConnection(){

        try {
            return DriverManager.getConnection(url, user, password);
        } catch (SQLException e) {
            // - 将编译异常 转成运行异常
            // - 调用者，可以选择捕获该异常，也可以选择默认处理该异常，比较方便
            throw new RuntimeException(e);
        }
    }


    // 关闭相关资源
    /*
    - ResultSet   结果集
    - Statement  或者 PreparedStatement
    - Connection
    - 如果需要关闭资源，就传入对象，否则传入null
     */
    public static void close(ResultSet set, Statement statement, Connection connection){
        try {
            if (set != null){
                set.close();
            }
            if (statement != null){
                statement.close();
            }
            if (connection != null){
                connection.close();
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

}
```

- 测试JDBCUtils

```java
public class JDBCUtils_ues {
    public static void main(String[] args) {
        testDML();
    }
    public static void testDML(){
        // 1.得到链接
        Connection connection = JDBCUtils.getConnection();
        // 2. 组织一个sql
        String sql = "update actor set name = ? where id = ?";
        PreparedStatement preparedStatement = null;
        // 创建个PreparedStatement 对象
        try {
            preparedStatement = connection.prepareStatement(sql);
            // 给占位符赋值
            preparedStatement.setString(1,"周新驰");
            preparedStatement.setInt(2,1);
            preparedStatement.executeUpdate();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            // 关闭资源
            JDBCUtils.close(null,preparedStatement,connection);
        }
    }
}
```

## 7、事务：

### 7.1、基本介绍：

- JDBC程序中当一个Connection对象创建时，默认情况下是自动提交事务：每次执行一个sql语句时，如果执行成功，就会向数据库自动提交，而不能回滚
- JDBC程序中为了让多个sql语句作为一个整体执行，需要使用事务
- 调用Connection 的 setAutoCommit(false) 可以取消自动提交事务
- 在所有的sql语句都成功执行后 ，调用 commit();  方法提交事务
- 在其中某个操作失败或出现异常时，调用 rollback();  方法回滚事务

#### 7.2、模拟事务：

```java
public class Transaction_ {
    public static void main(String[] args) {
        m1();
    }
    public static void m1(){
        // 创建链接
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        String sql = "update account set balance = balance - 100 where id = 1";
        String sql2 = "update account set balance = balance + 100 where id = 2";
        String sql3 = "select * from account";
        try {
            connection = JDBCUtils.getConnection();

            // 先将自动提交关闭  模拟事务
            connection.setAutoCommit(false);

            // 在默认情况下，connection为默认提交的，就算后面发生异常，前面执行的语句也会默认提交
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.executeUpdate(); // 执行第一条sql

            int i = 1/0;   // 抛出异常
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();

            preparedStatement = connection.prepareStatement(sql3);
            preparedStatement.executeQuery();


            resultSet = preparedStatement.executeQuery();
            // 这里提交事务
            connection.commit();
            while (resultSet.next()){
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                int balance = resultSet.getInt("balance");

                System.out.println(id + name + balance);

            }

        } catch (SQLException e) {
            System.out.println("提交失败，回滚执行 ..");
            // 在这里我们可以进行回滚
            try {
                connection.rollback();
                // 默认情况下   回滚到sql语句执行之前
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            e.printStackTrace();
        } finally {
            JDBCUtils.close(resultSet,preparedStatement,connection);
        }

    }
}
```

## 8、批处理：

### 8.1、基本介绍：

- 当需要成批插入或者更新记录时。可以采用java的批量更新机制，这一机制允许多条语句一次性提交给数据库进行批量处理。通常情况下比单独提交处理更有效率

- JDBC的批量处理语句包括下面方法：

  - addBatch()  :  添加需要批量处理的sql语句或参数
  - executeBatch()  :  执行批量处理语句
  - clearBatch()  :  清空批处理包的语句

- > JDBC 连接 MYSQL 时，如果要使用批处理功能，应在 url 中加参数 ?rewriteBatchedStatements=true（老版本需要添加）

- 批处理往往和 PreparedStatement 一起搭配使用，可以既减少编译次数，又减少运行次数，效率大大提高

```java
public class Batch_ {
    public static void main(String[] args) throws Exception {
//        tra();
        batch_();
    }
    public static void tra() throws Exception{
        Connection connection = JDBCUtils.getConnection();
        String sql = "insert into admin2 values (null, ?, ?)";

        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        System.out.println("传统添加方式开始执行...");
        long start = System.currentTimeMillis(); // 开始时间   传统方式 使用时间 = 13469
        for (int i = 0; i < 5000; i++) {
            preparedStatement.setString(1,"tom" + i);
            preparedStatement.setString(2,"666");

            preparedStatement.executeUpdate();

        }
        long end = System.currentTimeMillis(); // 结束时间
        System.out.println("传统方式 使用时间 = " + (end - start));

        // 关闭连接
        JDBCUtils.close(null,preparedStatement,connection);
    }
    

    // 使用批处理
    public static void batch_() throws Exception{
        Connection connection = JDBCUtils.getConnection();
        String sql = "insert into admin3 values(null, ?, ?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        System.out.println("开始执行批处理...");
        long start = System.currentTimeMillis(); // 开始时间
        for (int i = 0; i < 5000; i++) {
            preparedStatement.setString(1,"tom" + i);
            preparedStatement.setString(2,"666");
            // 将sql语言加入到批处理
            preparedStatement.addBatch();
            //当有1000条记录时，进行批处理
            if ((i + 1) % 1000 == 0){
                preparedStatement.executeUpdate();
                // 清空
                preparedStatement.clearBatch();
            }
        }
        long end = System.currentTimeMillis(); // 结束时间   批处理使用时间为 29
        System.out.println("批处理使用时间为 " + (end - start));
        // 关闭连接
        JDBCUtils.close(null,preparedStatement,connection);

    }
}
```

## 9、数据库连接池：

### 8.1、传统获取Connection问题分析：

- 传统的JDBC数据库连接使用DriverManager 来获取，每次向数据库建立连接的时候都要将Connection 加载到内存中，在**验证IP地址，用户名和密码**（0.05~1s时间）。需要数据库连接的时候，就向数据库要求一个，频繁的进行数据库连接操作将占用很多的系统资源，容易造成服务器崩溃
- 每一个数据库连接，使用完成后都要断开，如果程序出现异常而未能关闭，将导致数据库**内存泄漏**，最终将导致重启数据库
- 传统获取连接的方式，不能控制创建的连接数量，如连接过多时，也可能导致内存泄漏，MySQL崩溃
- 解决传统开发中的数据库连接问题，可以采用数据库连接池技术（connection pool

### 8.2、基本介绍：

- 预先在缓冲池中放入一定数量的连接，当需要建立数据库连接时，只需从“缓冲池”中取出一个，使用完毕后再放回去 
- 数据库连接池负责分配、管理和释放数据库连接，它允许应用程序**重复使用**一个现有的数据库连接，而不是重新建立一个
- 当应用程序向连接池请求的连接数超过最大连接数量时，这些请求将被加入到等待队列中

### 8.3、数据库连接池种类：

- JDBC的数据库连接池使用 javax.sql.DataSource 来表示 ,DataSource只是一个接口，该接口通常由第三方提供实现
- **C3P0** 数据库连接池，速度相对较慢，稳定性不错
- DBCP数据库连接池，速度相对C3P0较快，但是不稳定
- Proxool 数据库连接池，有监控连接池状态的功能，稳定性较C3P0差一点
- BoneCP 数据库连接池，速度快
- **Druid（德鲁伊）** 是阿里提供的数据库连接池，集DBCP、C3P0、Proxool优点于一身的数据库连接池

### 8.4、Test:

#### 8.4.1、 测试c3p0

- 首先导入相关的连接池的jar包

![image-20211222160651149](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211222160651149.png)

```java
// 方式一： 相关参数，在程序中指定user, url , password
    public void testC3P0_01() throws Exception {
        //1. 首先建立一个数据源对象
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();
        // 2. 通过配置文件获取相关连接信息
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\mysql.properties"));
        String user = properties.getProperty("user");
        String password = properties.getProperty("password");
        String url = properties.getProperty("url");
        String driver = properties.getProperty("driver");

        // 给数据源 combopooledDataSource  设置相关的参数
        //注意： 连接管理是由combopooledDataSource 来管理的
        comboPooledDataSource.setDriverClass(driver);
        comboPooledDataSource.setJdbcUrl(url);
        comboPooledDataSource.setUser(user);
        comboPooledDataSource.setPassword(password);

        // 初始化数据源的连接数
        comboPooledDataSource.setInitialPoolSize(50);
        // 设置最大数据源连接数
        comboPooledDataSource.setMaxPoolSize(100);
        Connection connection = comboPooledDataSource.getConnection();
        System.out.println("连接成功");
        connection.close();
    }
```

#### 8.4.2、测试Druid：

```java
public class Druid_ {
    @Test
    //1.加入 Druid jar包
    //2. 加入配置文件 druid.properties  ,将该文件拷贝项目的src目录
    //3. 创建Properties 对象， 读取配置文件
    public void testDruid() throws Exception {
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\druid.properties"));

        //4. 创建一个指定参数的数据库连接池， Druid连接池
        DataSource dataSource = DruidDataSourceFactory.createDataSource(properties);
        Connection connection = dataSource.getConnection();
        System.out.println("连接成功...");
        connection.close();
    }
}
```

![image-20211223101232139](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211223101232139.png)

**注意：Druid在大量数据处理时比c3p0速度快上很多**

#### 8.4.3、测试Druid 工具类：

```java
public class JDBCUtilsByDruid_Use {
    @Test
    public static void test(){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        String sql = "select * from actor where id = ?";
        try {
            connection = JDBCUtilsByDruid.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setInt(1,1);
            resultSet = preparedStatement.executeQuery();
            while (resultSet.next()){
                String name = resultSet.getString("name");
                String sex = resultSet.getString("sex");
                Date date = resultSet.getDate("borndate");
                String phone = resultSet.getString("phone");
                int id = resultSet.getInt("id");
                System.out.println(id + "\t" + name + "\t" + sex +"\t" + date + "\t" + phone );
            }

        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            JDBCUtilsByDruid.close(resultSet,preparedStatement,connection);
        }

    }
}
```

### 8.3、Apache—DBUtils

![image-20211223143434556](../java%E8%BD%AF%E4%BB%B6/TyporaPhoto/image-20211223143434556.png)
