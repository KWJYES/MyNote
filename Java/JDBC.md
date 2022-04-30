# 概括

​		JDBC操作MySQL

# 1.加载数据库驱动

在pom.xml中导入依赖

```xml
<!--mysql驱动-->
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>
```

加载数据库驱动

```java
try {
    Class.forName("com.mysql.cj.jdbc.Driver");//加载数据库驱动
} catch (ClassNotFoundException e) {
    e.printStackTrace();
}
```

# 2.连接上数据库

```java
private final static String url="jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&useSSL=false&serverTimezone=Asia/Shanghai&zeroDateTimeBehavior=CONVERT_TO_NULL&allowPublicKeyRetrieval=true";
private final static String root="root";
private final static String password="Wkj161001.";
-------------------------------------------------------
try{
            Connection connection= DriverManager.getConnection(url,root,password);
            PreparedStatement ps=connection.prepareStatement("select * from student");
            ResultSet resultSet=ps.executeQuery();
            while (resultSet.next()){
                int id=resultSet.getInt("id");
                String name=resultSet.getString("name");
                System.out.println("学号："+id+"   姓名："+name);
            }
            connection.close();
        } catch (SQLException throwable) {
            throwable.printStackTrace();
        }
```

# 3.基本操作

## 2.增：

```java
PreparedStatement ps1=connection.prepareStatement("insert into student values(?,?)");
ps1.setInt(1,2100300524);
ps1.setString(2,"prh");
ps1.executeUpdate();
```

## 3.删：

```java
PreparedStatement ps1=connection.prepareStatement("delete from student where name=?");
ps1.setInt(1,123);
ps1.setString(1,"prh");
ps1.executeUpdate();
```

## 4.改：

```java
PreparedStatement ps1=connection.prepareStatement("update student set id=? where name=?");
ps1.setInt(1,123);
ps1.setString(2,"prh");
ps1.executeUpdate();
```

## 5.查：

```java
PreparedStatement ps=connection.prepareStatement("select * from student where id =?");
ps.setInt(1,2100300425);
ResultSet resultSet=ps.executeQuery();
```

