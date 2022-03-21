# 课程目标

- 熟悉awk的**命令模式**基本语法结构
-  ==熟悉awk的相关内部变量==
- 熟悉awk常用的打印==函数print==
- 能够在awk中匹配正则表达式打印相关的行



# 一、awk介绍

## 1.awk概述

- awk是一种==编程语言==，主要用于在linux/unix下对==文本和数据==进行处理，是linux/unix下的一个工具。数据可以来自标准输入、一个或多个文件，或其它命令的输出。
- awk的处理文本和数据的方式：**==逐行扫描==文件**，默认从第一行到最后一行，寻找匹配的==特定模式==的行，并在这些行上进行你想要的操作。
- awk分别代表其作者姓氏的第一个字母。因为它的作者是三个人，分别是Alfred Aho、Brian Kernighan、Peter Weinberger。
- gawk是awk的GNU版本，它提供了Bell实验室和GNU的一些扩展。


- 下面介绍的awk是以GNU的gawk为例的，在linux系统中已把awk链接到gawk，所以下面全部以awk进行介绍。

## 2. awk能干啥?

1. awk==用来处理文件和数据==的，是类unix下的一个工具，也是一种编程语言
2. 可以用来==统计数据==，比如网站的访问量，访问的IP量等等
3. 支持条件判断，支持for和while循环



# 二、awk使用方式

## 1.==命令行模式使用==

### (一)语法结构

```shell
awk option 'command' file
```

特别说明：

引用shell变量需要用双引号饮用

### (二)常用选项介绍

- ==-F== 定义字段分割符号，默认的分割符是==空格==
- -v 定义变量并赋值

###㈢  **=='==**命名部分说明**=='==**

- 正则表达式，地址定位

```powershell
'/root/{awk语句}'				sed中： '/root/p'
'NR==1,NR==5{awk语句}'			sed中： '1,5p'
'/^root/,/^ftp/{awk语句}'  	sed中：'/^root/,/^ftp/p'
```

- {awk语句1**==;==**awk语句2**==;==**...}

```powershell
'{print $0;print $1}'		sed中：'p'
'NR==5{print $0}'				sed中：'5p'
注：awk命令语句间用分号间隔
```

- BEGIN...END....

```powershell
'BEGIN{awk语句};{处理中};END{awk语句}'
'BEGIN{awk语句};{处理中}'
'{处理中};END{awk语句}'
```



## 2. 脚本模式使用

### ㈠ 脚本编写

```powershell
#!/bin/awk -f 		定义魔法字符
以下是awk引号里的命令清单，不要用引号保护命令，多个命令用分号间隔
BEGIN{FS=":"}
NR==1,NR==3{print $1"\t"$NF}
...
```

### ㈡ 脚本执行

```powershell
方法1：
awk 选项 -f awk的脚本文件  要处理的文本文件
awk -f awk.sh filename

sed -f sed.sh -i filename

方法2：
./awk的脚本文件(或者绝对路径)	要处理的文本文件
./awk.sh filename

./sed.sh filename
```

#三、 awk内部相关变量

| 变量                                                         | 变量说明                               | 备注                               |
| ------------------------------------------------------------ | -------------------------------------- | ---------------------------------- |
| ==$0==                                                       | 当前处理行的所有记录                   |                                    |
| ==\$1,\$2,\$3...\$n==                                        | 文件中每行以==间隔符号==分割的不同字段 | awk -F: '{print \$1,\$3}'          |
| ==NF==                                                       | 当前记录的字段数（列数）               | awk -F: '{print NF}'               |
| ==$NF==           | 最后一列                           | $(NF-1)表示倒数第二列 |                                        |                                    |
| ==FNR/NR==                                                   | 行号                                   |                                    |
| ==FS==                                                       | 定义间隔符                             | 'BEGIN{FS=":"};{print \$1,$3}'     |
| ==OFS==                                                      | 定义输出字段分隔符，==默认空格==       | 'BEGIN{OFS="\t"};print \$1,$3}'    |
| RS                                                           | 输入记录分割符，默认换行               | 'BEGIN{RS="\t"};{print $0}'        |
| ORS                                                          | 输出记录分割符，默认换行               | 'BEGIN{ORS="\n\n"};{print \$1,$3}' |
| FILENAME                                                     | 当前输入的文件名                       |                                    |

