

## 10.4 SSH

### （1）命令和配置

> ssh连接：是一种使用Secure Shell（ssh）协议的客户端，去连接到运行了ssh协议的服务端的远程服务器上。
>
> OpenSSH：是ssh协议的免费开源实现。提供了服务端程序（openssh-server）和客户端工具（openssh-client）。一般情况下我们安装的openssh是客户端和服务端都安装的。
>
> Mac和Linux中默认已安装ssh客户端，Windows需手动安装ssh客户端。

+ 【命令】：

  1. `ssh 用户名@ip`    --    连接命令

     `-p`  --  指定ssh端口号，默认端口为22

     `-i`  --  使用指定私钥文件链接服务器（免密登录）

  2. `ssh-keygen`    --    秘钥生成命令

     `-t`  --   指定密钥类型，默认是 rsa ，可以省略。

     `-C`  --   设置注释文字，比如邮箱。

     `-f`  --   指定密钥文件存储文件名，默认在 ~/.ssh/文件夹下

  3. `ssh-copy-id -i ~/.ssh/id_rsa.pub userName@ipAddress`    --    发送公钥命令

       

+ 【配置】：

  1. ssh各用户配置（分别在每个用户的家目录）：`~/.ssh`

     + known_hosts：作为客户端，记录曾连接服务器授权。ssh第一次连接一台服务器会有一个授权提示，确认授权后会记录在此文件中，下次连接记录中的服务器时则不再需要进行授权确认提示。
     + authorized_keys：作为服务端。客户端的免密连接公钥文件在这个
     + config：作为客户端。记录连接服务器配置的别名。
     + id_rsa：免密连接私钥文件（自己保留，一定保密！）
     + id_rsa.pub：免密连接公钥文件（传输给服务端的）

  2. ssh总配置：`/etc/ssh/sshd_config`

     更改完这个配置文件后，若想生效必须重启ssh服务端！

     ```shell
     # 更多关于这个配置文件的使用方法，建议搜索！
     #############1. 关于 SSH Server 的整体设定##############
     Port 22    
     ##port用来设置sshd监听的端口，为了安全起见，建议更改默认的22端口为5位以上陌生端口
     Protocol 2
     ##设置协议版本为SSH1或SSH2，SSH1存在漏洞与缺陷，选择SSH2
         
     #############3.1、有关安全登录的设定###############
     Authentication:120
     ##限制用户必须在指定的时限内认证成功，0 表示无限制。默认值是 120 秒。
         
     LoginGraceTime 2m
     ##LoginGraceTime用来设定如果用户登录失败，在切断连接前服务器需要等待的时间，单位为妙
         
     PermitRootLogin yes
     ##PermitRootLogin用来设置能不能直接以超级用户ssh登录，root远程登录Linux很危险，建议注销或设置为no
         
     StrictModes yes
     ##StrictModes用来设置ssh在接收登录请求之前是否检查用户根目录和rhosts文件的权限和所有权，建议开启
     ###建议使用默认值"yes"来预防可能出现的低级错误。
         
     RSAAuthentication yes
     ##RSAAuthentication用来设置是否开启RSA密钥验证，只针对SSH1
         
     PubkeyAuthentication yes
     ##PubkeyAuthentication用来设置是否开启公钥验证，如果使用公钥验证的方式登录时，则设置为yes
         
     AuthorizedKeysFile     .ssh/authorized_keys
     ##AuthorizedKeysFile用来设置公钥验证文件的路径，与PubkeyAuthentication配合使用,默认值是".ssh/authorized_keys"
     ```



### （2）ssh原理及加密算法

（2-1）加密算法

+ 对称加密算法：

  + 单秘钥加密。
  + 采用单秘钥密码系统的加密算法，同一个秘钥可以同时用作信息的加密和解密。

+ 非对称加密算法：

  + 公开秘钥加密算法;
  + ssh就用的这个！

（2-2）哈希算法

（2-3）SSH密码工作原理

（2-4）SSH免密码登录工作原理

