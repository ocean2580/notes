# 一、 JDBC

为访问不同数据库提供的==统一接口==

![image-20220420093055223](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420093055223.png)



面向==接口==编程



## 1.1导包



​                                                       ==有用==

![image-20220420103929178](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420103929178.png)

![image-20220420104301985](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420104301985.png)

==最终直接添加到外部库==



## 1.2 编写

1) 注册驱动 -> 加载Driver 类

```java
Driver driver = new Driver();
```



2) 获取连接 -> 得到Connection

```java
// 协议 // ip:port / database
// mysql通过socket连接
        String url = "jdbc:mysql://localhost:3306/dxh_db01";

        // account, password -> properties
        Properties properties = new Properties();
        properties.setProperty("user","root"); // account
        properties.setProperty("password","dxh"); // password

        Connection connect = driver.connect(url, properties);
```



3) 执行增删改查 -> 发送SQL给MySQL执行

```java
String sql = "insert into actor values(null, 'Joey', '男', '2000-1-1',123)";
        // 执行静态SQL语句并返回生成结果的对象
        Statement statement = connect.createStatement();
        int rows = statement.executeUpdate(sql);
        System.out.println(rows > 0 ? "yes" : "no");
```



4) 释放资源 -> 关闭连接

```java
// limited sources
        statement.close();
        connect.close();
```

   ## 1.3 总结        

   ==驱动管理者得到连接创造语句执行操作==

```java
DriverManager
           .getConnection()
           .createStatement()
           .executeQuery();

```



![image-20220420144204602](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420144204602.png)



# 二、连接数据库

​                                                 ==得到connect==

传统方式连接过多抛异常 " Too many connections"

## 2.1 静态加载

灵活性差，依赖强

```java
Driver driver = new Driver();

Properties properties = new Properties();
        properties.setProperty("user","root"); 
        properties.setProperty("password","dxh"); 

Connection connect = driver.connect(url, properties);
```



## 2.2 反射

动态加载

```java
Class<?> aClass = Class.forName("com.mysql.jdbc.Driver");
Driver driver = (Driver) aClass.getDeclaredConstructor().newInstance();

Properties properties = new Properties();
        properties.setProperty("user","root"); 
        properties.setProperty("password","dxh"); 

Connection connect = driver.connect(url, properties);
```



## 2.3 DriverManager



```java
//  加载Driver类时，（其静态代码块）自动完成注册驱动
Class.forName("com.mysql.jdbc.Driver");

        String url = "jdbc:mysql://localhost:3306/dxh_db01";
        
        Properties properties = new Properties();
        properties.setProperty("user","root"); 
        properties.setProperty("password","dxh"); 

 
Connection connection = DriverManager
    .getConnection(url, properties);
// properties -> (String) user, password
```

![image-20220420121929259](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420121929259.png)







## 2.4 ==模板==



```java
// Properties obj
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\mysql.properties"));

        // info
        String user = properties.getProperty("user");
        String url = properties.getProperty("url");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");
        
        Connection connection = DriverManager
                .getConnection(url, user, password);
```

![image-20220420123253920](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420123253920.png)

==配置文件==更灵活



# 三、SQL

## 3.1 CRUD

DML + SELECT = CRUD



DML -> executeUpdate(): int           ==affected rows==   

SELECT -> executeQuery(): ResultSet     





## 3.2 ResultSet 

类比 ==双向迭代器==

```java
Statement statement = connection.createStatement();
        
        String sql = "select id, name, sex, birthdate from actor";
        ResultSet resultSet = statement.executeQuery(sql);

        while (resultSet.next()){ // resultSet指针指 行
            // 取列
            int id = resultSet.getInt(1); 
            int id = resultSet.getInt("id");
            
            String name = resultSet.getString(2);
            String sex = resultSet.getString(3);
            Date date = resultSet.getDate(4);

            System.out.println(id  + "\t" + name + "\t" + sex + "\t" + date );

        }

resultSet.close();
```

![image-20220421224806772](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421224806772.png)



## 3.3 Statement ( SQL 注入风险)



![image-20220420151250419](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420151250419.png)

![image-20220420151918933](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420151918933.png)

（Statement）万能密码：  or '1' = '1



## 3.4 PreparedStatement （预处理）

不用 + 拼接SQL语句，减少编译次数

![image-20220420155829431](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420155829431.png)



```java
// ? 占位符
        String sql = "select * from actor where id = ? ";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setInt(1,1); // 处理sql

        ResultSet resultSet = preparedStatement.executeQuery(); 
```

# 四、JDBC API



