# 课程目标

- ==掌握for循环语句的基本语法结构==
- ==掌握while和until循环语句的基本语法结构==
- 能会使用RANDOM产生随机数
- 理解嵌套循环

# 一、随机数

**关键字：一切都是未知数，永远不知道明天会抽什么风**:wind_chime::sweat_smile:

## 1.如何生成随机数？

**系统变量**：**==RANDOM==**，默认会产生0-32767的随机整数

**前言：**要调用变量，不管你是什么变量都要给钱，而且是美元:heavy_dollar_sign:

```shell
打印一个随机数
echo $RANDOM
查看系统上一次生成的随机数
# set|grep RANDOM
RANDOM=28325

产生0~1之间的随机数
echo $[$RANDOM%2]

产生0~2之间的随机数
echo $[$RANDOM%3]

产生0~3之间的随机数
echo $[$RANDOM%4]

产生0~9内的随机数
echo $[$RANDOM%10]

产生0~100内的随机数
echo $[$RANDOM%101]


产生50-100之内的随机数
echo $[$RANDOM%51+50]

产生三位数的随机数
echo $[$RANDOM%900+100]
```

## 2. 实战案例

### ㈠ 随机产生以139开头的电话号码

**具体需求1：**

写一个脚本，产生一个phonenum.txt文件，随机产生以139开头的手机号1000个，每个一行。

#### ① 思路

1. 产生1000个电话号码，脚本需要循环1000次 `FOR WHILE UNTIL`
2. 139+8位,后8位随机产生，可以让每一位数字都随机产生  `echo $[$RANDOM%10]`
3. 将随机产生的数字分别保存到变量里，然后加上139保存到文件里

#### ② 落地实现

~~~powershell

#!/bin/env bash
#产生1000个以139开头的电话号码并保存文件phonenum.txt
file=/shell03/phonenum.txt
for ((i=1;i<=1000;i++))
do
	n1=$[$RANDOM%10]
	n2=$[$RANDOM%10]
	n3=$[$RANDOM%10]
	n4=$[$RANDOM%10]
	n5=$[$RANDOM%10]
	n6=$[$RANDOM%10]
	n7=$[$RANDOM%10]
	n8=$[$RANDOM%10]
	echo "139$n1$n2$n3$n4$n5$n6$n7$n8" >> $file
done


#!/bin/bash
# random phonenum
# 循环1000次产生电话号码并保存到文件
for i in {1..1000}
do
	n1=$[RANDOM%10]
	n2=$[RANDOM%10]
	n3=$[RANDOM%10]
	n4=$[RANDOM%10]
	n5=$[RANDOM%10]
	n6=$[RANDOM%10]
	n7=$[RANDOM%10]
	n8=$[RANDOM%10]
	echo "139$n1$n2$n3$n4$n5$n6$n7$n8" >> phonenum.txt
done

#!/bin/bash
i=1
while [ $i -le 1000 ]
do
	n1=$[$RANDOM%10]
	n2=$[$RANDOM%10]
	n3=$[$RANDOM%10]
	n4=$[$RANDOM%10]
	n5=$[$RANDOM%10]
	n6=$[$RANDOM%10]
	n7=$[$RANDOM%10]
	n8=$[$RANDOM%10]
	echo "139$n1$n2$n3$n4$n5$n6$n7$n8" >> phonenum.txt
	let i++
done

continue:继续，跳过本次循环，执行下一次循环
break:打断，执行循环体外的代码do..done外
exit:退出程序


#!/bin/bash
for i in {1..1000}
do
	n1=$[$RANDOM%10]
	n2=$[$RANDOM%10]
	n3=$[$RANDOM%10]
	n4=$[$RANDOM%10]
	n5=$[$RANDOM%10]
	n6=$[$RANDOM%10]
	n7=$[$RANDOM%10]
	n8=$[$RANDOM%10]
	echo "139$n1$n2$n3$n4$n5$n6$n7$n8" >> phonenum.txt
done

#!/bin/bash
#create phone num file
for ((i=1;i<=1000;i++))
do
	n1=$[$RANDOM%10]
	n2=$[$RANDOM%10]
	n3=$[$RANDOM%10]
	n4=$[$RANDOM%10]
	n5=$[$RANDOM%10]
	n6=$[$RANDOM%10]
	n7=$[$RANDOM%10]
	n8=$[$RANDOM%10]
	echo "139$n1$n2$n3$n4$n5$n6$n7$n8" |tee -a phonenum.txt
done