+ SSH免密码登录原理说明：

  1. 本地客户机通过`ssh-keygen -t rsa`命令在本地创建`id_rsa`和`id_rsa.pub`两个文件；
  2. 将`id_rsa.pub`的公钥内容拷贝到服务器的`~/.ssh/authorized_keys`文件中；
  3. 本地ssh客户端向服务器端发送连接请求；登录格式：`ssh user_name@ip_address`
  4. 服务器根据请求的user_name在对应的authorized_keys文件下查找对应的公钥，如果存在这个客户端的信息，则服务器端发送一个随机的字符码；
  5. 本地客户端使用本地的私钥对服务器端发送过来的随机字符码进行加密；
  6. 本地客户端通过公共网络向服务器端发送加密过后的信息；
  7. 服务器端使用公钥对该信息进行解密；
  8. 若解密之后的信息和之前发送的信息匹配，则信任客户端，否则不信任。

+ SSH免密登录简述：

  1. 本地：`id_rsa`和`id_rsa.pub`
  2. 本地[id_rsa.pub]  ---拷贝---》 服务器[authorized_keys]
  3. 本地`ssh user@ip`
  4. 服务器：在authorized_keys找到本地的信息；并发送一个【随机字符码B】
  5. 互联网传送【随机字符码B】
  6. 本地：【随机字符码B】  ---私钥---》 【密文B'】
  7. 互联网传送【密文B'】
  8. 服务器：【密文B’】  ---公钥---》 【随机字符码B】
  9. 与发送的比较，如果一致，则验证通过。



### （3）配置ssh免密码登录服务器

ssh标准安全服务器登录法：

1. 不允许root用户被ssh远程登录；/etc/ssh/sshd_config    --    PermitRootLogin no
2. 设置一个普通用户，以后只能ssh免密码或者使用密码登录这个普通用户。
3. 不要给这个普通用户设置root权限！他的用途就是为了远程登录用。通过设置 /etc/sudoers，控制权限。
4. ssh以后只能通过这个普通用户登录到服务器，然后登录进去后再`su root`，输入root密码切换root用户操作。

（3-1）服务器端操作：

```shell
# 以下操作都是在root用户下。

#（0.1）查询我们当前的服务器版本：
$ cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
#（0.2）查询服务器ip地址。注意：腾讯云提供公网ip（111.229.57.47）
$ ip addr | grep inet
# 我的mac主机ip地址  --  sshd:192.168.2.206
# 我的腾讯云ip地址  --  sshd:111.229.57.47


#（1）检查ssh是否安装
$ rpm -qa | grep openssh*
#（1.1）输出下面内容表示已经安装，如果无内容输出，表示未安装。
openssl-1.0.2k-19.el7.x86_64
openssh-7.4p1-21.el7.x86_64
openssh-clients-7.4p1-21.el7.x86_64
openssh-server-7.4p1-21.el7.x86_64
openssl-libs-1.0.2k-19.el7.x86_64
#（1.2）如果没有安装，那么安装openssh，选择当前系统对应的下载器，我们这里以Redhat的yum为例。
$ yum -y install openssh


#（2）查看ssh服务是否开启
$ netstat -tlp | grep ssh
#（2.1）输出下面内容表示已经开启，无内容输出表示没有开启。
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN      1411/sshd
#（2.2）如果没有开启，那么手动启动ssh服务
$ systemctl start sshd.service
#（2.3）设置开机启动ssh服务
$ systemctl enable sshd.service


#（3）检查计算机远程登录权限（ArchLinux默认不允许其他计算机远程登录）
$ vim /etc/hosts.allow
#（3.1）允许所有计算机ssh远程登录
sshd: ALL : ALLOW
# (3.2) 允许特定主机连接
sshd:192.168.2.206:ALLOW
#（3.3）禁止所有计算机远程登录
$ vim /etc/hosts.deny


#（4）禁止root用户被ssh远程登录（Ubuntu默认不允许root帐户直接远程登录）
#（4.1）检查服务器是否有root用户，如果没有需要添加root用户，root用户设置你常用密码即可（见手机备忘录）
$ su root
#（4.2）修改ssh的配置文件：
# 将PermitRootLogin后改成no，并将其前的“#”号去掉就可以了。
$ sudo vim /etc/ssh/sshd_config
PermitRootLogin no
#（4.3）检查sshd_config文件的其他配置是否符合规范！！！
StrictModes no
RSAAuthentication yes
PubkeyAuthentication yes
PasswordAuthentication no
#（4.4）重启ssh服务
$ sudo systemctl restart sshd


#（5）创建ssh登录专用用户
#（5.1）创建一个普通用户
$ useradd zhao1-1
$ passwd zhao1-1
设置一个复杂的密码用于登录！！（密码见手机备忘录）
#（5.2）取消该用户的root权限，即不能使用sudo命令：（本质就是修改/etc/sudoers文件）
$ visudo
#（只有root这一行，没有其他的用户）
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
#（5.3）检查root权限是否取消
$ su zhao1-1
$ sudo -l
对不起，用户 zhao1-1 不能在 VM_0_9_centos 上运行 sudo。
$ su root


#（6）服务端执行完（2）步操作后：
# 注意了！接下来.ssh/这个文件夹不能放到/root下，而是要在/home/zhao1-1/目录下！！！
# (6.1) 查看免密码授权文件，并核查是否与客户端的id_rsa.pub文件一致。
$ cat /home/zhao1-1/.ssh/authorized_keys 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDE5H3L9PKmsw236Fe2uVpOyWyOjdkgiW+mMyqfElGnnoXiliVIS+/ydgPCUf9G15mXiJuc4u0/UbILOF6vGPhzTR9exKE96M15RwzHIBvcbPwZ7mhsSYxFG47LQ3LvrBKmOqCv1YtvLP2SeWjSgC94kJpdv0bfOHgOD/M8vI2B7vbWxll+7BlCElsEOXnuDw9ZKPfTyxArBIImC+dbCeNhYw/KJBj6K0gfVsRHyoD8JJemigz0FsxIsKUEO8td0sm7SsHzQYfHitmrp2XqMXHwH2iS566pchrjdDUtUGmkJv67vLcBjOsF0sG8nMlCaoa8fCG2csF+UVB7+b9jRPz/ 798998599@qq.com
#（6.2）检查文件夹和文件权限：
$ ls -al
drwx------  2 zhao1-1 zhao1-1 4096 5月   6 14:55 .ssh/
-rw-------  1 zhao1-1 zhao1-1  398 5月   6 14:55 authorized_keys
#（6.3）如果不符合上述权限，则调整
#（一般用ssh-copy-id命令拷贝公钥则权限符合，即.ssh文件夹和authorized_keys文件都是自动生成的；）
#（其他方式需要手动创建`mkdir .ssh/`和`touch authorized_keys`，则一般都需要修改权限！）
$ chmod 700  /home/zhao1-1/.ssh/
$ chmod 600  /home/zhao1-1/.ssh/authorized_keys
## 注意：如果不更改权限，很可能ssh无法免密码登录，永远得输入密码！！


#（7）服务端若也想ssh到其他服务器，就需要把自己当成客户端。
#（7.1）生成ssh公私密钥（生成的过程中一直回车即可，问你需不需要设置密码，我们不需要！）
$ ssh-keygen -t rsa
#（7.2）查询生成文件：默认生成在 /root/.ssh/
$ ls -al /root/.ssh
```



