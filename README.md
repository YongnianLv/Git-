# Git 入门总结

Git~~
1.0分类
集中式：CSV ,SVN,VSS
分布式：Git，Darcs,...

1.1本地库初始化

进入文件夹
ll #查看列表
ls -lA #查看隐藏列表
git init #本地库初始化
注意：生成的 .git 目录中存放的是本地库相关文件，不要删除

1.2设置签名

项目(仓库)级别仅在当前本地库有效
git config user.name tom  #设置用户名tom
git config user.email tom@qq.com #设置用户邮箱
查看信息：cat .git/config 

系统用户级别仅在当前登录的操作系统用户有效
git config --global user.name tony
git config --global user.email tony@qq.com
仅加了 --global
查看信息：cat ~/.gitconfig

优先级别：项目级别 > 系统级别

1.3基本操作

1.3.1 状态查看
git status   #查看工作区、暂存区状态

1.3.2 添加
git add fileName  #指定文件
git add . #所有
说明：将工作区的"新建/修改"添加到暂存区

1.3.3 提交
git commit -m 'commit message' fileName
说明：将暂存区内容提交到本地库

1.3.4 查看历史记录
git log 
git reflog  #常用
git log --pretty=oneline #漂亮一行显示
git log --oneline #简洁显示
说明：HEAD@{移动到当前版本需要多少步}
1.3.5 前进后退

基于索引值---推荐

git reset --hard [局部索引值]
例子：git reset --hard c6ace56 #回到这个状态

使用 ^ 符号只能后退
git reset --hard HEAD^
例子：git reset --hard HEAD^^
注意：几个 ^ 表示后退几步

使用 ~ 符号只能后退
git reset --hard HEAD~n
例子：git reset --hard HEAD~3

1.3.6 reset的三个参数比较
soft: 
  - 仅本地库移动HEAD 指针
mixed:
  - 在本地库移动HEAD指针
  - 重置暂存区
hard:
  - 在本地库移动HEAD指针
  - 重置暂存区
  - 重置工作区
  
1.3.7　删除文件并找回
相当于建立一个快照，虽然删除了，但只要添加到暂存区，就能找回
git reset --hard 指针位置

1.3.8 文件差异比较
git diff 文件名
git diff 哈希值 文件名  #和历史中的一个版本比较
git diff  #不带文件名，则比较多个文件

2.1 分支管理
hot_fix master feature_x feature_y

2.2.1 什么是分支管理
在版本控制中，使用推进多个任务

2.2.2 分支的好处
同时并行推进多个功能开发，提高开发效率
某一分支开发失败，不会对其它分支有任何影响

2.2.3 分支操作
创建分支
git branch 分支名
查看分支
git branch
git branch -v 

切换分支
git checkout 分支名
git checkout -b 分支名   #创建分支并直接切换到该分支

合并分支相当于把修改了的文件拉过来
git merge xxa
注意：合并分支的时候要明确谁谁合并
若xxa分支里面修改，要合并到master，就先切换到master，然后合并xxa

删除分支
git branch -d 分支名

2.2.4 解决冲突

第一步：编辑，删除特殊标记<<< ===
第二步：修改到满意位置，保存退出
第三步：添加到缓存区 git add 文件名
第四步：提交到本地库git commit -m '日志信息' 注意：后面一定不能带文件名

Git 结合Github

1.1 创建远程库地址别名
git remote -v  #查看远程地址别名
git remote add 别名 远程地址 
例子：git remote add origin https://xx

1.2 推送
开发修改完把本地库的文件推送到远程仓库 前提是提交到了本地库才可以推送

git push 别名 分支名
git push -u 别名 分支名    #-u指定默认主机
例子：git push origin master

1.3 克隆
完整的把远程库克隆到本地，从无到有的过程，更新用pull

git clone  远程地址
例子：git clone https://xx

1.4 拉取
本地存在clone下来的文件 就用pull更新

pull = fetch + merge
	git fetch 别名 分支名
	git merge 别名 分支名
git pull 别名 分支名

1.5 解决冲突
先 git pull origin master
然后和以前处理分支冲突方式一样
最后 git push origin master
注意：解决冲突后的提交是不能带文件名的

如果不是基于远程库最新版做的修改不能推送，必须先pull下来安装冲突办法解决

1.6 SSH 免密登录
输入:ssh-keygen -t rsa -C GitHub邮箱地址
进入.ssh目录，复制id_rsa.pub文件内容
登录GitHub。Settings --> SSH and GPG keys --> New SSH Key
回到git通过ssh地址创建。git remote add 别名 SSH地址
