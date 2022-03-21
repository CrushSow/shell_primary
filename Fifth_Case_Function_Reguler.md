# 课程目标

- 掌握case语句的基本语法结构
- 掌握函数的定义
- 掌握常用的正则表达式元字符含义

# 一、case语句

**关键字：确认过眼神，你是对的人**:couple_with_heart:

1. case语句为多重匹配语句
2. 如果匹配成功，执行相匹配的命令



## 1.语法结构

```shell
说明：pattern表示需要匹配的模式

case var in 				定义变量；var 代表是变量名
pattern 1)         模式1；用｜ 分割多个模式，相当于 or
	command1          需要执行的命令
	;;									两个分号表示命令的结束
pattern 2)
	command2
	;;
pattern 3)
	command3
	;;
*)									default，不满足以上模式，默认执行*）下面的语句
	command4
	;;
esac						   esac 表示case语句结束
```

## 2. 应用案例

### ㈠ 脚本传不同值做不同事

**具体需求：**当给程序传入start、stop、restart三个不同参数时分别执行相应命令

```shell
case $1 in
	start|S)
		service apache start &>/dev/null && echo "apache start success"
		;;
	stop|T)
		service apache stop &>/dev/null && echo "apache stop success"
	restart|R)
		service apache restart &>/dev/null && echo "apache restart success"
		;;
	*)
		echo "please you want do something:"
		;;
esac
```

### ㈡ 根据用户需求选择做事

**具体需求：**

脚本提示让用户输入需要管理的服务名，然后提示用户需要对服务做什么操作，如启动，关闭等操作

```shell
#!/bin/bash
read -p "please input you want manage service(such as:vsftpd):" service
case $service in
	vsftpd|ftp)
		read -p "please input (restart|stop):" action
		case $action in
			stop|S)
				service vsftpd stop &>/dev/null && echo "service stop success"
				;;
			start)
				service vsftpd start &>/dev/null && echo "service start success"
				;;
			esac
			;;
	httpd|apache)
		echo "apache hello world"
		;;
	*)
		echo "please input you want manage service(such as:vsftpd):"
    ;;
 esac
```

###㈢ 菜单提示让用户选择需要做的事

**具体需求：**

模拟一个多任务维护界面;当执行程序时先显示总菜单，然后进行选择后做相应维护监控操作

```powershell
**********请选择*********
h	显示命令帮助
f	显示磁盘分区
d	显示磁盘挂载
m	查看内存使用
u	查看系统负载
q	退出程序
*************************
```

**思路：**

1. 菜单打印出来
2. 交互式让用户输入操作编号，然后做出相应处理

**落地实现：**

~~~powershell
#!/bin/bash
#打印菜单
cat <<-EOF
	h	显示命令帮助
	f	显示磁盘分区
	d	显示磁盘挂载
	m	查看内存使用
	u	查看系统负载
	q	退出程序
	EOF

#让用户输入需要的操作
while true
do
read -p "请输入需要操作的选项[f|d]:" var1
case $var1 in
	h)
	cat <<-EOF
        h       显示命令帮助
        f       显示磁盘分区
        d       显示磁盘挂载
        m       查看内存使用
        u       查看系统负载
        q       退出程序
	EOF
	;;
	f)
	fdisk -l
	;;
	d)
	df -h
	;;
	m)
	free -m
	;;
	u)
	uptime
	;;
	q)
	exit
	;;
esac
done



#!/bin/bash
#打印菜单
menu(){
cat <<-END
	h	显示命令帮助
	f	显示磁盘分区
	d	显示磁盘挂载
	m	查看内存使用
	u	查看系统负载
	q	退出程序
	END
}
menu
while true
do
read -p "请输入你的操作[h for help]:" var1
case $var1 in
	h)
	menu
	;;
	f)
	read -p "请输入你要查看的设备名字[/dev/sdb]:" var2
	case $var2 in
		/dev/sda)
		fdisk -l /dev/sda
		;;
		/dev/sdb)
		fdisk -l /dev/sdb
		;;
	esac
	;;
	d)
	lsblk
	;;
	m)
	free -m
	;;
	u)
	uptime
	;;
	q)
	exit
	;;
esac
done

~~~

**课堂练习：**

1. 输入一个等级（A-E），查看每个等级的成绩；如：输入A，则显示“90分~100分”，依次类推
2. 判断用户输入的字符串，如果是"hello",则显示"world"；如果是"world",则显示"hello",否则提示"请输入hello或者world，谢谢！"



# 二、==函数==

## 1. 什么是函数？

- shell中允许将**一组命令集合**或**语句**形成一段**可用代码**，这些代码块称为shell函数
- 给这段代码起个名字称为函数名，后续可以直接调用该段代码的功能

## 2. 如何定义函数？

**方法1：**