#!/bin/bash
count=0
while true
do
	n1=$[$RANDOM%10]
	n2=$[$RANDOM%10]
	n3=$[$RANDOM%10]
	n4=$[$RANDOM%10]
	n5=$[$RANDOM%10]
	n6=$[$RANDOM%10]
	n7=$[$RANDOM%10]
	n8=$[$RANDOM%10]
	echo "139$n1$n2$n3$n4$n5$n6$n7$n8" |tee -a phonenum.txt && let count++
	if [ $count -eq 1000 ];then
		break
	fi
done
~~~

### ㈡ 随机抽出5位幸运观众

**具体需求：**

1. 在上面的1000个手机号里抽奖==5个==幸运观众，显示出这5个幸运观众。
2. 但只显示头3个数和尾号的4个数，中间的都用*代替

#### ① 思路

1. 确定幸运观众所在的行	`0-1000  随机找出一个数字   $[$RANDOM%1000+1]`
2. 将电话号码提取出来      `head -随机产生行号 phonenum.txt |tail -1`
3. ==显示==前3个和后4个数到屏幕   `echo 139****`

#### ② 落地实现

```shell
#!/bin/bash
phone=/shell03/phonenum.txt
for i in {1..5}
do
	line=`wc -l $phone | cut -d' ' -f1`
	luck_line=$[RANDOM%$line+1]
	#取出幸运观众所在行的电话号码
	luck_num=`head -$luck_line $phone | tail -l`
	echo "139*****${luck_num:7:4}"
	echo $luck_num >> luck.txt
	#删除已经抽取的幸运观众的号码
	#sed -i "/luck_num/d" $phone
done


#!/bin/bash
file=/shell04/phonenum.txt
for i in{1..5}
	do
		file_num=`wc -l $file | cut -d' ' -f1`
		line=`echo $[RANDOM%file_num+1]`
		luck=`head -n $line $file | tail -l`
		echo "139****${luck:7:4}" && echo $luck >> /shell04/luck_num.txt
	done
	
#!/bin/bash
for ((i=1;i<=5;i++))
do
file=phonenum.txt
line=`cat phonenum.txt |wc -l`	1000
luckline=$[$RANDOM%$line+1]
phone=`cat $file|head -$luckline|tail -1`
echo "幸运观众为:139****${phone:7:4}"
done


或者
#!/bin/bash
# choujiang
phone=phonenum.txt
for ((i=1;i<=5;i++))
do
	num=`wc -l phonenum.txt |cut -d' ' -f1`
	line=`echo $[$RANDOM%$num+1]`
	luck=`head -$line $phone |tail -1`
	sed -i "/$luck/d" $phone
	echo "幸运观众是:139****${luck:7:4}"
done

```

### ㈢ 批量创建用户(密码随机产生)

**需求：**批量创建5个用户，每个用户的密码为一个随机数

#### ① 思路

1. 循环5次创建用户
2. 产生一个密码文件来保存用户的随机密码
3. 从密码文件中取出随机密码赋值给用户

#### ② 落地实现

```shell
#!/bin/bash
#crate user and set passwd
#产生一个保存用户名和密码的文件
echo user0{1..5}:itcast$[$RANDOM%9000+1000]#@~|tr ' ' '\n'>> user_pass.file

#循环创建5个用户
for ((i=1;i<=5;i++))
do
	user=`head -$i user_pass.file|tail -1|cut -d: -f1`
	pass=`head -$i user_pass.file|tail -1|cut -d: -f2`
	useradd $user
	echo $pass|passwd --stdin $user
done

或者
for i in `cat user_pass.file`
do
	user=`echo $i|cut -d: -f1`
	pass=`echo $i|cut -d: -f2`
	useradd $user
	echo $pass|passwd --stdin $user
done

#!/bin/bash
#crate user and set passwd
#产生一个保存用户名和密码的文件
echo user0{1..3}:itcast$[$RANDOM%9000+1000]#@~|tr ' ' '\n'|tr ':' ' ' >> user_pass.file
#循环创建5个用户
while read user pass
do
useradd $user
echo $pass|passwd --stdin $user
done < user_pass.file


pwgen工具产生随机密码：
[root@server shell04]# pwgen -cn1 12
Meep5ob1aesa
[root@server shell04]# echo user0{1..3}:$(pwgen -cn1 12)
user01:Bahqu9haipho user02:Feiphoh7moo4 user03:eilahj5eth2R
[root@server shell04]# echo user0{1..3}:$(pwgen -cn1 12)|tr ' ' '\n'
user01:eiwaShuZo5hi
user02:eiDeih7aim9k
user03:aeBahwien8co
```



# 二、嵌套循环

**关键字：大圈套小圈**