## 1、==常用内置变量举例==

```powershell
# awk -F: '{print $1,$(NF-1)}' 1.txt
# awk -F: '{print $1,$(NF-1),$NF,NF}' 1.txt
# awk '/root/{print $0}' 1.txt
# awk '/root/' 1.txt
# awk -F: '/root/{print $1,$NF}' 1.txt 
root /bin/bash
# awk -F: '/root/{print $0}' 1.txt      
root:x:0:0:root:/root:/bin/bash
# awk 'NR==1,NR==5' 1.txt 
# awk 'NR==1,NR==5{print $0}' 1.txt
# awk 'NR==1,NR==5;/^root/{print $0}' 1.txt 
root:x:0:0:root:/root:/bin/bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin

```

## 2、内置变量分割符举例

```shell
FS和OFS：
# awk 'BEGIN{FS=":"};/^root/,/^lp/{print $1,$NF}' 1.txt
# awk 'BEGIN{OFS="\t\t"};/^root/,/^lp/{pring $1,$NF}' 1.txt
root            /bin/bash
bin             /sbin/nologin
daemon          /sbin/nologin
adm             /sbin/nologin
lp              /sbin/nologin
# awk -F: 'BEGIN{OFS="@@@"};/^root/,/^lp/{print $1,$NF}' 1.txt     
root@@@/bin/bash
bin@@@/sbin/nologin
daemon@@@/sbin/nologin
adm@@@/sbin/nologin
lp@@@/sbin/nologin

RS和ORS：
修改源文件前2行增加制表符和内容：
vim 1.txt
root:x:0:0:root:/root:/bin/bash hello   world
bin:x:1:1:bin:/bin:/sbin/nologin        test1   test2

# awk 'BEGIN{RS="\t"};{print $0}' 1.txt
# awk 'BEGIN{ORS="\t"};{print $0}' 1.txt
```

#四、 awk工作原理

`awk -F: '{print $1,$3}' /etc/passwd`

1. awk使用一行作为输入，并将这一行赋给内部变量$0，每一行也可称为一个记录，以换行符(RS)结束

2. 每行被间隔符**==:==**(默认为空格或制表符)分解成字段(或域)，每个字段存储在已编号的变量中，从$1开始

   问：awk如何知道用空格来分隔字段的呢？

   答：因为有一个内部变量==FS==来确定字段分隔符。初始时，FS赋为空格

3. awk使用print函数打印字段，打印出来的字段会以==空格分隔==，因为\$1,\$3之间有一个逗号。逗号比较特殊，它映射为另一个内部变量，称为==输出字段分隔符==OFS，OFS默认为空格

4. awk处理完一行后，将从文件中获取另一行，并将其存储在$0中，覆盖原来的内容，然后将新的字符串分隔成字段并进行处理。该过程将持续到所有行处理完毕

# 五、awk使用进阶

## 1. 格式化输出`print`和`printf`

```powershell
print函数		类似echo "hello world"
# date |awk '{print "Month: "$2 "\nYear: "$NF}'
# awk -F: '{print "username is: " $1 "\t uid is: "$3}' /etc/passwd


printf函数		类似echo -n
# awk -F: '{printf "%-15s %-10s %-15s\n", $1,$2,$3}'  /etc/passwd
# awk -F: '{printf "|%15s| %10s| %15s|\n", $1,$2,$3}' /etc/passwd
# awk -F: '{printf "|%-15s| %-10s| %-15s|\n", $1,$2,$3}' /etc/passwd

awk 'BEGIN{FS=":"};{printf "%-15s %-15s %-15s\n",$1,$6,$NF}' a.txt

%s 字符类型  strings			%-20s
%d 数值类型	
占15字符
- 表示左对齐，默认是右对齐
printf默认不会在行尾自动换行，加\n
```