![image-20220420163820963](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420163820963.png)

![image-20220420164347078](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420164347078.png)



==preparedStatement 由于其 sql 执行前会被改变，所以 执行方法**空参**==



#  五、==JDBCUtils== (得到/关闭 连接)



```java
package dxh.jdbc.utils;

public class JDBCUtils {
    private static String user;
    private static String password;
    private static String url;
    private static String driver;

    // 字段初始化
    static {

        try {
            Properties properties = new Properties();
            properties.load(new FileInputStream("src\\mysql.properties"));

            // read
            user = properties.getProperty("user");
            password = properties.getProperty("password");
            url = properties.getProperty("url");
            driver = properties.getProperty("driver");

        } catch (IOException e) {
            // 1. 编译异常 转换为 运行异常
            // 2. 调用者可以 捕获该异常 / 默认处理该异常
            throw new RuntimeException(e);
        }


    }

    // get
    public static Connection getConnection() {
        try {
            return DriverManager.getConnection(url,user,password);
        } catch (SQLException e) {

            throw new RuntimeException(e);
        }
    }

    // close
    public static void close(ResultSet set, Statement statement, Connection connection) {
        // alter + ctrl + T
        try {
            if (set != null) {
                set.close();
            }
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }

    }

}

```

# 六、事务

![image-20220420195852616](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420195852616.png)

```java 
Connection connection = null;

        String sql = "update account set balance = balance - 100 where id = 1";
        String sql2 = "update account set balance = balance + 100 where id = 2";

        PreparedStatement preparedStatement = null;

        try {
            connection = JDBCUtils.getConnection(); // 默认自动提交
            
            connection.setAutoCommit(false); // 取消自动提交 (开启事务)

            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.executeUpdate(); // sql

            // int i = 1/0;
            // 导致异常则会撤销操作

            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate(); // sql2

            // commit
            connection.commit();

        } catch (SQLException e) {
            System.out.println("No");
            // roll back
            // 默认回滚到事务开始状态 (撤销sql)
            connection.rollback();
            e.printStackTrace();
        } finally {
            JDBCUtils.close(null,preparedStatement,connection);
        }
```



# 七、批处理

![image-20220420203530554](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420203530554.png)

![image-20220420205735832](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420205735832.png)

```Java
Connection connection = JDBCUtils.getConnection();
        String sql = "insert into admin2 values(null, ?, ?)";
        PreparedStatement preparedStatement = connection.prepareStatement(sql);

        System.out.println("开始 ");

        long start = System.currentTimeMillis();
        for (int i = 0; i < 1200; i++) {
            preparedStatement.setString(1,"Jack" + i);
            preparedStatement.setString(2,"666");

            // add sql
            preparedStatement.addBatch();
            
            if ((i + 1) % 1000 == 0) {
                // execute sqls
                preparedStatement.executeBatch();
                // clear
                preparedStatement.clearBatch();
            }
        }

        long end = System.currentTimeMillis();
        System.out.println("耗时=" + (end - start));

        JDBCUtils.close(null,preparedStatement,connection);
```

addBatch(), executeBatch(), clearBatch()



# 八、数据库连接池

## 8.1 传统连接弊端

![image-20220420211331295](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420211331295.png)

![image-20220420211727946](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420211727946.png)



## 8.2 原理

![image-20220420212613364](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420212613364.png)

![image-20220420213000691](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420213000691.png)

==连接池 分配、管理和释放 **有限的**连接（让应用程序 **重复使用** 而不是 新建连接）， 请求数超过最大连接数时，超额请求 被加入 **等待队列**==



## 8.3 常用

![image-20220420214202491](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420214202491.png)

### 8.3.1 C3P0

==要导入相关的jar包==

![image-20220420225247785](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220420225247785.png)

==上下两个==



- 常规

```Java
ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();

        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\mysql.properties"));

        String user = properties.getProperty("user");
        String url = properties.getProperty("url");
        String password = properties.getProperty("password");
        String driver = properties.getProperty("driver");

        // set
        comboPooledDataSource.setDriverClass(driver);
        comboPooledDataSource.setUser(user);
        comboPooledDataSource.setJdbcUrl(url);
        comboPooledDataSource.setPassword(password);

        // 连接数
        comboPooledDataSource.setInitialPoolSize(10);
        comboPooledDataSource.setMaxPoolSize(50);

        Connection connection = comboPooledDataSource.getConnection();
        System.out.println("Yes");
        connection.close();
```

- 配置文件.xml

该方法目前抛空指针异常，==未解决==