（3-2）本地客户端设置

```shell
# (0.1) ssh安装检查，如果无内容输出，表示未安装。
$ rpm -qa | grep openssh*
# (0.2) 安装openssh
$ yum -y install openssh
# (0.3) 生成ssh公私密钥（生成的过程中一直回车即可，问你需不需要设置密码，我们不需要！）
#    默认会在 ~/.ssh/下生成一对文件
$ ssh-keygen -t rsa
$ ls -al /root/.ssh
-rw-r--r--   1 apple  staff   200  5  5 17:22 config
-rw-------   1 apple  staff  1823 11 24 17:05 id_rsa
-rw-r--r--   1 apple  staff   398 11 24 17:05 id_rsa.pub
-rw-r--r--   1 apple  staff  1721  5  5 16:32 known_hosts


# (1) 将本地的公钥通过scp命令拷贝到服务器的authorized_keys文件内（如果你之前已经配置了其它服务器上的密钥，使用这种方法，就会覆盖掉你原来的密钥）。
# 注意：如果你有多个用户需要免密码登录，则需要分别在这些用户的家目录下操作。（root用户和普通用户需要分别设置）
# 方法一：
$ scp -p ~/.ssh/id_rsa.pub zhao1-1@111.229.57.47:/home/zhao1-1/.ssh/authorized_keys
# 方法二（强烈建议！）：
$ ssh-copy-id -i ~/.ssh/id_rsa.pub zhao1-1@111.229.57.47
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/apple/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@111.229.57.47's password:
Number of key(s) added:        1
Now try logging into the machine, with:   "ssh 'zhao1-1@111.229.57.47'"
and check to make sure that only the key(s) you wanted were added.
# 方法三：直接手工拷贝


#（2）设置~/.ssh/known_hosts（初次用ssh登录会自动向这个文件添加内容，再次登录则不会校验了）
#（2.1）首次登录：
ssh -p 22 -i ~/.ssh/id_rsa zhao1-1@111.229.57.47
# 注意：-p 和 -i 选项都可以不用填，默认就是22端口，和~/.ssh/id_rsa
The authenticity of host '111.229.57.47 (111.229.57.47)' can't be established.
ECDSA key fingerprint is SHA256:lDUhxmkBvbUaq5fo5Kbf/gCv1GjmFYW5MMFPkTDlyjI.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '111.229.57.47' (ECDSA) to the list of known hosts.
zhao1-1@111.229.57.47's password:
# 输入密码即可登录
#（2.2）查看
$ vim ~/.ssh/known_hosts
111.229.57.47 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBGUrRi4LOsrestfr9J71ctyr7ruGOBIBBTiMrGawBw8ZdtJX7XuMSnCYTjrA5xEn82WtgldDS8FLf5WOengAKIo=


#（3）使用别名代替IP地址进行ssh远程登录
$ vim ~/.ssh/config
Host zhao1-1
    HostName 111.229.57.47
    User zhao1-1
    # 以下内容系统会给默认值，也可自行修改
    Port 22
    IdentityFile ~/.ssh/id_rsa
    AddKeysToAgent yes
  	UseKeychain yes
```