:clock3:**时钟**：分针与秒针，秒针转⼀圈（60格），分针转1格。循环嵌套就是外层循环⼀次，内层循环⼀轮。

1. 一个==循环体==内又包含另一个**完整**的循环结构，称为循环的嵌套。
2. 每次外部循环都会==触发==内部循环，直至内部循环完成，才接着执行下一次的外部循环。
3. for循环、while循环和until循环可以**相互**嵌套。

##1. 应用案例

### ㈠ 打印指定图案

```powershell
1
12
123
1234
12345

5
54
543
5432
54321
```

### ㈡ 落地实现1

~~~powershell
X轴：
for ((i=1;i<=5;i++));do echo -n $i;done
Y轴：
负责打印换行

#!/bin/bash
for ((y=1;y<=5;y++))
do
	for ((x=1;x<=$y;x++))
	do
		echo -n $x
	done
echo
done

#!/bin/bash
for ((y=1;y<=5;y++))
do
	x=1
	while [ $x -le $y ]
		do
		echo -n $x
		let x++
		done
echo
done
~~~

### ㈢ 落地实现2

~~~powershell
Y轴：打印换行
X轴：打印数字 5-1

#!/bin/bash
y=5
while (( $y >= 1 ))
do
	for ((x=5;x>=$y;x--))
	do
		echo -n $x
	done
echo
let y--
done


#!/bin/bash
for (( y=5;y>=1;y--))
do
	for (( x=5;x>=$y;x--))
	do
	echo -n $x
	done
echo
done

#!/bin/bash
y=5
while [ $y -ge 1 ]
do
	for ((x=5;x>=$y;x--))
	do
	echo -n $x
	done
echo
let y--
done


#!/bin/bash
y=1
until (( $y >5 ))
do
	x=1
	while (( $x <= $y ))
	do
	echo -n $[6-$x]
	let x++
	done	
echo
let y++
done


课后打印：
54321
5432
543
54
5

~~~

##2. 课堂练习

**打印九九乘法表（三种方法）**

~~~powershell
1*1=1

1*2=2   2*2=4

1*3=3   2*3=6   3*3=9

1*4=4   2*4=8   3*4=12  4*4=16

1*5=5   2*5=10  3*5=15  4*5=20  5*5=25

1*6=6   2*6=12  3*6=18  4*6=24  5*6=30  6*6=36

1*7=7   2*7=14  3*7=21  4*7=28  5*7=35  6*7=42  7*7=49

1*8=8   2*8=16  3*8=24  4*8=32  5*8=40  6*8=48  7*8=56  8*8=64

1*9=9   2*9=18  3*9=27  4*9=36  5*9=45  6*9=54  7*9=63  8*9=72  9*9=81


Y轴：循环9次，打印9行空行
X轴：循环次数和Y轴相关；打印的是X和Y轴乘积 $[] $(())

#!/bin/bash
for ((y=1;y<=9;y++))
do
	for ((x=1;x<=$y;x++))
	do
		echo -ne "$x*$y=$[$x*$y]\t"
	done
echo
echo
done


#!/bin/bash
y=1
while [ $y -le 9 ]
do
        x=1
        while [ $x -le $y ]
        do
                echo -ne "$x*$y=$[$x*$y]\t"
                let x++
        done
echo
echo
let y++
done

或者
#!/bin/bash
for i in `seq 9`
do
    for j in `seq $i`
    do
        echo -ne  "$j*$i=$[$i*$j]\t"
    done
echo
echo
done
或者
#!/bin/bash
y=1
until [ $y -gt 9 ]
do
        x=1
        until [ $x -gt $y ]
        do
                echo -ne "$x*$y=$[ $x*$y ]\t"
                let x++
        done
echo
echo
let y++
done

~~~

# 三、拓展知识expect 交互性



expect 自动应答  tcl语言

**需求1：**A远程登录到server上什么都不做

~~~powershell
#!/usr/bin/expect
# 开启一个程序
spawn ssh root@10.1.1.1
# 捕获相关内容
expect {
        "(yes/no)?" { send "yes\r";exp_continue }
        "password:" { send "123456\r" }
}
interact   //交互

脚本执行方式：
# ./expect1.sh
# /shell04/expect1.sh
# expect -f expect1.sh

1）定义变量
#!/usr/bin/expect
set ip 10.1.1.2
set pass 123456
set timeout 5
spawn ssh root@$ip
expect {
	"yes/no" { send "yes\r";exp_continue }
	"password:" { send "$pass\r" }
}
interact