```java
        // c3p0.config.xml 拷贝到 src目录

        // read xml
        ComboPooledDataSource comboPooledDataSource = new ComboPooledDataSource();

        Connection connection = comboPooledDataSource.getConnection();
        System.out.println("yyy");

        connection.close();
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<c3p0-config>
    <!--
    c3p0的缺省（默认）配置
    如果在代码中"ComboPooledDataSource ds=new ComboPooledDataSource("");"这样写就表示使用的是C3P0的缺省（默认）配置信息来创建数据源-->
    <default-config>
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/dxh_db01?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false</property>
        <property name="user">root</property>
        <property name="password">dxh</property>

        <property name="acquireIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </default-config>




    <!--
    c3p0的命名配置
    如果在代码中"ComboPooledDataSource ds=new ComboPooledDataSource("MySQL");"这样写就表示使用的是name是MySQL的配置信息来创建数据源-->
    <name-config name="mysql">
        <property name="driverClass">com.mysql.jdbc.Driver</property>
        <property name="jdbcUrl">jdbc:mysql://localhost:3306/dxh_db01?useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC</property>
        <property name="user">root</property>
        <property name="password">dxh</property>

        <property name="acquireIncrement">5</property>
        <property name="initialPoolSize">10</property>
        <property name="minPoolSize">5</property>
        <property name="maxPoolSize">20</property>
    </name-config>
</c3p0-config>

```



### 8.3.2 Druid

==强烈推荐==

```java
// get properties
        Properties properties = new Properties();
        properties.load(new FileInputStream("src\\druid.properties"));

        // connect pool
        DataSource dataSource =
                DruidDataSourceFactory.createDataSource(properties);

        Connection connection = dataSource.getConnection();
        System.out.println("yes");
        connection.close();
```

src中添加配置文件 druid.properties

```properties
url=jdbc:mysql://localhost:3306/dxh_db01?rewriteBatchedStatements=true
username=root
password=dxh
driverClassName=com.mysql.jdbc.Driver
initialSize=10
maxActive=50
maxWait=2000
filters=wall
```



#### ==JBDCUtilsByDruid==

注意 Connection 类导包路径

```java
import com.alibaba.druid.pool.DruidDataSourceFactory;
import java.sql.Connection; // !!!
import com.mysql.jdbc.Statement;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Properties;

public class JDBCUtilsByDruid{

    private static DataSource ds;

    static {
        Properties properties = new Properties();
        try {
            properties.load(new FileInputStream("src\\druid.properties"));
            ds = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    // 放回连接池
    public static void close(ResultSet resultSet, Statement statement, Connection connection) {
        try {
            if (resultSet != null) {
                resultSet.close();
            }
            if (statement != null) {
                statement.close();
            }
            if (connection != null) {
                connection.close();
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }


    }
}
```

# 九、==ApDBUtils== (保存resultSet数据)

Apache - DBUtils

![image-20220421122540116](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421122540116.png)

==resultSet与connection关联==



![image-20220421122554450](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421122554450.png)

==ArrayList<?>存储结果==



![image-20220421123954775](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421123954775.png)

==官网下载 commons-dbutils-xxx.jar==

## 9.1 Query



```java
public void testQuery() throws SQLException { 
        Connection connection = JDBCUtilsByDruid.getConnection();

        // 查询（执行 sql 语句）并返回 resultSet ,封装到 ArrayList 集合中
        // param3: 反射
        // param4: 给 ？ 赋值（可多个）
        // resultQuery, preparedStatement 已关闭
        QueryRunner queryRunner = new QueryRunner();
    
// 1.多行 ( n^2 )
        String sql = "select * from actor where id >= ?";
        List<Actor> list =
                queryRunner.query(connection, sql, new BeanListHandler<>(Actor.class), 1);

        for (Actor actor : list) {
            System.out.println(actor);
        }

// 2.单行 ( n ) 
        String sql = "select * from actor where id = ?";

        Actor actor =
                queryRunner.query(connection, sql, new BeanHandler<>(Actor.class), 1);
        // 未查询到则返回 null
        System.out.println(actor);
    
// 3.单行单列 ( 1 )
        String sql = "select name from actor where id = ?";

        Object obj = 
            queryRunner.query(connection, sql, new ScalarHandler(), 1);
        System.out.println(obj);
    
 
        JDBCUtilsByDruid.close(null,null,connection);

    }
```

new BeanListHandler<>(Actor.class)   -> List

new BeanHandler<>(Actor.class)  ->  Actor

new ScalarHandler()   -> Object



## 9.2 DML



