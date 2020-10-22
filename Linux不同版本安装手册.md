## 10.1 Linux各系统安装

### 各版本发展及相互间演化关系



### （0）安装环境

##### （0-1）启动盘及BIOS制作

+ 启动磁盘制作：

  + 软件：etcher

+ BIOS：

  1. 进入BIOS：

     不同的主板都是不一样的：需要搜索电脑型号

     + F2：Lenovo、Dell
     + DEL
     + ESC
     + F1
     + Fn+f12：ThinkPad

  2. 更改启动盘顺序U盘在前：

  3. 再把启动盘修改会硬盘



##### （0-2）虚拟机

+ VMware
+ VirtualBox



##### （0-3）云服务器

+ 阿里云
+ 腾讯云





### （1）RedHat（Centos）

+ RedHat：
  + Red Hat Enterprise Linux 8；
  + Fedora；
  + CentOS；



### （2）Ubuntu



### （3）Debian



### （4）Arch

1. 制作启动盘并进入BIOS修改启动盘

2. 进入安装盘（启动U盘）界面，做一些基础设置：

   + 安装字体：`setfont /usr/share/kbd/consolefonts/LatGrkCyr-12x22.psfu.gz`

   + 更改键盘布局为colemak：`loadkeys colemak`

   + 键盘布局配置文件：

     ```shell
     #（1）任意位置用vim创建文件keys.conf
     #（2）编写 【Esc和Caps两个键互换位置】
     keycode 1 = Caps_Lock
     keycode 58 = Escape
     #（3）保存退出回到shell
     loadkeys keys.conf
     ```

   + 连接网络：见10.2（1）

     arch的安装盘中自带 wpa_supplicant 和 dhcpcd工具。

   + 更新系统时间：`timedatectl set-ntp true`

3. 在安装盘进行系统分区：

   + 查询当前磁盘设备都有神马：`fdisk -l`

   + 进入需要分区的主硬盘（只会更改这个磁盘）：`fdisk /dev/mmcblk1`

   + 输入完上述命令后，即进入fdisk这个磁盘管理工具软件，输入m查看使用方法，按说明操作即可。

   + `P`，查看当前硬盘已经有的分区。

   + `g`，清干净当前硬盘的所有分区。

   + 使用时候要查询archLinux提供的官方分区文档（确定机器是否支持uefi，还是只支持BIOS的mbr？）：

     以支持uefi为例分区：

     1. boot：引导分区，一般260~512M
     2. 主分区：如果是固态和机械盘的话可以把主分区再分一下。
        + 系统区：
        + 假分区：
     3. swap：虚拟内存，一般1G~8G

   + `w`，写入分区并退出fdisk

   + 定义分区格式：

     + 设置引导分区为fat文件格式：`mkfs.fat -F32 /dev/mmcblk1p1`

     + 设置主分区为ext4文件格式：`mkfs.ext4 /dev/mmcblk1p2`

     + 设置swap分区：

       `mkswap /dev/mmcblk1p3`

       `swapon /dev/mmcblk1p3`