2）使用位置参数
#!/usr/bin/expect
set ip [ lindex $argv 0 ]
set pass [ lindex $argv 1 ]
set timeout 5
spawn ssh root@$ip
expect {
	"yes/no" { send "yes\r";exp_continue }
	"password:" { send "$pass\r" }
}
interact

~~~



**需求2:**A远程登录到server上操作

```shell
#!/bin/bash
set ip 10.1.1.1
set pass 123456
set timeout 5
spawn ssh root@$ip
expect{
		"yea/no",{send "yes\r";exp_continue}
		"password:" {send "$pass\r"}
}

expect "#"
send "rm -rf /tmp/*\r"
send "touch /tmp/file{1..3}\r"
send "date\r"
send "exit\r"
expect eof

```

**需求3:“**shell脚本和expect结合使用，在多台服务器上创建1个用户

ip.txt

10.1.1.1 123456

10.1.1.2 123456

```shell
#!/bin/bash
cat ip.txt|while read ip pass
do
	{
		/usr/bin/expect <<-HOU
		spawn ssh root$ip
		expect {
			"yea/no" { send "year\r";exp_continue }
			"password:"{ send "$pass\r" }
		}
		expect "#"
		send "hostname\r"
		send "exit\r"
		expect eof
		HOU
	}&
done
wait
echo "user is ok...."

或者是
#!/bin/bash
while read ip pass
	do
		{
			/usr/bin/expect <<-HOU
			spawn ssh root@$ip
			expect {
				"yes/no" { send "yes\r";exp_continue }
				"password:" { send "$pass\r" }
			}
			expect "#"
        send "hostname\r"
        send "exit\r"
        expect eof
        HOU
		}&
	done<ip.txt
	wait
echo "user is ok...."
```



#四、综合案例

##1. 实战案例1

### ㈠ 具体需求

写一个脚本，将跳板机上yunwei用户的公钥推送到局域网内可以ping通的所有机器上

说明：主机和密码文件已经提供

10.1.1.1:123456

10.1.1.2:123456

###㈡ 案例分析

- **关闭防火墙和selinux**
- 判断ssh服务是否开启（默认ok）
- ==循环判断给定密码文件里的哪些IP是可以ping通== 
- ==判断IP是否可以ping通——>$?—>流程控制语句==
- ==密码文件里获取主机的IP和密码保存变量== 
- ==判断公钥是否存在—>不存在创建它==
- ==ssh-copy-id 将跳板机上的yunwei用户的公钥推送到远程主机—>expect解决交互==
- ==将ping通的主机IP单独保存到一个文件==
- ==测试验证==

### ㈢ 落地实现

~~~powershell
#!/bin/bash
#判断公钥是否存在
[ ! -f /home/yunwei/.ssh/id_rsa ] && ssh-keygen -P '' -f ~/.ssh/id_rsa

#循环判断主机是否ping通，如果ping通推送公钥
tr ':' ' ' < /shell04/ip.txt|while read ip pass
do
{
        ping -c1 $ip &>/dev/null
        if [ $? -eq 0 ];then
        echo $ip >> ~/ip_up.txt
        /usr/bin/expect <<-END &>/dev/null
         spawn ssh-copy-id root@$ip
         expect {
                "yes/no" { send "yes\r";exp_continue }
                "password:" { send "$pass\r" }
                }
        expect eof
        END
        fi
}&
done
wait
echo "公钥已经推送完毕，正在测试...."
#测试验证
remote_ip=`tail -1 ~/ip_up.txt`
ssh root@$remote_ip hostname &>/dev/null
test $? -eq 0 && echo "公钥成功推送完毕"
~~~

##2. 实战案例2

写一个脚本，统计web服务的不同==连接状态==个数

~~~powershell
#!/bin/bash
#count_http_80_state
#统计每个状态的个数
declare -A array1
states=`ss -ant|grep 80|cut -d' ' -f1`
for i in $states
do
        let array1[$i]++
done
#通过遍历数组里的索引和元素打印出来
for j in ${!array1[@]}
do
        echo $j:${array1[$j]}
done
~~~



#三、综合案例

## 1. 任务背景

现有的跳板机虽然实现了统一入口来访问生产服务器，yunwei用户权限太大可以操作跳板机上的所有目录文件，存在数据被误删的安全隐患，所以希望你做一些安全策略来保证跳板机的正常使用。

## 2. 具体要求

1. 只允许yunwei用户通过跳板机远程连接后台的应用服务器做一些维护操作
2. 公司运维人员远程通过yunwei用户连接跳板机时，跳出以下菜单供选择：

~~~powershell
欢迎使用Jumper-server，请选择你要操作的主机：
1. DB1-Master
2. DB2-Slave
3. Web1
4. Web2
h. help
q. exit
~~~

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