## 2. awk变量定义

~~~powershell
# awk -v NUM=3 -F: '{ print $NUM }' /etc/passwd
# awk -v NUM=3 -F: '{ print NUM }' /etc/passwd
# awk -v num=1 'BEGIN{print num}' 
1
# awk -v num=1 'BEGIN{print $num}' 
注意：
awk中调用定义的变量不需要加$
~~~

##3. awk中BEGIN...END使用

	①==BEGIN==：表示在==程序开始前==执行
	
	②==END== ：表示所有文件==处理完后==执行
	
	③用法：`'BEGIN{开始处理之前};{处理中};END{处理结束后}'`

#### ㈠ 举例说明1

**打印最后一列和倒数第二列（登录shell和家目录）**

~~~powershell
awk -F: 'BEGIN{ print "Login_shell\t\tLogin_home\n*******************"};{print $NF"\t\t"$(NF-1)};END{print "************************"}' 1.txt

awk 'BEGIN{ FS=":";print "Login_shell\tLogin_home\n*******************"};{print $NF"\t"$(NF-1)};END{print "************************"}' 1.txt

Login_shell		Login_home
************************
/bin/bash		/root
/sbin/nologin		/bin
/sbin/nologin		/sbin
/sbin/nologin		/var/adm
/sbin/nologin		/var/spool/lpd
/bin/bash		/home/redhat
/bin/bash		/home/user01
/sbin/nologin		/var/named
/bin/bash		/home/u01
/bin/bash		/home/YUNWEI
************************************
~~~

#### ㈡ 举例说明2

**打印/etc/passwd里的用户名、家目录及登录shell**

```powershell
u_name      h_dir       shell
***************************

***************************

awk -F: 'BEGIN{OFS="\t\t";print"u_name\t\th_dir\t\tshell\n***************************"};{printf "%-20s %-20s %-20s\n",$1,$(NF-1),$NF};END{print "****************************"}'


# awk -F: 'BEGIN{print "u_name\t\th_dir\t\tshell" RS "*****************"}  {printf "%-15s %-20s %-20s\n",$1,$(NF-1),$NF}END{print "***************************"}'  /etc/passwd

格式化输出：
echo		print
echo -n	printf

{printf "%-15s %-20s %-20s\n",$1,$(NF-1),$NF}
```

## 4. awk和正则的综合运用

| 运算符 | 说明     |
| ------ | -------- |
| ==     | 等于     |
| !=     | 不等于   |
| >      | 大于     |
| <      | 小于     |
| >=     | 大于等于 |
| <=     | 小于等于 |
| ~      | 匹配     |
| !~     | 不匹配   |
| !      | 逻辑非   |
| &&     | 逻辑与   |
| \|\|   | 逻辑或   |

### ㈠ 举例说明

~~~powershell
从第一行开始匹配到以lp开头行
awk -F: 'NR==1,/^lp/{print $0 }' passwd  
从第一行到第5行          
awk -F: 'NR==1,NR==5{print $0 }' passwd
从以lp开头的行匹配到第10行       
awk -F: '/^lp/,NR==10{print $0 }' passwd 
从以root开头的行匹配到以lp开头的行       
awk -F: '/^root/,/^lp/{print $0}' passwd
打印以root开头或者以lp开头的行            
awk -F: '/^root/ || /^lp/{print $0}' passwd
awk -F: '/^root/;/^lp/{print $0}' passwd
显示5-10行   
awk -F':' 'NR>=5 && NR<=10 {print $0}' /etc/passwd     
awk -F: 'NR<10 && NR>5 {print $0}' passwd 

打印30-39行以bash结尾的内容：
[root@MissHou shell06]# awk 'NR>=30 && NR<=39 && $0 ~ /bash$/{print $0}' passwd 
stu1:x:500:500::/home/stu1:/bin/bash
yunwei:x:501:501::/home/yunwei:/bin/bash
user01:x:502:502::/home/user01:/bin/bash
user02:x:503:503::/home/user02:/bin/bash
user03:x:504:504::/home/user03:/bin/bash

