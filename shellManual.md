

## 10.6 shell

### （0）shell编程

+ 查询系统下的所有shell：`cat /etc/shells`
+ 更改当前用户的shell：`chsh -s /usr/bin/fish`
+ 查询当前所用的shell：`env | grep SHELL`



### （1）bash



### （2）zsh

（2-1）oh,myzsh

（2-2）alias



### （3）fish

（3-1）下载安装并配置fish：

1. `sudo pacman -S fish`
2. `which fish`
3. `chsh -s /usr/bin/fish`
4. 重启服务器
5. `echo $SHELL`

（3-2）omf：

1. `curl -L https://get.oh-my.fish | fish` 安装omf插件;
2. `fish_config` 自动打开浏览器更改fish样式;
3. `omf install wttr` 在终端下可以看天气的小东西;终端下直接输入`wttr`就可以启动;
4. 命令omf之后按TAB键发现有很多选项，可以配置主题等;

（3-3）alias别名设置

```shell
#（1）在命令行中直接设置：

$ alias l 'ls -hl'
$ funcsave l

$ alias ll 'ls -ahl'
$ funcsave ll

$ alias ra 'ranger'
$ funcsave ra

$ alias s 'screenfetch'
$ funcsave s
```

```shell
#（2）修改配置文件：.vim ~/.config/fish/functions/s.fish

# Defined in - @ line 0
function s --description 'alias s screenfetch'
	screenfetch  $argv;
end
```

（3-4）设置全局变量

1. 临时设置：`set -g -x RANGER_LOAD_DEFAULT_RC FALSE`

2. 开机默认设置：

   ```shell
   $ vim ~/.config/fish/config.fish
   
   # 在该文件下增加下面的一行
   set -g -x RANGER_LOAD_DEFAULT_RC FALSE
   ```

3. 