~~~powershell
#!/bin/bash
# jumper-server
# 定义菜单打印功能的函数
menu()
{
cat <<-EOF
欢迎使用Jumper-server，请选择你要操作的主机：
1. DB1-Master
2. DB2-Slave
3. Web1
4. Web2
h. help
q. exit
	EOF
}
# 屏蔽以下信号
trap '' 1 2 3 19
# 调用函数来打印菜单
menu
#循环等待用户选择
while true
do
# 菜单选择，case...esac语句
read -p "请选择你要访问的主机:" host
case $host in
	1)
	ssh root@10.1.1.1
	;;
	2)
	ssh root@10.1.1.2
	;;
	3)
	ssh root@10.1.1.3
	;;
	h)
	clear;menu
	;;
	q)
	exit
	;;
esac
done


将脚本放到yunwei用户家目录里的.bashrc里执行：
bash ~/jumper-server.sh
exit

~~~

**进一步完善需求**

为了进一步增强跳板机的安全性，工作人员通过跳板机访问生产环境，但是不能在跳板机上停留。

```powershell
#!/bin/bash
#公钥推送成功
trap '' 1 2 3 19
#打印菜单用户选择
menu(){
cat <<-EOF
欢迎使用Jumper-server，请选择你要操作的主机：
1. DB1-Master
2. DB2-Slave
3. Web1
4. Web2
h. help
q. exit
EOF
}

#调用函数来打印菜单
menu
while true
do
read -p "请输入你要选择的主机[h for help]：" host

#通过case语句来匹配用户所输入的主机
case $host in
	1|DB1)
	ssh root@10.1.1.1
	;;
	2|DB2)
	ssh root@10.1.1.2
	;;
	3|web1)
	ssh root@10.1.1.250
	;;
	h|help)
	clear;menu
	;;
	q|quit)
	exit
	;;
esac
done

自己完善功能：
1. 用户选择主机后，需要事先推送公钥；如何判断公钥是否已推
2. 比如选择web1时，再次提示需要做的操作，比如：
clean log
重启服务
kill某个进程
```

**回顾信号：**

~~~powershell
1) SIGHUP 			重新加载配置    
2) SIGINT			键盘中断^C
3) SIGQUIT      	键盘退出
9) SIGKILL		 	强制终止
15) SIGTERM	    	终止（正常结束），缺省信号
18) SIGCONT	   	继续
19) SIGSTOP	   	停止
20) SIGTSTP     	暂停^Z
~~~

# 四、正则表达式

##1. 正则表达式是什么？

**正则表达式**（Regular Expression、regex或regexp，缩写为RE），也译为正规表示法、常规表示法，是一种字符模式，用于在查找过程中==匹配指定的字符==。

许多程序设计语言都支持利用正则表达式进行**字符串操作**。例如，在Perl中就内建了一个功能强大的正则表达式引擎。

正则表达式这个概念最初是由Unix中的工具软件（例如sed和grep）普及开的。

支持正则表达式的程序如：locate |find| vim| grep| sed |awk

## 2. 正则能干什么？

1. 匹配邮箱、匹配身份证号码、手机号、银行卡号等
2. 匹配某些特定字符串，做特定处理等等

## 3.正则当中名词解释

- **元字符**

  指那些在正则表达式中具有**特殊意义的==专用字符==“”，如：点. 星号（*） 问好（？） 等

- **前导字符**

​		位于**元字符**前面的字符 a b**==c==**,  a00**==0==.**

##4. 第一类正则表达式

### ㈠ 正则中普通常用的元字符

| 元字符                                                       | 功能                                         | 备注      |
| ------------------------------------------------------------ | -------------------------------------------- | --------- |
| .                                                            | 匹配除了换行符以外的==任意单个==字符         |           |
| *                                                            | ==前导字符==出现==0==次或==连续多次==        |           |
| .*                                                           | 任意长度字符                                 | ab.*      |
| ^                                                            | 行首(以...开头)                              | ^root     |
| $      | 行尾(以...结尾)                              | bash$ |                                              |           |
| ^$                                                           | 空行                                         |           |
| []                                                           | 匹配括号里任意单个字符或一组单个字符         | [abc]     |
| [^]                                                          | 匹配不包含括号里任一单个字符或一组单个字符   | [^abc]    |
| ^[]                                                          | 匹配以括号里任意单个字符或一组单个字符开头   | ^[abc]    |
| \^[\^]                                                       | 匹配不以括号里任意单个字符或一组单个字符开头 | \^\[^abc] |

- 示例文本

