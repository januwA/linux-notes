../home/   相对路劲
/home/  绝对路径

系统目录：
- / 根目录
- /bin 命令保存目录
- /boot 启动目录
- /dev  设备文件保存目录
- /etc  配置文件保存目录
- /home  普通用户家目录
- /lib  系统库保存目录
- /mnt  系统挂载目录
- /media  挂载目录
- /tmp  临时数据目录
- /usr 系统软件资源目录
- /var  系统文档

---

## login登陆，logout退出
当进入页面后看到的 ajanuw@ubuntu:~$
当前登陆用户	ajanuw
当前登陆主机名	ubuntu
当前所在目录	~
$ 是普通用户提示符 超级用户是 #

* pwd 查看当前所在路径
* ifconfig  查看本机ip
* ls 或者 ll [-h] 查看当前目录下的文件 -h显示文件大小
* linux下 . 点开始的文件是隐藏文件

d rwx r-x r-x （文件权限默认10位）
* d目录 （-文件，d目录，l软链接文件）
* rwx(u所有者) r-x(g所属组) r-x(o其他人)
* r读4  w写2  x执行1

---

创建文件：
*  mkdir -p www/app/src  -p 选项递归创建，二级目录
* touch a.txt  创建一个空的文件
* echo hello > c.txt  流重定向创建文件
* echo 2> b.txt  错误流重定向创建文件

删除文件：
* rm -rf  目录名或文件名   -rf 强制删除文件或目录
* rm -f *   // 删除当前文件夹下所有的文件
* rm -rf *.txt  // 删除当前目录下 后缀名为 .txt的文件
* rm -rf *.js *.py  // 删除当前目录下 后缀名为 .js和.py的文件

查找文件：
* find /home/ajanuw/ -iname *.txt -exec rm -rf {} \;  // 查找了ajanuw/下的.txt文件然后删除掉