```Java
public void testUML() throws SQLException {

        Connection connection = JDBCUtilsByDruid.getConnection();

        QueryRunner queryRunner = new QueryRunner();

        // DML SQL
        String sql1 = "update actor set name = ? where id = ?";
        String sql2 = "insert into actor values(null,'Jerry','男','2000-10-15','12581')";
        String sql3 = "delete from actor where id = ?";


        int affectedRow1 = queryRunner.update(connection, sql1, "Ocean", 1);
        int affectedRow2 = queryRunner.update(connection, sql2);
        int affectedRow3 = queryRunner.update(connection,sql3,4);

        System.out.println(affectedRow3 > 0 ? "y" : "n"); // 是否对表有影响


        JDBCUtilsByDruid.close(null,null,connection);


    }
```



## 9.3 Bean类

```java 
// Bean
public class Actor {
    private int id;
    private String name;
    private String sex;
    private Date birthday;
    private String phone;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public String toString() {
        return "Actor{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", birthday=" + birthday +
                ", phone='" + phone + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public Actor(int id, String name, String sex, Date birthday, String phone) {
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.birthday = birthday;
        this.phone = phone;
    }

    public Actor() { // 反射需要
    }
}
```



- 表 和 JavaBean 类型映射

![image-20220421153900196](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421153900196.png)

# 十、DAO (泛型通用)

![image-20220421230427548](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421230427548.png)

![image-20220421220735180](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421220735180.png)

data access object 数据访问对象

## 10.1 BasicDAO

作为==父类==

```java
package dxh.dao_.dao;

import java.sql.Connection;
import dxh.dao_.utils.JDBCUtilsByDruid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.SQLException;
import java.util.List;

public class BasicDAO<T> { // 泛型指定具体类
    private QueryRunner qr = new QueryRunner();

    // DML 针对任意表
    public int update(String sql, Object... parameters) {

        Connection connection = null;

        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.update(connection, sql, parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e); // compile -> run
        } finally {
            JDBCUtilsByDruid.close(null,null,connection);
        }

    }

    /**
     *
     * @param sql sql语句
     * @param clazz 传入一个类的Class对象
     * @param parameters 传入 ? 具体的值
     * @return 根据 clazz 返回对应对象
     */
    public List<T> queryMulti(String sql, Class<T> clazz, Object... parameters) {
        Connection connection = null;

        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.query(connection, sql, new BeanListHandler<T>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e); // compile -> run
        } finally {
            JDBCUtilsByDruid.close(null,null,connection);
        }


    }

    // Single row
    public T querySingle(String sql, Class<T> clazz, Object... parameters) {
        Connection connection = null;

        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.query(connection, sql, new BeanHandler<T>(clazz), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e); // compile -> run
        } finally {
            JDBCUtilsByDruid.close(null,null,connection);
        }

    }

    // Scalar
    public Object queryScalar(String sql,  Object... parameters) {
        Connection connection = null;

        try {
            connection = JDBCUtilsByDruid.getConnection();
            return qr.query(connection, sql, new ScalarHandler(), parameters);
        } catch (SQLException e) {
            throw new RuntimeException(e); // compile -> run
        } finally {
            JDBCUtilsByDruid.close(null,null,connection);
        }

    }

}

```

## 10.2 ActorDAO

```java
import dxh.dao_.domain.Actor;

public class ActorDAO extends BasicDAO<Actor>{

    // 根据需求另外加功能
}
```

## 10.3 TestDAO

```java
import dxh.dao_.dao.ActorDAO;
import dxh.dao_.domain.Actor;
import org.junit.Test;

import java.util.List;

public class TestDAO {

    @Test
    public void testActorDAO() {
        ActorDAO actorDAO = new ActorDAO();

        List<Actor> actors =
                actorDAO.queryMulti("select * from actor where id >= ?", Actor.class, 1);
        for (Actor actor : actors) {
            System.out.println(actor);
        }

        System.out.println("==============================");
        Actor actor = actorDAO.querySingle("select * from actor where id = ?", Actor.class, 2);
        System.out.println(actor);

        System.out.println("==============================");
        Object o = actorDAO.queryScalar("select name from actor where id = ?", 3);
        System.out.println(o);

        // DML
        int update = actorDAO.update("insert into actor values(null,?,?,?,?)", "Helen", "女", "2001-10-16", "3445566");
        System.out.println(update > 0? "y" : "n");


    }


}
```





## 10.4 另外

![image-20220421221014801](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421221014801.png)

![image-20220421221609951](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220421221609951.png)