~~~powershell
# cat 1.txt
ggle
gogle
google
gooogle
goooooogle
gooooooogle
taobao.com
taotaobaobao.com

jingdong.com
dingdingdongdong.com
10.1.1.1
Adfjd8789JHfdsdf/
a87fdjfkdLKJK
7kdjfd989KJK;
bSKJjkksdjf878.
cidufKJHJ6576,

hello world
helloworld yourself
~~~

- 举例说明

```powershell

```



### ㈡ 正则中其他常用元字符

| 元字符    | 功能                                    | 备注         |
| --------- | --------------------------------------- | ------------ |
| \\<       | 取单词的头                              |              |
| \\>       | 取单词的尾                              |              |
| \\<  \\>  | 精确匹配                                |              |
| \\{n\\}   | 匹配前导字符==连续出现n次==             |              |
| \\{n,\\}  | 匹配前导字符==至少出现n次==             |              |
| \\{n,m\\} | 匹配前导字符出现==n次与m次之间==        |              |
| \\(   \\) | 保存被匹配的字符                        |              |
| \d        | 匹配数字（**grep -P**）                 | [0-9]        |
| \w        | 匹配字母数字下划线（**grep -P**）       | [a-zA-Z0-9_] |
| \s        | 匹配空格、制表符、换页符（**grep -P**） | [\t\r\n]     |

**举例说明：**

**举例说明：**

~~~powershell
需求：将10.1.1.1替换成10.1.1.254

1）vim编辑器支持正则表达式
# vim 1.txt
:%s#\(10.1.1\).1#\1.254#g 
:%s/\(10.1.1\).1/\1.254/g 

2）sed支持正则表达式【后面学】
# sed -n 's#\(10.1.1\).1#\1.254#p' 1.txt
10.1.1.254

说明：
找出含有10.1.1的行，同时保留10.1.1并标记为标签1，之后可以使用\1来引用它。
最多可以定义9个标签，从左边开始编号，最左边的是第一个。


需求：将helloworld yourself 换成hellolilei myself

# vim 1.txt
:%s#\(hello\)world your\(self\)#\1lilei my\2#g

# sed -n 's/\(hello\)world your\(self\)/\1lilei my\2/p' 1.txt 
hellolilei myself

# sed -n 's/helloworld yourself/hellolilei myself/p' 1.txt 
hellolilei myself
# sed -n 's/\(hello\)world your\(self\)/\1lilei my\2/p' 1.txt 
hellolilei myself

Perl内置正则：
\d      匹配数字  [0-9]
\w      匹配字母数字下划线[a-zA-Z0-9_]
\s      匹配空格、制表符、换页符[\t\r\n]

# grep -P '\d' 1.txt
# grep -P '\w' 2.txt
# grep -P '\s' 3.txt
~~~



### ㈢ 扩展类正则常用元字符

**==丑话说在前面：==**

我说我比较特殊，你要相信！否则我错给你看:smirk:

- grep你要用我，必须加 **==-E==**  或者  让你兄弟`egrep`来找我

- sed你要用我，必须加 **==-r==**

| 扩展元字符 | 功能                   | 备注                                         |
| ---------- | ---------------------- | -------------------------------------------- |
| +          | 匹配一个或多个前导字符 | bo+ 匹配boo、 bo                             |
| ?          | 匹配零个或一个前导字符 | bo? 匹配b、 bo                               |
| \|         | 或                     | 匹配a或b                                     |
| ()         | 组字符（看成整体）     | (my\|your)self：表示匹配myself或匹配yourself |
| {n}        | 前导字符重复n次        |                                              |
| {n,}       | 前导字符重复至少n次    |                                              |
| {n,m}      | 前导字符重复n到m次     |                                              |

**举例说明：**

~~~powershell
# grep "root|ftp|adm" /etc/passwd
# egrep "root|ftp|adm" /etc/passwd
# grep -E "root|ftp|adm" /etc/passwd

# grep -E 'o+gle' test.txt 
# grep -E 'o?gle' test.txt 

# egrep 'go{2,}' 1.txt
# egrep '(my|your)self' 1.txt


使用正则过滤出文件中的IP地址：
# grep '[0-9]\{2\}\.[0-9]\{1\}\.[0-9]\{1\}\.[0-9]\{1\}' 1.txt 
10.1.1.1
# grep '[0-9]{2}\.[0-9]{1}\.[0-9]{1}\.[0-9]{1}' 1.txt 
# grep -E '[0-9]{2}\.[0-9]{1}\.[0-9]{1}\.[0-9]{1}' 1.txt 
10.1.1.1
# grep -E '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' 1.txt 
10.1.1.1
# grep -E '([0-9]{1,3}\.){3}[0-9]{1,3}' 1.txt 
10.1.1.1

