# 1.前往官网下载:[MySQL官网](https://www.mysql.com/)

![image-20220407121550333](image\MsSQL下载_1.png)



![image-20220407122126069](image\MsSQL下载_2.png)

![image-20220407122126069](image\MsSQL下载_3.png)

![image-20220407122126069](image\MsSQL下载_4.png)

点击图中下方蓝色文字就可以开始下载了

![image-20220407122126069](image\MsSQL下载_7.png)

但一般超级慢(“3天...”)

![image-20220407122126069](image\MsSQL下载_6.png)

所以你可以和我一样去下载一个“讯雷”，在Edge浏览器中会这样，你在点那蓝色文字，讯雷就会跳出来了(我的可达3M/s)

讯雷下载官网:[迅雷](https://www.xunlei.com/)

![image-20220407122126069](image\MsSQL下载_5.png)

![image-20220407122126069](image\MsSQL下载_8.png)

# 2.配置MySQL

把刚刚下好的压缩包解压到你喜欢的地方(你的安装路径)

![image-20220407122126069](image\MsSQL下载_9.png)

在这个路径下新建一个my.txt文件，如上图

把下面的配置复制到my.txt中

然后修改**mysql的安装目录**和**mysql数据库的数据的存放目录**为你自己的，下面是我的

```ini
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录   
basedir=D:\MySQL\mysql-8.0.28-winx64
# 设置mysql数据库的数据的存放目录  
datadir=D:\MySQL\mysql-8.0.28-winx64\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

然后将这个**my.txt**另存为**my.ini**(选所有格式)

![image-20220407122126069](image\MsSQL下载_10.png)

![image-20220407122126069](image\MsSQL下载_11.png)

以**管理员身份**运行cmd

先cd 到你安装路径的的bin目录中(下面是我的)

```
cd D:\MySQL\mysql-8.0.28-winx64\bin
D:
```

![image-20220407122126069](image\MsSQL下载_12.png)

cd 到bin目录后,输入命令mysqld --initialize --console

```
mysqld --initialize --console
```

![MsSQL下载_12 - 副本](image\MsSQL下载_12 - 副本.png)

运行结束后记住**root@localhost:后的密码**后面会用

然后执行mysqld --install命令进行安装

```
mysqld --install
```

![MsSQL下载_12 - 副本 (5)](image\MsSQL下载_12 - 副本 (5).png)

看到successfully表示成功了

然后启动MySQL服务

```
net start mysql
```

![MsSQL下载_12 - 副本 (4)](image\MsSQL下载_12 - 副本 (4).png)

然后执行mysql -uroot -p  并输入刚刚**root@localhost:后的密码**登陆数据库

```
mysql -uroot -p
```

![MsSQL下载_12 - 副本 (3)](image\MsSQL下载_12 - 副本 (3).png)

最后再修改密码

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
```

# 3.配置环境变量

(win 10)打开设置，点“关于”，点“高级系统设置”

![配置环境变量](image\配置环境变量.png)

![配置环境变量2](image\配置环境变量2.png)

![1](image\1.png)

变量名:

```
MYSQL_HOME
```

变量值(你的安装目录)：

```
D:\MySQL\mysql-8.0.28-winx64
```



![2](image\2.png)

双击Path变量

![3](image\3.png)

加入下面两个值：

```
%MYSQL_HOME%\bin
```

```
%MYSQL_HOME%
```

![4](image\4.png)

# 4.检验安装

cd 到安装的bin目录

```
cd D:\MySQL\mysql-8.0.28-winx64\bin
```

![5](image\5.png)

输入mysql -h localhost -u root -p 并输入你的密码登录数据库

```
mysql -h localhost -u root -p
```

![6](image\6.png)

用status命令显示 MySQL的相关信息，说明安装成功

```
status
```

![7](image\7.png)

大功告成了
