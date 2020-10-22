## 10.8 其他神器

### （1）tmux

**（1-1）配置：**



**（1-2）使用：**

【1】Session管理 --  终端下操作

1. 显示session信息：
   + `tmux ls`　　显示会话列表
2. 新建session：
   + `tmux new`　　创建默认名称的会话（在tmux命令模式使用new命令可实现同样的功能，其他命令同理，后文不再列出tmux终端命令）
   + `tmux new -s PythonLearing`　　创建名为PythonLearing的会话
3. 回到/连接session：
   + `tmux a`　　连接上一个会话
   + `tmux a -t PythonLearing`　　连接指定名称的会话
4. 重命名会话：
   + `tmux rename -t s1 s2`　　重命名会话s1为s2
5. 关闭会话：
   + `tmux kill-session`　　关闭上次打开的会话
   + `tmux kill-session -t s1`　　关闭会话s1
   + `tmux kill-session -a -t s1`　　关闭除s1外的所有会话
   + `tmux kill-server`　　关闭所有会话

【1】Session管理 --  进入session后操作

6. 列出全部session：
   + `prefix s`
7. 重命名session：
   + `prefix $`
8. 分离session：
   + `prefix d`  --  分离当前session
   + `prefix D`  --  分离指定session

【2】Window管理

1. 列出所有window：
   + `prefix w`  --  列出所有窗口，可进行切换
2. 新建window：
   + `prefix c`  --  新建window
3. 关闭window：
   + `prefix &`  --  关闭当前窗口
4. 切换window：
   + `prefix n`  --  进入下一个窗口
   + `prefix p`  --  进入上一个窗口
   + `prefix l`  --  进入之前操作的窗口
   + `prefix 0~9`  --  选择编号0~9对应的窗口
   + `prefix .`  --  修改当前窗口索引编号
   + `prefix ‘`  --  切换至指定编号（可大于9）的窗口
5. 其他：
   + `prefix ,`  --  重命名当前窗口
   + `prefix f`  --  根据显示的内容搜索窗格

【3】Pane管理

1. 创建Pane：
   + `prefix %`  --  水平方向创建窗格
   + `prefix “`  --  垂直方向创建窗格
2. 关闭Pane：
   + `prefix x`  --  关闭当前窗格
3. 切换Pane：
   + `prefix q`  --  显示窗格编号
   + `prefix Up|Down|Left|Right`  --  根据箭头方向切换窗格
   + `prefix o`  --  顺时针切换窗格
4. 变换Pane位置：
   + `prefix }`  --  与下一个窗格交换位置
   + `prefix {`  --  与上一个窗格交换位置
   + `prefix space(空格键)`  --  重新排列当前窗口下的所有窗格
   + `prefix Ctrl+o`  --  逆时针旋转当前窗口的窗格
   + `prefix !`  --  将当前窗格置于新窗口
5. 其他：
   + `prefix t`  --  在当前窗格显示时间
   + `prefix z`  --  放大当前窗格(再次按下将还原)
   + `prefix i`  --  显示当前窗格信息

【4】Help

1. `tmux list-key`  --  列出所有绑定的键，等同于prefix ?
2. `tmux list-command`  --  列出所有命令





### （2）ranger

（2-1）配置