~~~

##5. 第二类正则

| 表达式    | 功能                             | 示例            |
| --------- | -------------------------------- | --------------- |
| [:alnum:] | 字母与数字字符                   | [[:alnum:]]+    |
| [:alpha:] | 字母字符(包括大小写字母)         | [[:alpha:]]{4}  |
| [:blank:] | 空格与制表符                     | [[:blank:]]*    |
| [:digit:] | 数字                             | [[:digit:]]?    |
| [:lower:] | 小写字母                         | [[:lower:]]{4,} |
| [:upper:] | 大写字母                         | [[:upper:]]+    |
| [:punct:] | 标点符号                         | [[:punct:]]     |
| [:space:] | 包括换行符，回车等在内的所有空白 | [[:space:]]+    |

~~~powershell
[root@server shell05]# grep -E '^[[:digit:]]+' 1.txt
[root@server shell05]# grep -E '^[^[:digit:]]+' 1.txt
[root@server shell05]# grep -E '[[:lower:]]{4,}' 1.txt
~~~



## 6. 正则表达式总结

**把握一个原则，让你轻松搞定可恶的正则符号：**

1. 我要找什么？
   - 找数字  		                  [0-9]
   - 找字母                                    [a-zA-Z]
   - 找标点符号                            [[:punct:]]
2. 我要如何找？看心情找
   - 以什么为首                           ^key
   - 以什么结尾                           key$
   - 包含什么或不包含什么        [abc]  \^[abc]   [\^abc]   \^[\^abc]
3. 我要找多少呀？
   - 找前导字符出现0次或连续多次             ab==*==
   - 找任意单个(一次)字符                             ab==.==
   - 找任意字符                                               ab==.*==
   - 找前导字符连续出现几次                        {n}  {n,m}   {n,}
   - 找前导字符出现1次或多次                      go==+==
   -  找前到字符出现0次或1次                       go==?==                  

# 五、正则元字符一栏表

**元字符**：在正则中，具有特殊意义的专用字符，如: 星号(*)、加号(+)等

**前导字符**：元字符前面的字符叫前导字符

| 元字符                                                       | 功能                                     | 示例              |
| ------------------------------------------------------------ | ---------------------------------------- | ----------------- |
| *                                                            | 前导字符出现0次或者连续多次              | ab*  abbbb        |
| .                                                            | 除了换行符以外，任意单个字符             | ab.   ab8 abu     |
| .*                                                           | 任意长度的字符                           | ab.*  adfdfdf     |
| []                                                           | 括号里的任意单个字符或一组单个字符       | [abc]\[0-9]\[a-z] |
| [^]                                                          | 不匹配括号里的任意单个字符或一组单个字符 | [^abc]            |
| ^[]                                                          | 匹配以括号里的任意单个字符开头           | ^[abc]            |
| \^[^]                                                        | 不匹配以括号里的任意单个字符开头         |                   |
| ^                                                            | 行的开头                                 | ^root             |
| $                | 行的结尾                                 | bash$ |                                          |                   |
| ^$                                                           | 空行                                     |                   |
| \\{n\\}和{n}                                                 | 前导字符连续出现n次                      | [0-9]\\{3\\}      |
| \\{n,\\}和{n,}                                               | 前导字符至少出现n次                      | [a-z]{4,}         |
| \\{n,m\\}和{n,m}                                             | 前导字符连续出现n-m次                    | go{2,4}           |
| \\<\\>                                                       | 精确匹配单词                             | \\<hello\\>       |
| \\(\\)                                                       | 保留匹配到的字符                         | \\(hello\\)       |
| +                                                            | 前导字符出现1次或者多次                  | [0-9]+            |
| ?                                                            | 前导字符出现0次或者1次                   | go?               |
| \|                                                           | 或                                       | \^root\|\^ftp     |
| ()                                                           | 组字符                                   | (hello\|world)123 |
| \d                                                           | perl内置正则                             | grep -P  \d+      |
| \w                                                           | 匹配字母数字下划线                       |                   |

# 六、正则练习作业

## 1. 文件准备

```powershell
# vim test.txt 
Aieur45869Root0000
9h847RkjfkIIIhello
rootHllow88000dfjj
8ikuioerhfhupliooking
hello world
192.168.0.254
welcome to uplooking.
abcderfkdjfkdtest
rlllA899kdfkdfj
iiiA848890ldkfjdkfj
abc
12345678908374
123456@qq.com
123456@163.com
abcdefg@itcast.com23ed
```



