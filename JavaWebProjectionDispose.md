
## 10.11 JavaWeb配置

### （1）安装配置MySQL

##### （1-1）rpm包

+ rpm包安装：

  1. 下载：

     ( 1 )  [进入mysql官网下载页](https://dev.mysql.com/downloads/mysql/)

     ( 2 )  选择General Availability (GA) Releases栏（Archive栏是存放历史版本）

     ( 3 )  选择操作系统：RedHat

     ( 4 )  选择OS版本：`cat /etc/redhat-release`命令查询当前OS版本

     ( 5 )  选择下载文件：Red Hat Enterprise Linux 8 / Oracle Linux 8 (x86, 64-bit), RPM Bundle(mysql-8.0.20-1.el8.x86_64.rpm-bundle.tar)（该文件包含了下面全部的rpm包）

     ( 6 )  登录甲骨文账户（账号密码见备忘录）

  2. 将安装包上传至服务器：

  3. 解压到/usr/mysql/目录下：`tar -xvf mysql-8.0.20-1.el7.x86_64.rpm-bundle.tar -C /usr/mysql/`

  4. 先安装客户端：`rpm -ivh mysql-community-client-8.0.20-1.el7.x86_64.rpm`

     如果发现有依赖警告，先解决依赖问题，再执行第4步：

     ```
     错误：依赖检测失败：
     	libnuma.so.1()(64bit) 被 mysql-community-client-8.0.20-1.el7.x86_64 需要
     	libnuma.so.1(libnuma_1.1)(64bit) 被 mysql-community-client-8.0.20-1.el7.x86_64 需要
     	libnuma.so.1(libnuma_1.2)(64bit) 被 mysql-community-client-8.0.20-1.el7.x86_64 需要
     	mysql-community-common(x86-64) = 8.0.20-1.el7 被 mysql-community-client-8.0.20-1.el7.x86_64 需要
     ```

     解决依赖：

     ( 1 )  `yum install numactl`

     ( 2 )  `rpm -ivh mysql-community-common-8.0.20-1.el7.x86_64.rpm`

     ( 3 )  `rpm -ivh mysql-community-libs-8.0.20-1.el7.x86_64.rpm`

     ​		如果报错：mariadb-libs 被 mysql-community-libs-8.0.20-1.el7.x86_64 取代.

     ​		因为：CentOS的默认数据库不再是MySQL，而是MariaDB（完全兼容mysql，防甲骨文闭源mysql）

     ​	    (3.1) 查找是否存在：`rpm -qa | grep mariadb`

     ​	    (3.2) 若存在则删除它：`rpm -e --nodeps mariadb-libs`

     ​	    (3.4) 再重新安装mysql-community-libs

     ( 4 )  以上则把所有依赖都解决了，重新安装mysql-community-client

  5. 最后安装服务端：`rpm -ivh mysql-community-server-8.0.20-1.el7.x86_64.rpm`

  6. 检查安装情况：

     `rpm -qa | grep mysql`

     `rpm -ql 上一步查询到的mysql服务端全称`

     `which mysql`

+ 卸载：

  1. 停掉服务：

     `systemctl stop mysqld.service`

  2. rpm删除：

     `rpm -qa | grep mysql`    --    查看完整rpm包名

     `rpm -e --nodeps mysql-community-server-8.0.20-1.el7.x86_64`

     `rpm -e --nodeps mysql-community-client-8.0.20-1.el7.x86_64`

  3. 删除mysql数据库目录：

     `find / -name mysql`

     `rm -rf /var/lib/mysql/`

     `rm -rf /usr/lib64/mysql/`

  4. 删除配置文件及备份文件：

     ·`rm -rf /etc/my.cnf`

     `rm -rf /etc/my.cnf.rpmsave`

+ 使用：

  1. 必须在启动前先设置（初始化之前）：`$ vim /etc/my.cnf`

     ( 1.1 )  大小写不敏感：

     注意：如果源码中jdbc乱用大写的表名，那么此处建议修改大小写不敏感

     [修改及详细说明还得是官网](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html)

     ```shell
     [mysql]
     lower_case_table_names=1
     ```

     ( 1.2 )  设置编码：

     ```shell
     [mysqld]
     character_set_server=utf8
     init-connect='SET NAMES utf8'
     ```

     ( 1.3 )  设置默认端口：

     ```shell
     [mysqld]
     port=3306
     ```

  2. 检查mysql是否启动：

     `netstat -tlp | grep mysql`

     `ps -ef | grep mysql`

  3. 开启关闭mysql：

     开启    --    `systemctl start mysqld`

     关闭    --    `systemctl stop mysqld`

     重启    --    `systemctl restart mysqld`

     开启开机自启    --    `systemctl enable mysqld.service`

     取消开机自启    --    `systemctl disable mysqld.service`

  4. 如果是初次登录mysql-service服务器：

     错误提示：Access denied for user 'root'@'localhost' (using password: NO)

     ( 1 )  需要获取初始登录密码：`grep 'temporary password' /var/log/mysqld.log`

     ( 2 )  复制这个密码后登录：`mysql -u root -p`

     ( 3 )  直接将密码粘贴到输入提示框即可

  5. 首次登录进去后需要修改root用户密码（一般设置密码为‘admin’）：

     ```mysql
     -- （5.1）修改密码：
     mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'admin';
     ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
     # 报错，因为密码太简单
     # 解决：
     # (1) 设置复杂密码，大小写数字
     # (2) 但是有些jdbc的密码是固定的，所以只能修改默认策略。
     #	  修改策略前，还是得设置一个复杂密码，然后再改过来。
     -- （5.2）查看策略
     mysql> SHOW VARIABLES LIKE 'validate_password%';
     +--------------------------------------+--------+
     | Variable_name                        | Value  |
     +--------------------------------------+--------+
     | validate_password.check_user_name    | ON     |
     | validate_password.dictionary_file    |        |
     | validate_password.length             | 8      |
     | validate_password.mixed_case_count   | 1      |
     | validate_password.number_count       | 1      |
     | validate_password.policy             | MEDIUM |
     | validate_password.special_char_count | 1      |
     +--------------------------------------+--------+
     mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '满足要求的复杂密码';
     -- （5.3）修改策略【注意：参数名一定要跟上一步的表核对一下，不同版本参数名不同！】
     mysql> set global validate_password.policy=0;
     mysql> set global validate_password.length=4;
     mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'admin';
     ```

  6. 查看编码：

     ```mysql
     mysql> SHOW VARIABLES LIKE 'character%';
     +--------------------------+--------------------------------+
     | Variable_name            | Value                          |
     +--------------------------+--------------------------------+
     | character_set_client     | utf8mb4                        |
     | character_set_connection | utf8mb4                        |
     | character_set_database   | utf8mb4                        |
     | character_set_filesystem | binary                         |
     | character_set_results    | utf8mb4                        |
     | character_set_server     | utf8mb4                        |
     | character_set_system     | utf8                           |
     | character_sets_dir       | /usr/share/mysql-8.0/charsets/ |
     +--------------------------+--------------------------------+
     ```

  7. 查看大小写敏感：

     ```mysql
     mysql> show global variables like '%lower_case%';
     +------------------------+-------+
     | Variable_name          | Value |
     +------------------------+-------+
     | lower_case_file_system | OFF   |
     | lower_case_table_names | 0     |
     +------------------------+-------+
      -- lower_case_table_names  --  0时，mysql会根据表名直接操作，大小写敏感；1，不敏感
      -- Linux默认是0
      -- Windows默认是1
      -- Mac默认是2，相当于0
     ```

  8. 查看默认端口：

     ```mysql
     mysql> show global variables like 'port';
     +---------------+-------+
     | Variable_name | Value |
     +---------------+-------+
     | port          | 3306  |
     +---------------+-------+
     ```

  9. 允许root通过远程客户端访问（可选）：

     ```mysql
     mysql> use mysql;
     mysql> update user set host='%' where user ='root';
     mysql> FLUSH PRIVILEGES;
     mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'WITH GRANT OPTION;
     -- mysql> GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'admin' WITH GRANT OPTION;
     mysql> FLUSH PRIVILEGES;
     ```

     防火墙：

     ( a )  简单可以直接停掉防火墙：

     `service iptables stop`

     `systemctl stop firewalld `

     ( b )  设置防火墙放行端口：

     `/sbin/iptables -I INPUT -p tcp -dport 3306 -j ACCEPT`

     `/etc/rc.d/init.d/iptables sava`

     `/etc/init.d/iptables status`

  10. 重启mysqld服务：

  11. 让普通用户无法使用mysql，只能root用户才能访问：

      ​      

  12. 导入SQL文件：

      （方法一）：

      ```mysql
      mysql> CREATE DATABASE tmall DEFAULT CHARACTER SET utf8;
      mysql>use tmall;
      mysql>set names utf8;
      mysql>source /root/tmall/tmall.sql;
      ```

      （方法二）：
      `mysql -u root -p admin < /root/tmall/tmall.sql`

+ 问题分析：

  1. 部署项目的mysql-connector驱动版本必须跟mysql的安装版本一致（5和8区别很大）
  2. 大小写敏感问题。（以后开发的时候遵循阿里巴巴java开发规范，SQL语句必须小写，表名必须小写）
  3. 账户root和密码admin必须和项目中的配置文件一致！



##### （1-2）二进制包

> 二进制安装MySQL无需编译，能够在一台机器上实现多个MySQL数据库。

+ 二进制安装：

  1. 下载：

     ( 1 )  [进入mysql官网下载页](https://dev.mysql.com/downloads/mysql/)

     ( 2 )  选择General Availability (GA) Releases栏（Archive栏是存放历史版本）

     ( 3 )  选择操作系统：Linux-Generic

     ( 4 )  选择OS版本：Linux-Generic（glibc 2.12）（x86,64-bit）

     ( 5 )  选择下载文件：Compressed TAR Archive    -->    (mysql-8.0.20-linux-glibc2.12-x86_64.tar.xz)

  2. 上传到服务器

  3. 解压缩（注意，这是.xz压缩形式）

##### （1-3）yum源安装

> 减少依赖包的繁琐下载

##### （1-4）源码包

[知乎有帖子](https://zhuanlan.zhihu.com/p/34566414)





### （2）安装JDK

[首先建议查看官方安装手册](https://docs.oracle.com/en/java/javase/14/install/installation-jdk-linux-platforms.html#GUID-737A84E4-2EFF-4D38-8E60-3E29D1B884B8)

+ JDK、JVM、JRE：

  JDK（Java Development Kit）

  JRE（Java Runtime Environment）

  JDK  ==  JRE  +  开发Java所需要的工具  ==  （JVM + 运行Java所需的核心内库） + 开发Java所需要的工具

  JRE  ==  JVM + 运行Java所需的核心内库。想要运行一个已有的java程序，安装JRE即可。

##### （2-1）rpm包

+ JDK安装（.rpm包）【必须root权限】：

  1. 查询本机是否安装其他版本的jdk：`which java`    `java --version`    `rpm -qa | grep jdk`

     如果已经安装低版本的，那么需要先卸载。

     JDK14可以和更早的版本共存。对于每一个版本都会在/usr/java/路径下创建不同的安装文件夹。

  2. 官网下载最新版rpm安装包（也可以下载更早版本，一般都是java8，这个是长期维护的老版本）

     下载文件：Linux RPM Package    --    [jdk-14.0.1_linux-x64_bin.rpm](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html#license-lightbox)

  3. 上传到zhao1-1目录下（因为只能ssh到它这里）：`scp -p /Users/apple/Downloads/jdk-14.0.1_linux-x64_bin.rpm zhao1-1@111.229.57.47:/home/zhao1-1/`

  4. 安装：`rpm -ivh jdk-14.0.1_linux-x64_bin.rpm`

  5. 升级：`rpm -Uvh jdk-14.0.1_linux-x64_bin.rpm`

  6. 查询安装路径（rpm安装无法指定路径，是打包者提前定义好的）：`rpm -qal | grep jdk | more`

  7. 检查是否成功安装：`java --version`

     如果没有成功，可以检查环境变量（正常情况rpm安装是无需配置环境变量的）。

  8. 安装成功后可以删除rpm安装包了：`rm -rf /home/zhao1-1/jdk-14.0.1_linux-x64_bin.rpm`

+ 卸载（.rpm包）：

  1. 查询完整包名：

     （方式一）`rpm -qa | grep jdk`

     （方式二）`$ rpm -q --whatprovides java`

  2. 卸载：`rpm -e jdk-14.0.1-14.0.1-ga.x86_64`

  3. 检查是否卸载完全：`rpm -qal | grep jdk`


##### （2-2）二进制包

+ JDK安装（二进制包）【可以是任何用户，放到任何位置，一般存放于U盘！】：

  1. 查询本机是否安装java，如果有旧版本，要先卸载
  2. 官网下载二进制包：Linux Compressed Archive    --    [jdk-14.0.1_linux-x64_bin.tar.gz](https://www.oracle.com/java/technologies/javase-jdk14-downloads.html#license-lightbox)
  3. 上传到服务器，并解压到/usr/java/目录下（解压后记得删除压缩包）
  4. 配置环境变量（JAVA_HONE和PATH）

+ 卸载（二进制包）：直接删除二进制安装包即可。

    

+ 配置环境变量：

  + `vim /etc/profile`

    1. 检测是否设置环境变量（14版之后不用设置了，已经加了软链接）

    2. 编写：

       ```shell
       set java environment
       # 存放java安装路径
       JAVA_HOME=/usr/java/jdk-14.0.1
       # ？？
       CLASSPATH=.:$JAVA_HOME/lib/
       # 把java的bin目录添加到现有的$PATH环境变量下
       PATH=$JAVA_HOME/bin:$PATH
       export JAVA_HOME CLASSPATH PATH
       ```

    3. 加载配置：`source ~/etc/profile`      

  + 与`vim ~/.bash_profile`两者区别：

    ( a )

    ( b )

（2-3）yum源安装：

`yum -y install java-1.8.0-openjdk.x86_64`



### （3）安装配置Tomcat

+ 各版本区别：

  [通过jdk版本来选择Tomcat的](https://tomcat.apache.org/whichversion.html)

  tomcat7

  tomcat8

  tomcat9

  建议7、8、9全都下载，装在/usr/tomcat/目录下，需要哪个启动哪个，反正都是二进制包。

+ 下载安装【二进制包】：

  1. [官网下载二进制包到本地](https://tomcat.apache.org/download-90.cgi)：Binary Distributions - Core    -->   apache-tomcat-9.0.34.tar.gz

     ~~注意：Source Code Distributions    --    这是源码包版本，一般只是学习用，不用来安装~~

     ( a )  Binary Distributions - Core    -->    正式的二进制发布版。【下载这个安装】

     ( b )  Binary Distributions - Deployer    -->    基于Tomcat的web应用的发布器，就是在把写好的JavaEE应用发布到Tomcat的时候可以使用Deployer来动态的发布。

     ( c )  Source Code Distributions    -->    Tomcat源码。

  2. 上传到服务器：`scp -p  /Users/apple/Downloads/apache-tomcat-9.0.34.tar.gz zhao1-1@111.229.57.47:/home/zhao1-1// `

  3. ~~（1，2两个步骤也可以用这个替换，相当于直接下载到服务器，这里选用的是清华的镜像源文件下载）~~

     `cd /usr/tomcat/`

     `wget https://downloads.apache.org/tomcat/tomcat-7/v7.0.103/bin/apache-tomcat-7.0.103.tar.gz`

  4. 解压到/usr/tomcat/文件夹下即可（解压后记得删除压缩包）：`tar -zxvf apache-tomcat-9.0.34.tar.gz -C /usr/tomcat`

  5. 无需设置环境变量。

  6. 建议安装完毕后，将Tomcat默认端口改为80端口！！

       

+ 启动：

  1. `cd /usr/tomcat/apache-tomcat-9.0.34/bin/`

  2. `./startup.sh`    --    （注意必须前面加`./`才能使用启动命令！）

     ```shell
     Using CATALINA_BASE:   /usr/tomcat/apache-tomcat-9.0.34
     Using CATALINA_HOME:   /usr/tomcat/apache-tomcat-9.0.34
     Using CATALINA_TMPDIR: /usr/tomcat/apache-tomcat-9.0.34/temp
     Using JRE_HOME:        /usr
     Using CLASSPATH:       /usr/tomcat/apache-tomcat-9.0.34/bin/bootstrap.jar:/usr/local/tomcat/apache-tomcat-9.0.34/bin/tomcat-juli.jar
     Tomcat started.
     ```

  3. 检查是否启动：

     ( a )  `netstat -anp | grep 8080`    --    查看端口号8080占用情况

     ( b )  `ps -ef |grep tomcat`    --    查看进程是否启动。

     ( c )  浏览器访问：`http://111.229.57.47:8080`，是否看到了Tom猫

  4. 如果服务器无法顺利开启：

     + 如果是启动时间特别长：

       ( 1 )  先关闭Tomcat

       ( 2 )  `yum -y install rng-tools`

       ( 3 )  `systemctl start rngd`

       ( 4 )  再启动Tomcat

     + 检查是否端口冲突？

       ( 1 )  检查80端口被哪个进程占用，被记录该进程号    --    `netstat -anp | grep :80`

       ( 2 )  通过查到的进程号杀死占用该端口号的进程    --    `kill -l PID`

     + 检查JAVA环境变量是否配置正确？

     + java版本是否过低？

     + 核查文件`startup.sh`的权限是否设置正确（755权限）。

       提示错误：No such file or directory 或 Permission denied

       `chmod 755 xxxx/bin/*.sh`

  5. 如果服务器可以顺利启动，但是浏览器无法访问：

     + 检查防火墙（需要注意是否安装iptables防火墙）

       ```shell
       #（1）检查防火墙状态
       $ service iptables status
       #（2）配置开放80端口
       vim /etc/sysconfig/iptables
       -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
       #（3）重启防火墙
       $ service iptables restart
       #（4）查看端口是否正常开启
       iptables -L -n
       ```

     + 如果用的是云服务器，检查安全组是否开通端口（腾讯云默认不开放8080端口）

       解决办法：

       方式一：修改Tomcat端口号为80

       方式二：修改腾讯云的安全组，[添加对外开放8080端口权限](https://www.linuxprobe.com/linux-install-tomcat9.html)

  6. 设置软连接（可选）：

       

  7. 设置服务开机自启动（可选）：

     ```shell
     #（1）
     $ cp /usr/tomcat/apache-tomcat-9.0.10/bin/catalina.sh /etc/init.d/tomcat
     
     #（2）
     $ vim /etc/init.d/tomcat
     #（2.1）在`#!/bin/sh`下加入
     ### BEGIN INIT INFO
     # Provides: tomcat
     # Required-Start: $remote_fs $network
     # Required-Stop: $remote_fs $network
     # Default-Start: 2 3 4 5
     # Default-Stop: 0 1 6
     # Short-Description: The tomcat Java Application Server
     ### END INIT INFO
     #（2.2）在分割线下加入环境变量
     JAVA_HOME=/usr/java/jdk-14.0.1
     export JAVA_HOME
     PATH=$JAVA_HOME/bin:$PATH
     export PATH
     CATALINA_HOME=/usr/apache-tomcat
     #（2.3）给该脚本设置权限
     $ chmod 755 tomcat
     
     #（3）添加tomcat服务（如果没有chkconfig命令，需要先yum安装）
     $ chkconfig --add tomcat
     #（3.1）查看是否添加成功
     $ chkconfig --list tomcat
     tomcat                    0:off  1:off  2:on   3:on   4:on   5:on   6:off
     #（3.2）如果345这三项为关闭状态。执行如下命令设置tomcat为开机自启动
     $ chkconfig tomcat on
     
     #（4）可以使用service命令了
     $ service tomcat start
     $ service tomcat stop
     $ service tomcat restart
     ```

+ 停止：

  （方法一）：`./shutdown.sh`

  （方法二）：

  ​	( 1 )  `ps -ef | grep tomcat`    --    查询进程号

  ​	( 2 )  `kill -l TomcatPID`    --    强制停止进程

+ 卸载：

  直接移除安装包即可，无任何多于的东西需要处理，非常方便。

  `rm -rf /usr/tomcat/apache-tomcat-9.0.34/`

+ 配置：

  `vim /usr/tomcat/apache-tomcat-9.0.34/conf/server.xml`

  ```xml
  <!-- 端口配置 -->
  <Connector port="8080" protocol="HTTP/1.1"
             connectionTimeout="20000"
             redirectPort="8443" />
  
  <!-- 项目部署（一般不采用） -->
  <!-- docBase：项目存放的路径 -->
  <!-- path：虚拟目录 -->
  <Context docBase="/User/apple/Documents/zhaobinweb" path="/hehe" />
  ```

+ 项目部署：

  1. 将项目打成一个war包，再将war包放置到/usr/tomcat/apache-tomcat-9.0.34/webapps/目录下

  2. 在`/usr/tomcat/apache-tomcat-9.0.34/conf/Catalina/localhost/`路径下创建任意名称（这里起名tmall.xml）的xml文件，在文件中编写：

     ```xml
     <!-- docBase="项目所在路径" -->
     <!-- 虚拟目录：该xml文件的名称，这里即tmall -->
     <Context docBase="/usr/local/tomcat/apache-tomcat-9.0.34/webapps/ROOT" />
     ```

+ 查看运行日志：

  `tail -50f /usr/tomcat/apache-tomcat-9.0.34/logs/catalina.out`



### （4）安装Redis

+ 只能源码安装：
  1. [官网下载源码包]()：
  2. 上传到服务器：



### （5）安装Nginx

+ 只能源码安装：
  1. 官网下载并上传到服务器：
  2. 



### （6）本地Maven工程部署到Linux服务器