1. [rangerWiki](https://github.com/ranger/ranger/wiki)
2. 终端下输入：`ranger --copy-config=all`
3. 更改全局变量：`export RANGER_LOAD_DERAULT_RC FALSE`
4. 进入：`~/.config/ranger`

   + `commands.py`  --  可以用Python给ranger编辑各种脚本
   + `commands_full.py`
   + `rc.conf`  --  主要的配置文件
   + `rifle.conf`  --  打开哪个文件用哪个程序（默认打开程序）
   + `scope.sh`  --  预览脚本（如何预览不同的文件）
   + `colorschemes/`  --  后添加的，存放主题
   + `plugins/`  --  后添加的，存放插件
5. 插件安装：

   + [安装说明地址](https://github.com/ranger/ranger/wiki/Plugins)
   + git annex plugin
   + plugin directory diff
   + Ranger Devicons plugin ( file icon glyphs for ranger)（ranger不同文件前面有不一样的图标）
     1. 先安装：[NERDfonts](https://github.com/ryanoasis/nerd-fonts)
     2. `git clone https://github.com/alexanderjeurissen/ranger_devicons ~/.config/ranger/plugins/ranger_devicons`
     3. `echo 'default_linemode devicons' >> rc.conf`
   + Autojump integration for ranger
   + feedranger (feed reader using ranger as a UI)
   + fzf-marks (fuzzy bookmark with fzf)
   + ranger-cmus (integration with cmus audio player)



（2-2）自带快捷键：

【1】跳转移动命令：

+ h j k l  --  同vim一样的上下左右
+ ][  --  父文件夹目录导航移动
+ g  --  跳转
  + gl  --  
  + gL  --  
  + gr  --  跳转到 / 根目录
  + g/  --  跳转到 / 根目录
  + gh  --  跳转到 ~ 家目录
  + ge  --  跳转到 /etc
  + gu  --  cd /usr
  + gv  --  cd /var
  + gd  --  cd /dev
  + go  --  cd /opt
  + g?  --  cd /usr/share/doc/ranger
  + gt  --  
  + gT  --  
  + gn  --  
  + gc  --  
  + gg  --  
+ H  --  
+ L  --  
+ S  --  终端进入当前文件夹

【2】排序：

+ o  --  排序
  + os  --  按照文件大小排序（大 --> 小）
  + oS --  按照文件大小排序（小 --> 大）
  + oc  --  按照修改日期排序
  + on --  按照文件名排序
  + ot --  文件类型
  + oa
  + ob
  + oe
  + om
  + oz  --  随机排序

【3】搜索：

+ /  --  搜索文件
  + n  N  --  搜索下一个、上一个
+ fzf插件

【4】文件信息查询

+ dU  --  显示当前文件目录里每个文件夹都是多大

【5】删除、剪切、复制、粘贴：

+ v  --  当前文件夹下所有全部选中
+ 空格  --  选定当前文件（文件夹）

+ y  --  复制
  + yy  --  复制当前文件
  + yp  --  复制当前文件路径
  + yn  --  复制文件名
  + y.  --  复制去掉后缀的文件名
+ p  --  粘贴
  + pp  --  在当前路径下粘贴文件
  + pp  --  如果当前文件已经存在相同的名字，则给你在文件最后加下划线重命名
  + po  --  覆盖当前名字一样的文件
+ d  --  删除/剪切
  + dd  --  剪切当前文件
  + dD  --  删除当前文件/文件夹

【6】修改文件名：

+ c  --  更改
  + cw  --  重命名文件
+ a  --  在文件名最后（不包含后缀）开始编辑，并重命名
+ A  --  在文件名最后（包括后缀）开始编辑，并重命名
+ I  --  在文件名最开始编辑，并重命名  
+ `:rename new_name`  --  重命名文件（编辑的操作方式同vim）
+ :bulkername  --  打开vim，并把所有文件名在vim下操作

【7】新建文件/文件夹：

+ `:shell vim file_name`  --  用vim打开一个文件（没有的话则在当前路径下新建）
+ `:mkcd filefold_name`  --  新建一个文件夹

【8】其他：

+ zh  --  打开/关闭隐藏文件
+ q  --  退出ranger
+ w  --  任务管理器
  + dd  --  任务管理器下取消任务
  + q  --  退出任务管理器



### （3）tree

> 查看当前目录下的树状结构



### （4）screenfetch

### （5）fzf（模糊搜索神器）

### （6）autojump

### （7）figlet

### （8）给你的Linux系统设置一个垃圾箱

### （9）tldr

>（Too Long and Don’t Read）
>
>【牛逼的man工具】，用于查询命令是如何使用的，把命令的使用步骤简化阐述

### （10）tlp

> 电源管理，帮你省电



### 可视化需要安装的软件：

1. `sudo pacman -S screenkey` 捕捉你输入的所有按键内容并呈现至屏幕
2. `sudo pacman -S simplescreenrecorder` 录屏软件
3. `sudo pacman -S code` VSCode
4. `sudo pacman -S dmenu`
5. polybar，i3等窗口管理器可能需要，用于电脑下边的信息栏，配置很麻烦
6. `sudo pacman -S xorg`改硬件设置，键盘键位
7. `sudo pacman -S lxappearance` 更改窗口主题
8. `sudo pacman -S feh` 更改壁纸
9. `sudo pacman -S variety` 利用feh来对壁纸进行管理的管理器（在i3设置文件设置开机启动）
10. `sudo pacman -S compto` 渲染器,达成各种渲染效果（设置开机启动）
    10.`sudo pacman -S fcitx fcitx-im fcitx-configtool` 
11. `sudo pacman -S fcitx-sunpinyin`
    `vim ~/.xprofile`
    `export GTK_IM_MODULE=fcitx`
    `export QT_IM_MODULE=fcitx`
    `export XMODDIFIERS="@im=fcitx"`
    reboot
12. `sudo pacman -S chromium`
13. `sudo pacman -S kdenlive` shi pin jan ji
14. `sudo pacman -S gimp`
15. `sudo pacman -S vlc`
16. `sudo pacman -S virtualbox`
17. `sudo pacman -S deepin.com.qq.im`
18. `sudo pacman -S electronic-wechat`
19. `sudo pacman -S transmission-qt`
20. qbittorrent

