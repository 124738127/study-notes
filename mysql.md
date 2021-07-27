# mysql

## windows community版本 zip安装

1. community下载地址

   ```markdown
   https://dev.mysql.com/downloads/mysql/

2. 解压mysql到 D:\DevEnv\mysql-8.0.25

3. 在解压目录下，新建my.ini，添加配置

   ```ini
   [mysql]
   # 设置mysql客户端默认字符集
   default-character-set=utf8mb4 
   
   [mysqld]
   # 设置mysql服务端默认监听的端口
   port = 3306
   
   # set basedir to your installation path
   basedir=D:/DevEnv/mysql-8.0.25
   # set datadir to the location of your data directory
   datadir=D:/DevEnv/mysql-data
   
   # 设置mysql服务端默认字符集
   character-set-server=utf8mb4
   collation-server = utf8mb4_unicode_ci 
   
   # 创建新表时将使用的默认存储引擎
   default-storage-engine=INNODB
   
   # 允许最大连接数
   max_connections=200
   ```

4. 理解 mysql client 和 mysql server

   ```markdown
   客户 --> mysql client --> mysql server --> 物理数据
   
   mysql client是指对外服务的客户端，生成管理数据库实例，数据库实例任务调度线程之类，并提供相关接口供不同客户端调用，增删改查命令等
   mysql server是运行的mysql服务，操作数据库实例的工具，从官网下载的mysql server默认包含了client客户端
   ```

5. 环境变量配置

   ```markdown
   配置：
   MYSQL_HOME: D:\DevEnv\mysql-8.0.25
   PATH: %MYSQL_HOME%\bin
   ```

6. mysql初始化，管理员权限cmd

   ```markdown
   # 移除mysql server
   mysqld --remove servername
   
   # 初始化mysql server
   mysqld --initialize
   
   # 安装mysql server
   mysqld --install servername
   默认数据库服务名为: mysql
   
   # 启动mysql server
   # windows环境需要管理员权限
   net start servername
   
   # 连接mysql server
   mysql -u username -p
   password储存在临时密码为mysql data路径下的machinename.err文件内，在初始化时生成
   初始化用户名为root
   ```

7. 修改root用户密码

   ```sql
   # 修改密码
   ALTER user 'root'@'localhost' IDENTIFIED BY 'password';
   
   # Psbc@123456
   ```

## 创建数据库

1. 查看数据库

   ```sql
   show databases;
   ```

2. 创建数据库

   ```sql
   create database databasename;
   ```
   
3. 选择数据库

   ```sql
   # 选择数据库后，所有的sql操作默认在该库下，无需指定数据库
   use databasename;
   
   # 如果不选择数据库，所有的sql操作需要指定数据库
   ```

##  创建用户

1. 查看用户

   ```sql
   select * from mysql.user;
   ```

2. 创建用户

   ```sql
   CREATE USER 'username'@'host' IDENTIFIED BY 'password';
   
   # 说明：
   -- username：创建的用户名
   -- host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
   -- password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器
   -- 新建的用户，默认是没有任何权限的
   # 举例：
   CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
   CREATE USER 'pig'@'192.168.1.101' IDENDIFIED BY '123456';
   CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
   CREATE USER 'pig'@'%' IDENTIFIED BY '';
   CREATE USER 'pig'@'%'; 
   ```

3. 删除用户

   ```sql
   DROP USER 'username'@'host';
   ```

4. 分配用户权限

   ```sql
   GRANT privileges ON databasename.tablename TO 'username'@'host';
   
   # 说明：
   -- databasename：用户的操作权限，如select，insert，update，delete，create，drop等，如果要授予所的权限则使用ALL
   -- tablename：数据库名
   -- username：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*
   -- host：指定该用户的主机
   
   # 举例：
   GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
   GRANT ALL ON *.* TO 'pig'@'%';
   GRANT ALL ON maindataplus.* TO 'pig'@'%';
   ```

5. 授权用户给其他用户授权

   ```sql
   GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
   ```

6. 设置或更改用户密码

   ```sql
   SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
   ```

7. 取消用户授权

   ```sql
   REVOKE privilege ON databasename.tablename FROM 'username'@'host';
   ```

## 执行*.sql脚本

进入mysql命令行后，使用source命令

```sql
source filepath;
```

或者，使用\.

```sql
\. filepath;
```