## 2. 具体要求

```powershell
1、查找不以大写字母开头的行（三种写法）。
grep '^[^A-Z]' 2.txt
grep -V '[^A-Z]' 2.txt
grep '^[^[:upper:]]'
2、查找有数字的行（两种写法）
grep '[0-9]' 2.txt
grep -P '\d' 2.txt
3、查找一个数字和一个字母连起来的
grep -E '[0-9][a-zA-Z]|[a-zA-Z][0-9]' 2.txt
4、查找不以r开头的行
grep -V '^r' 2.txt
grep '^[^r]' 2.txt
5、查找以数字开头的
grep '^[0-9]'  2.txt
6、查找以大写字母开头的
grep '^[A-Z]' 2.txt
7、查找以小写字母开头的
grep '^[a-z]' 2.txt
8、查找以点结束的
grep '\.$' 2.txt
9、去掉空行
grep -V '^$' 2.txt
10、查找完全匹配abc的行
grep '\<abc\>' 2.txt
11、查找A后有三个数字的行
grep -E 'A[0-9]{3}' 2.txt
12、统计root在/etc/passwd里出现了几次
grep -o 'root' | wc -l

13、用正则表达式找出自己的IP地址、广播地址、子网掩码
ifconfig eth0 | grep Bcast | grep -o '[0-9]\{1,3\}.[0-9]\{1,3}.[0-9]\{1,3\}.[0-9]\{1,3\}'
ifconfig eth0 | grep Bcast | grep -E -o "([0-9]{1,3}.){3}[0-9]{1,3}"
ifconfig eth0 | grep Bcast | grep -P -o '\d{1,3}.\d{1,3}.\d{1,3}.\d{1,3}'
ifconfig eth0 | grep Bcast | grep -P -o '(\d+.){3}\d+'

# egrep --color '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' /etc/sysconfig/network-scripts/ifcfg-eth0
IPADDR=10.1.1.1
NETMASK=255.255.255.0
GATEWAY=10.1.1.254

# egrep --color '[[:digit:]]{1,3}\.[[:digit:]]{1,3}\.[[:digit:]]{1,3}\.[[:digit:]]{1,3}' /etc/sysconfig/network-scripts/ifcfg-eth0 
IPADDR=10.1.1.1
NETMASK=255.255.255.0
GATEWAY=10.1.1.254

14、找出文件中的ip地址并且打印替换成172.16.2.254
grep -o -E '([0-9]{1,3}\.){3}[0-9]{1,3}' 1.txt | sed -n 's/192.168.0.\(254\)/172.16.2.\1/p'

15、找出文件中的ip地址
grep -o -E '([0-9]{1,3}\.){3}[0-9]{1.3}' 2.txt

16、找出全部是数字的行
grep -E '^[0-9]+$' test

17、找出邮箱地址
grep -E '^[0-9]+@[a-z0-9]+\.[a-z]+$'

grep --help:
匹配模式选择：
Regexp selection and interpretation:
  -E, --extended-regexp     扩展正则
  -G, --basic-regexp        基本正则
  -P, --perl-regexp         调用perl的正则
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         忽略大小写
  -w, --word-regexp         匹配整个单词

```

#七、课后作业

## 脚本搭建web服务

**要求如下**：

1. 用户输入web服务器的IP、域名以及数据根目录
2. 如果用户不输入则一直提示输入，直到输入为止
3. 当访问www.test.cc时可以访问到数据根目录里的首页文件“this is test page” 

**参考脚本：**

```shell
#!/bin/bash
conf=/etc/httpd/conf/httpd.conf
input_fun()
{
	input_var=""
	output_var=$1
	while [ -z $input_var ]
	do
		read -p "$output_var" input_var
	done
		echo $input_var
}
ipaddr=$(input_fun "input Host ip[192.168.0.1]:")
web_host_name=$(input_fun "input virtualHostName [222.test.cc]:")
root_dir=$(input_fun "input host Documentroot dir:[/var/www/html]:")

[ ! -d $root_dir ] && mkdir -p $root_dir
chown apache.apache $root_dir && chmod 755 $root_dir
echo this is $web_host_name > $root_dir/index.html
echo "$ipaddr $web_host_name" >> /etc/hosts

[ -f $conf ] && cat >> $conf <<end
NameVirtualHost $ipaddr:80
<VirtualHost $ipaddr:80>
	ServerAdim webmaster@web_host_name
	DocumentRoot $root_dir
	ServerName $web_host_name
	ErrorLog logs/$web_host_name-error_log
	CustomLog logs/$web_host_name-access_loh common
</VirtualHost>
end
```





















































