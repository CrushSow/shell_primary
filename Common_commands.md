#### 常用的shell 命令

批量创建文件

```shell
mkdir -p a/b/c
```

每个文件夹下面去创建文件

```shell
mkdir {a,b,b}/src
```

创建多了文件

```shell
touch html/{index.html,page.html}
```

创建多个文件并进去其中一个

```shell
mkdir {html,asset} & cd html
```

创建多个文件并打开

```shell
touch html/{index.html,page.html} & st html{index.html,page.html}
```

luck_num=`head -$luck_line $phone | tail -l`

#产生一个保存用户名和密码的文件
echo user0{1..5}:itcast$[$RANDOM%9000+1000]#@~|tr ' ' '\n'>> user_pass.file

echo $pass|passwd --stdin $user

trap '' 1 2 3 19