[root@MissHou shell06]# awk 'NR>=3 && NR<=8 && /bash$/' 1.txt  
stu7:x:1007:1007::/rhome/stu7:/bin/bash
stu8:x:1008:1008::/rhome/stu8:/bin/bash
stu9:x:1009:1009::/rhome/stu9:/bin/bash

打印文件中1-5并且以root开头的行
[root@MissHou shell06]# awk 'NR>=1 && NR<=5 && $0 ~ /^root/{print $0}' 1.txt
root:x:0:0:root:/root:/bin/bash
[root@MissHou shell06]# awk 'NR>=1 && NR<=5 && $0 !~ /^root/{print $0}' 1.txt
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin

理解;号和||的含义：
[root@MissHou shell06]# awk 'NR>=3 && NR<=8 || /bash$/' 1.txt
[root@MissHou shell06]# awk 'NR>=3 && NR<=8;/bash$/' 1.txt


打印IP地址
# ifconfig eth0|awk 'NR>1 {print $2}'|awk -F':' 'NR<2 {print $2}'    
# ifconfig eth0|grep Bcast|awk -F':' '{print $2}'|awk '{print $1}'
# ifconfig eth0|grep Bcast|awk '{print $2}'|awk -F: '{print $2}'


# ifconfig eth0|awk NR==2|awk -F '[ :]+' '{print $4RS$6RS$8}'
# ifconfig eth0|awk -F"[ :]+" '/inet addr:/{print $4}'
~~~

## 4. 课堂练习

1. 显示可以登录操作系统的用户所有信息     从第7列匹配以bash结尾，输出整行（当前行所有的列）

```powershell
[root@MissHou ~] awk '/bash$/{print $0}'    /etc/passwd
[root@MissHou ~] awk '/bash$/{print $0}' /etc/passwd
[root@MissHou ~] awk '/bash$/' /etc/passwd
[root@MissHou ~] awk -F: '$7 ~ /bash/' /etc/passwd
[root@MissHou ~] awk -F: '$NF ~ /bash/' /etc/passwd
[root@MissHou ~] awk -F: '$0 ~ /bash/' /etc/passwd
[root@MissHou ~] awk -F: '$0 ~ /\/bin\/bash/' /etc/passwd
```

2. 显示可以登录系统的用户名 

```powershell
# awk -F: '$0 ~ /\/bin\/bash/{print $1}' /etc/passwd
```

3. 打印出系统中普通用户的UID和用户名

```powershell
500	stu1
501	yunwei
502	user01
503	user02
504	user03


# awk -F: 'BEGIN{print "UID\tUSERNAME"} {if($3>=500 && $3 !=65534 ) {print $3"\t"$1} }' /etc/passwdUID	USERNAME


# awk -F: '{if($3 >= 500 && $3 != 65534) print $1,$3}' a.txt 
redhat 508
user01 509
u01 510
YUNWEI 511
```

##5. awk的脚本编程

### ㈠ 流程控制语句

#### ① if结构

```powershell
if语句：

if [ xxx ];then
xxx
fi

格式：
awk 选项 '正则，地址定位{awk语句}'  文件名

{ if(表达式)｛语句1;语句2;...｝}

awk -F: '{if($3>=500 && $3<=60000) {print $1,$3} }' passwd

# awk -F: '{if($3==0) {print $1"是管理员"} }' passwd 
root是管理员

# awk 'BEGIN{if('$(id -u)'==0) {print "admin"} }'
admin
```

#### ② if...else结构

```powershell
if...else语句:
if [ xxx ];then
	xxxxx
	
else
	xxx
fi

格式：
{if(表达式)｛语句;语句;...｝else｛语句;语句;...}}

awk -F: '{ if($3>=500 && $3 != 65534) {print $1"是普通用户"} else {print $1,"不是普通用户"}}' passwd 

awk 'BEGIN{if( '$(id -u)'>=500 && '$(id -u)' !=65534 ) {print "是普通用户"} else {print "不是普通用户"}}'

```































