（3-3）故障排查

```shell
# 故障：
$ ssh zhao1-1@192.168.2.132
The authenticity of host '192.168.2.132 (192.168.2.132)' can't be established.
ECDSA key fingerprint is SHA256:DkyalG9YlIGusPeHZilq/SaOQkEVfVvHo0PLRgM+d98.
Are you sure you want to continue connecting (yes/no)?
Host key verification failed.

# 解决：
$ sudo ssh  -o StrictHostKeyChecking=no  192.168.2.132
# 先输入sudo系统密码
# 再输入登录主机的root密码
successfully login
```



### （4）Windows的ssh客户端

+ SecureCRT

+ Xshell

+ Putty

+ WinSCP

+ mobaXterm



###（5）scp文件传输工具

（5-1）scp文件传输命令（UNIX）

>scp 是 secure copy 的缩写, scp 是 linux 系统下基于 ssh 登陆进行安全的远程文件拷贝命令。
>
>scp 是加密的，rcp 是不加密的，scp 是 rcp 的加强版。

`scp [-r] [用户名@ip:文件路径] [本地路径]`	  --    Linux服务器下载文件

`scp [本地文件] [用户名@ip:上传路径]`	  --    Mac上传文件

+ 参数说明：

  + -1： 强制scp命令使用协议ssh1
  + -2： 强制scp命令使用协议ssh2
  + -4： 强制scp命令只使用IPv4寻址
  + -6： 强制scp命令只使用IPv6寻址
  + -B： 使用批处理模式（传输过程中不询问传输口令或短语）
  + -C： 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
  + -p：保留原文件的修改时间，访问时间和访问权限。
  + -q： 不显示传输进度条。
  + -r： 递归复制整个目录。
  + -v：详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
  + -c cipher： 以cipher将数据传输进行加密，这个选项将直接传递给ssh。
  + -F ssh_config： 指定一个替代的ssh配置文件，此参数直接传递给ssh。
  + -i identity_file： 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。
  + -l limit： 限定用户所能使用的带宽，以Kbit/s为单位。
  + -o ssh_option： 如果习惯于使用ssh_config(5)中的参数传递方式，
  + -P port：注意是大写的P, port是指定数据传输用到的端口号
  + -S program： 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项。

+ 传输权限：

  ```
  scp ./xxx文件 zhao1-1@111.229.57.47:/usr
  scp: /usr/xxx文件: Permission denied
  ```

  当出现Permission denied时，说明我们是不能直接向root管理的文件夹传输文件，如/usr等。

  解决办法：`scp ./xxx文件 zhao1-1@111.229.57.47:/home/zhao1-1`    向/home/zhao1-1/用户的文件夹传输，或者是/tmp目录下！





（5-2）lrzsz（Linux-RedHat）

+ 需要yum安装，也是可以图形化操作，有点类似于同步盘！
+ 需要在SecureCRT中设置上传和下载目录。
+ `rz`  --  上传
+ `sz`  --  下载



（5-3）filezilla（win桌面版）



（5-4）sftp