4. 安装盘内配置packman镜像服务器：

   ```shell
   #（1）vim /etc/pacman.conf
   #（2）去掉`#Color`这个注释
   #（3）找到配置镜像源的文件位置
   [community]
   Include= /etc/pacman.d/mirrorlist
   #（4）vim /etc/pacman.d/mirrorlist，这个文件存放各个国家服务器的位置，找到所有## China的
   ## China
   Server = http://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch
   ## China
   Server = http://mirrors.163.com/archlinux/$repo/os/$arch
   .......总共是七个...当然你也可以添加自己的服务器源.....
   #（5）把找到的这些剪切到文件最顶层（因为默认寻找是自顶向上依次寻找镜像源）
   ```

5. 系统盘挂载到当前安装环境下（不然的话会直接安装在启动U盘里）：

   + 创建boot文件夹：`mkdir /mnt/boot/`
   + 查看分区：`fdisk -l`
   + 挂载主分区：`mount /dev/mmcblk1p2 /mnt`
   + 挂载系统引导分区：`mount /dev/mmcblk1p1 /mnt/boot`

6. 开始安装ArchLinux：

   `pacstrap /mnt base linux linux-firmware`

7. 生成fstab文件：`genfstab -U /mnt >> /mnt/etc/fstab`

8. 本地化基本设置：

   + 更改编码：`vim /mnt/etc/locale.gen`

     去掉`en_US.UTF-8 UTF-8`这个注释

   + 设置系统语言：vim /mnt/etc/locale.conf

     `LANG = en_US.UTF-8`

   + 更改键盘布局：vim /mnt/etc/vconsole.conf

     KEYMAP = colemak
     keycode 1 = Caps_Lock
     keycode 58 = Escape

   + 设置hostname：`vim /mnt/etc/hostname`，里面写入你想要机器在网络中的名字

   + 设置hosts：`vim /mnt/etc/hosts`

     ```
     127.0.0.1	localhost
     ::1			localhost
     127.0.0.1	zhaobin.localdomain  zhaobin
     ```

9. 进入安装好的系统：

   `arch-chroot /mnt`

   + 输入这个命令之后，就相当于在安装好的Arch系统内执行命令了！

   + 注意：这个系统下可是啥都没有呢，包括vim，网络配置工具啥的。所以需要编辑文件等操作一定要在启动盘的系统内完成（因为启动盘自带了很多工具如vim、dhcpcd等）

10. 接下来一直到12步都是在这个安装好的系统内完成。

    + 生成本地化：`locale-gen`
    + 创建时区：`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`
    + 同步系统时间：`hwclock --systohc`
    + 创建root密码：`passwd``

11. 制造启动引导程序：

    1. `pacman -S grub efibootmgr intel-ucode os-prober`
    2. 创建系统引导文件夹：`mkdir /boot/grub`
    3. 生成grub配置：`grub-mkconfig > /boot/grub/grub.cfg/`
    4. 确认系统架构：`uname -m`
    5. 安装对应架构的grub：`grub-install --target=x86_64-efi --efi-directory=/boot`

12. 因为这个系统啥都没有，所以需要用pacman安装你需要的东西：

    + `pacman -S vim zsh wpa_supplicant vi dhcpcd`
    + vim，网络配置工具等都需要下载安装。

13. 退出chroot环境：`exit`

    退出后再关掉一些程序：`killall wpa_supplicant dhcpcd`

    重启：`reboot`

14. 拔掉USB启动盘，进入安装好的新系统尽情玩耍吧。

    注意：开机之后一定要再次设置一下网络，并连接WiFi哦！（因为之前的网络是在安装盘下的系统设置的）



###（5）Manjaro

1. 官网下载安装版本：

   1. Manjaro XFCE版 （轻量）
   2. Manjaro KDE版
   3. Manjaro GNOME版（推荐）

2. rufus制作启动盘（Manjaro需要用DD模式来安装）

3. 进入BIOS，设置启动方式：

   + 更换U盘为启动盘;（将启动级别调到最高）
   + 保存重启;
   + 注意：安装好之后一定要记得把启动盘从U盘设置回硬盘！！！

4. 进入安装界面，一顿下一步。

5. 选择分区方法：

   + 双系统：
   + 自定义：
     1. /
     2. /home/
     3. swap
     4. /boot/

6. 安装可能要很久，安装好之后重启，切记拔掉U盘

7. 设置pacman镜像源：

   ```shell
   #（0）改造全部国内镜像源：`sudo pacman-mirrors -c China`
   #（1）nano /etc/pacman.conf
   #（2）先将36行Color选项的注释去掉，这样pacman就有颜色高亮了;
   #（3）找到配置镜像源的文件位置
   [archlinuxcn]
   Include = /etc/pacman.d/archlinuxcn
   
   #（4）nano /etc/pacman.d/archlinuxcn，这个文件存放各个国家服务器的位置，找到所有## China的
   ## China
   Server = http://mirrors.neusoft.edu.cn/archlinux/$repo/os/$arch
   ## China
   Server = http://mirrors.163.com/archlinux/$repo/os/$arch
   ## China
   Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
   .......总共是七个...当然你也可以添加自己的服务器源.....
   
   #（5）把找到的这些剪切到文件最顶层（因为默认寻找是自顶向上依次寻找镜像源），保存退出即可。
   ```

8. 更新系统（时间会有点久）

   `sudo pacman -Syyu`

9. 重启即可



###（6）UNIX和MacOS

