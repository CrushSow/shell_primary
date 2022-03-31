### Git

#### git fetch 和git pull的区别是

#####简单概述

git在本地会保存两个版本的仓库，分为local 和 remote

- local 我们可以有的操作是 add，commit 
- remote 可以是使用 git remote -v（这里的仓库是保存在本地仓库的远程仓库，等同于另外一个版本，不是远程的远程仓库）



fetch 和 pull的不同

fetch只能更新远程仓库的代码为最新的，本地仓库的代码还未被更新，我们需要通过git merge origin/master来合并这两个版本，你可以把它理解为合并分支一样的



pull的操作是将本地仓库和远程仓库（本地的）更新到远程的最新版本



==如果想要更加可控一点推荐使用fetch+merge==

##### 底层原理

准备知识：

![refs](picture\refs.png)

> 这里看shellresouce这个仓库，从config这个配置文件我们能够知道

- 当前分支是main分支，关联的远程仓库是shellresouce

refs下的目录结构：

![refs目录结构](picture\refs目录结构.png)



- ==heads里面存放的是本地最新的提交返回的commitId==
- ==remotes里面存放的是不同远程仓库下不同分支的最新的提交的commitId==
- tags是打标签，tag执行的是一次commit的id，如标记一个版本号「这里不涉及」
- .git/log文件保存着我们操作的日志 需要用到



先明确：

```powershell
本地main分支的最新commitid是：758ba6a58341a49b3c8b3b157fe42b4f5822f490
github上面最新的commit id是：ce71505b3626a3648b2c32eac92001ef8ae86104
```



git fetch

开始前：

.git\refs\heads\main

`758ba6a58341a49b3c8b3b157fe42b4f5822f490`

.git\refs\remotes\shellresource\main

`758ba6a58341a49b3c8b3b157fe42b4f5822f490`

.git\logs\refs\heads\main

```shell
758ba6a58341a49b3c8b3b157fe42b4f5822f490 luzzo <577535400@qq.com> 1648260926 +0800	commit: 20220326 markdown note learn for PingPingS
```

.git\logs\refs\remotes\shellresource\main

```shell
0000000000000000000000000000000000000000 
758ba6a58341a49b3c8b3b157fe42b4f5822f490 luzzo <577535400@qq.com> 1648046206 +0800	update by push
```

之后：

.git\refs\heads\main（不变）

.git\refs\remotes\shellresource\main

`ce71505b3626a3648b2c32eac92001ef8ae86104`

.git\logs\refs\heads\main (不变)

.git\logs\refs\remotes\shellresource\main

```shell
0000000000000000000000000000000000000000 
758ba6a58341a49b3c8b3b157fe42b4f5822f490 luzzo <577535400@qq.com> 1648046206 +0800	update by push

758ba6a58341a49b3c8b3b157fe42b4f5822f490 
ce71505b3626a3648b2c32eac92001ef8ae86104 luzzo <577535400@qq.com> 7248046206 +0800	fetch origin: fast-forward
```

==本地库并没有变化，也就是说，git fetch只会将本地库所关联的远程库的commit id更新至最新==

==只是idea里面的remote对应的分支是最新的代码==





先明确

```powershell
本地main分支的最新commitid是：99f5559c863454285ee10b35864bbc717279b5b2
github上面最新的commit id是：e6fd8e55fed365444c51bd074573136906f05edc
```



git pull

开始前：

.git\refs\heads\main

`99f5559c863454285ee10b35864bbc717279b5b2`

.git\refs\remotes\shellresource\main

`99f5559c863454285ee10b35864bbc717279b5b2`

.git\logs\refs\heads\main

```shell
7aa9d29b2c090e8e3920ee6a2dda483ed0062d1f 
luzzo <577535400@qq.com> 1647875268 +0800	commit: second commit awk update
```

.git\logs\refs\remotes\shellresource\master

```shell
7aa9d29b2c090e8e3920ee6a2dda483ed0062d1f 
luzzo <577535400@qq.com> 1647875268 +0800	update by push
```



之后：

.git\refs\heads\master

`e6fd8e55fed365444c51bd074573136906f05edc`

.git\refs\remotes\shellresource\master（不变）

.git\logs\refs\heads\master

```shell
7aa9d29b2c090e8e3920ee6a2dda483ed0062d1f 
luzzo <577535400@qq.com> 1647875268 +0800	update by push

7aa9d29b2c090e8e3920ee6a2dda483ed0062d1f
e6fd8e55fed365444c51bd074573136906f05edc  
luzzo <577535400@qq.com> 1647873874 +0800 pull shellresource:shellresouce:fast-forward
```

.git\logs\refs\remotes\shellresource\master（不变）



==本地库更新至远程仓库的最新代码，但是本地的remote不会更新最新的代码==

从结果来看git pull = git fetch + git merge ,但是底层的原理不是这个样子的

![流程简图](picture\流程简图.png)




















