```powershell
函数名()
{
  函数体（一堆命令的集合，来实现某个功能）   
}
```

**方法2：**

```powershell
function 函数名()
{
   函数体（一堆命令的集合，来实现某个功能）
   echo hello
   echo world
}

```

**函数中==return==说明:**

1. return可以==结束一个函数==。类似于循环控制语句break(结束当前循环，执行循环体后面的代码)。
2. return默认返回函数中最后一个命令状态值，也可以给定参数值，范围是0-256之间。
3. 如果没有return命令，函数将返回最后一个指令的退出状态值。



##3. 函数如何调用？

### ㈠ 当前命令行调用

~~~powershell
[root@MissHou shell04]# cat fun1.sh 
#!/bin/bash
hello(){
echo "hello lilei $1"
hostname
}
menu(){
cat <<-EOF
1. mysql
2. web
3. app
4. exit
EOF
}

[root@MissHou shell04]# source fun1.sh 
[root@MissHou shell04]# . fun1.sh 

[root@MissHou shell04]# hello 888
hello lilei 888
MissHou.itcast.cc
[root@MissHou shell04]# menu
1. mysql
2. web
3. app
4. exit

~~~



### ㈡ 定义到用户的环境变量中

~~~powershell
[root@MissHou shell05]# vim ~/.bashrc 
文件中增加如下内容：
hello(){
echo "hello lilei $1"
hostname
}
menu(){
cat <<-EOF
1. mysql
2. web
3. app
4. exit
EOF
}

注意：
当用户打开bash的时候会读取该文件
~~~

### ㈢ 脚本中调用

~~~powershell
#!/bin/bash
#打印菜单
source ./fun1.sh
menu(){
cat <<-END
	h	显示命令帮助
	f	显示磁盘分区
	d	显示磁盘挂载
	m	查看内存使用
	u	查看系统负载
	q	退出程序
	END
}
menu		//调用函数

~~~

##4. 应用案例

**具体需求：**

1. 写一个脚本==收集用户输入==的基本信息(姓名，性别，年龄)，如不输入==一直提示输入==
2. 最后根据用户的信息输出相对应的内容

**思路：**

1. ==交互式==定义多个变量来保存用户信息  姓名、性别、年龄
2. 如果不输一直提示输入
   - ==循环==直到输入字符串不为空  while  判断输入字符串是否为空
   - 每个信息都必须不能为空，该功能可以定义为一个函数，方便下面脚本调用

3. 根据用户输入信息做出匹配判断

**代码实现：**

```shell
#!/bin/bash
input_fun(){
	input_var=""
	output_var=$1
	while [ -z $input_var ]
	do
		read -p "$output_var" input_var
	done
	echo $input_var
		
}
input_fun please input your name:

或者是
#!/bin/bash
fun(){
	read -p "$1" var
	if [ -z $var ];then
		fun $1
	else
		echo $var
	fi
}

#调用函数并且获取用户的姓名、性别、年龄分别赋值给name、sex、age变量
name=$(input_fun 请输入你的姓名:)
sex=$(input_fun 请输入你的性别:)
age=$(input_fun 请输入你的年龄:)

#根据用户输入的性别进行匹配判断
case $sex in
			man)
			if [ $age -gt 18 -a $age -le 35 ];then
				echo "中年大叔你油腻了吗？加油"
			elif [ $age -gt 35 ];then
				echo "保温杯里泡枸杞"
			else
				echo "年轻有为。。。"
			fi
			;;
			woman)
			xxx
			;;
			*)
			xxx
			;;
esac
```

**扩展延伸：**

```powershell
描述以下代码含义：	
:()
{
   :|:&
}
:
```

#三、综合案例

## 1. 任务背景

现有的跳板机虽然实现了统一入口来访问生产服务器，yunwei用户权限太大可以操作跳板机上的所有目录文件，存在数据被误删的安全隐患，所以希望你做一些安全策略来保证跳板机的正常使用。

## 2. 具体要求

1. 只允许yunwei用户通过跳板机远程连接后台的应用服务器做一些维护操作
2. 公司运维人员远程通过yunwei用户连接跳板机时，跳出以下菜单供选择：

3. 当用户选择相应主机后，直接**免密码登录**成功
4. 如果用户不输入一直提示用户输入，直到用户选择退出

## 3. 综合分析

1. 将脚本放到yunwei用户家目录里的.bashrc文件里（/shell05/jumper-server.sh）
2. 将菜单定义为一个函数[打印菜单]，方便后面调用
3. 用case语句来实现用户的选择【交互式定义变量】
4. 当用户选择了某一台服务器后，进一步询问用户需要做的事情  case...esac  交互式定义变量
5. 使用循环来实现用户不选择一直让其选择
6. 限制用户退出后直接关闭终端  exit 

## 4. 落地实现

