移动文件：
* mv a/1.txt b/ && rm -rf a   // 把a下面的1.txt剪切到b下面去，这条命令执行成功才继续删除a目录
- mx xxx/* ./   // 把xxx目录下的文件全部 剪切当前目录
- mv x.txt a.txt  也可以用于更改文件名
- mv 1.txt 2.txt xxx/   // 把两个文件 同时剪切到 xxx/ 目录下面

目录切换：
* cd 目录名  用于在目录间切换
* cd 进入更目录 
* cd -  进入上一次目录
* cd .. 进入上一级目录

拷贝文件：
* cp -a 目录或文件 目标目录  拷贝命令，拷贝时可跟换文件名

创建硬链接:
- ln [-s] 源文件 目标文件  -s 选项创建软链接 
- 源文件要写绝对路径 ln -s /home/ajanuw/x.txt ./  在当前目录下创建了一个软链接
- 注： 硬链接过来的文件，如果源文件被删除了，硬链接还在，软连接则会无法使用

* cat x.txt   ;可直接打印文件内容

---
其他：
- data  ;显示时间
- cal ;显示这个月的日历
- cal 12 2017  ;显示2017年10月的日历
- cal 2017 ;显示一年的日历
- clear  ;清空屏幕
- cal 2017 | less  ;| less通过管道分屏 q退出
- bc  ;计算器 quit退出计算器
- echo $PATH  ;是看环境变量

---

## 文件搜索命令
locate 文件名参数 ;就1参数
- 特点：只能搜文件名，速度快，不能搜索新建文件，需要执行 >updatedb 更新数据库
- locate xx.txt 还是推荐使用find
- /etc/updatedb.conf  筛选的配置文件

搜索命令的命令 >whereis 和 >which , 不能用来搜文件
- whereis -b rm


find [搜索范围] [搜索条件]  ;搜索文件和目录的命令，功能多，速度慢,条件可多条

- find /home/ -iname xx.txt  在home下搜索名为 xx.txt的文件，文件名完全匹配 -iname为忽略大小写并按文件名搜索

linux 通配符 配合搜索命令再好不过:
- \*  匹配任意内容
- ?  匹配任意一个字符
- []  匹配任意一个括号中间的字符
- find /home/ -iname "*.txt"		; 找后缀为.txt的全部文件 [i]是不区分大小写
- find /home/ajanuw/ -iname *.txt -o -iname *.js  // 搜索 txt文件和js文件

按时间搜索:
- find /home/ -mtime +10  //查找10天前修改的文件
  
	注： -10 十天之内修改的， 10 十天当天修改的， +10 十天前修改的
    :  -atime 文件访问时间， -ctime 改变文件属性，-mtime 修改文件时间

- find /home/ -mtime 0  // 搜索当天修改的文件

按文件大小搜索:
- find . -size 30k   当前目录下等于30k的文件[-30, 30 , +30]
- find . -size +30k -a -size -50k   当前目录大于30k并且小于50k文件
- find /home/ -size +5k -exec ls -lh {} \;   查询出信息同时管道执行 ls -lh
- find /home/ -size -30k -iname *.js  搜索小于30k的js文件
- find /home/ -size +10k -a -size -50k  -a两个条件都满足 -o满足一个即可

文件里面的搜索指定的字符串:
- grep [选项] 字符串 文件名  [-v取反不包涵后面的内容, -i忽略大小写] grep 命令在搜索到相应的内容后 会打印出来...
 - find /home/ajanuw/ \( -iname "*.js" -o -iname "*.txt" \) -exec grep -i "hello" {} \; -exec ls {} \;  这条命令 搜索了 .txt和.js文件， 查找这些文件中带有"hello"字段的文件，在ls打印路径出来,  需要加上括号， 括号需要转义， 不加括号默认取最后一个结果





文件的压缩与解压:
>zip [-r] xx.zip xx.txt   ;把xx.txt压缩成为 xx.zip,[-r]为压缩目录结构必加参数
>unzip xx.txt.zip  ;把 zip包解压缩

关机和重启:
>shutdown [选项] 时间 ; [-c 取消上一个关机命令,-h 关机,-r 重启]

>shutdown -h now   ;立刻关机
>shutdown -r 08:30 &   ;在早上8:30的时候定时重启电脑 [&] 把任务放在后台执行
>shutdown -c   ;取消掉关机服务

mount 光盘挂载:
>mount [-t] [-o 特殊选项] 设备文件名 挂载点
>umount 挂载点  ;卸载挂载


用户登陆查看命令:
```
>w
 09:02:14 up 50 min,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
ajanuw   pts/0    192.168.147.1    08:12    0.00s  0.13s  0.04s sshd: ajanuw
```

第一行:
- 09:02:14 ;当前系统时间
- up 50  ;当前系统运行时间
- 1 user  ;当前系统1个用户在登陆

第二行:
- USER   ;登录用户
- TTY    ;用什么登陆， pts/0 为第一个远程终端登陆
- FROM	;那个ip地址登陆
- LOGIN@   ;登录时间
- IDLE	;用户闲置时间
- WHAT    ;当前正在使用的命令

>who   ;也是一样的

>last   ;查看当前和以前登陆的信息

>lastlog   ;查看所用用户最后一次登陆的时间Never logged in从未登录


初识shell脚本语言:
- echo $SHELL   ;查看环境变量 看当前系统使用的那种shell语言

- echo [选项] 参数  ;输出信息[-e] 有个-e选项 支持控制字符 [\a输出警告音, \b退格删除键, \n换行, \r回车, \t TAB键, \v垂直制表符, \0 八进制, \x 16进制]
- echo "123abc"   ;将会打印123abc
- echo -e "\e[1;32m 123abc \e[0m"   ;将会 打印绿色字体
- echo -e "123\nabc"   ;123换行abc

创建一个 hello.sh 脚本文件:
- #!/bin/bash   头文件必写。#为注释
- bash hello.sh  ;执行脚本文件
- chmod 755 hello.sh ;或给予文件755权限 数字权限[读r-4,写w-2, 执行x-1]
	
	./hello.sh  ;然后直接执行文件，绝对路径和相对路径执行都可以

命令别名:
- alias  ;查看当前系统，类似环境变量，目的方便
- alias node="nodejs"  ;设置别名 以后打node就相当于打了nodejs这个命令
- unalias node  ;删除别名

这样设置的别名，在计算机重启后失效.想要永久生效，写入环境变量配置文件

	 vi ~/.bashrc  ;打开家目录下的.bashrc文件 #some more ls aliases下面添加更多别名 ubuntu16
	 source .bashrc  ;更新下

xshell快捷键:
- ctrl+c 强制退出
- ctrl+l 清屏 clear一样
- ctrl+u  删除命令
- ctrl+a  光标移动到行首
- ctrl+e  光标移动到行尾
- ctrl+z  把命令放入后台
- ctrl+s  锁屏
- ctrl+q  解锁屏

查看历史命令:
- history  ;会打印出这次登陆后，操作的所有命令
- vi ~/.bash_history   ;会看到上次正确退出系统，保留的命令
- history -w  ;主动向~/.bash_history写入历史命令
- history -c  ;删除全部历史纪录，在用>history -w 更新~/.bash_history
- 上下箭头可以调用以前的命令
- !n  ;调用第n条数据
- !!  ;执行上一条数据
- !string  ;执行最后一条以该字符串开头的命令

输出重定向[>>], 输入从定向[wc 文件, < , <<]
- 0 标准输入
- 1 标准输出
- 2 错误输出
- \> 替换
- \>\> 追加

- date >> a.txt 2>&1  ;追加的方式，把错误和正确的都写在文件中
- date &>> a.txt  ;更上面一样
- date &>/etc/null   ;相当与扔垃圾站
- date >> 1.txt  2>> 2.txt  ;正确和错误的信息分开处理,1.txt保存正确的信息

管道:
 - 多命令执行顺序 [; && ||]
 - ;  ->> 命令1;命令2   ；按个执行命令之间没有逻辑关系
 - &&  命令1&&命令2  ；命令1正确执行命令2会执行
 - ||  命令1 || 命令2  ；命令1正确执行命令2不会执行
 - |  命令1 | 命令2  ；命令1的正确输出作为命令2的操作对象


通配符:
- 【?,*,[],-,^】
- ? 匹配任意一个字符串 ll .?
- \* 匹配任意个 ll .bash*
- [abc] a或b或c
- [^abc] 非a或b或c
- [0-9]  匹配一个范围

bash 的其他符号:
	
	name="ajanuw" ; echo $name 打印ajanuw, name是变量, $name引用变量
	'标准字符串' 和 "$var可以调用变量" 是有区别的
	#注释, \转义, a=$(date) date输出赋值更a



Ubuntu系统配置文件默认在 /etc下

ps -e | grep ssh 查找是否启动 ssh 启动显示 sshd

 apt-get install openssh-server  安装ssh

/etc.init.d/ssh start 启动ssh服务


使用命令：lsb_release -a 查看ubuntu版本信息
tasksel install lamp-server

重启服务   sudo service mysql restart
apache 默认目录 var/www
sudo apt-get install php-gd curl libcurl3 libcurl3-dev php-curl  安装扩展