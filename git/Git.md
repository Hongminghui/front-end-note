## 一. 邂逅git

`bash`作为git的语言

### 1. 安装

见如下链接

```
https://www.jianshu.com/p/b8755c9c5183
```

### 2. 用户信息配置

这些命令在任意目录下，右键使用git Bash here都可以使用

```bash
$ git config --global user.name "damu" // 配置用户名
$ git config --global user.email damu@example.com // 配置用户邮箱
```

--system：配置系统，系统下所有用户都遵循这个配置

--global：配置用户，该用户所有项目都遵循这个配置

: 空白，表示配置当前项目，优先级最高

```
git config --system user.name "damu" // 配置系统用户名
git config --global user.name "damu" // 配置用户名
git config user.name "damu" // 配置项目用户名
```

```
git config --list 查看已有的配置列表
git config --global --unset user.email 删除配置信息
git config --global user.name "damu" 重设配置信息，只需要把设置的语句再写一次，就会覆盖
```

### 3. 基础的linux命令

```
 基础的 linux 命令
clear ：清除屏幕

echo 'test content'：往控制台输出信息 
echo 'test content' > test.txt：将'test content' 输入到test.txt文件，如果文件不存在，就创建该文件

ll ：将当前目录下的 子文件&子目录平铺在控制台
find 目录名： 将对应目录下的子孙文件&子孙目录平铺在控制台
find 目录名 -type f ：将对应目录下的文件平铺在控制台
rm 文件名 ： 删除文件
mv 源文件 重命名文件: 重命名
cat 文件的url : 查看对应文件的内容
vim 文件的 url(在英文模式下)
 按 i 进插入模式 进行文件的编辑
 按 esc 键&按:键 进行命令的执行
 q! 强制退出（不保存）
 wq 保存退出
 set nu 设置行号
```

### 4. 初始化新仓库

```
命令：git init
```

例如`workspace`是一个空目录，作为项目的总目录，就在`workspace`内，使用`git init`命令，此时workspace中就会生成.git目录（隐藏目录），如下是.git目录结构：

![image-20201208110313459](C:\Users\洪明辉\AppData\Roaming\Typora\typora-user-images\image-20201208110313459.png)

## 二. 底层命令

对象：

`git`对象：文件快照

树对象：项目快照

提交对象：项目快照 + 提交信息 + 注释

区域：

工作区 暂存区 版本库

```
git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4 返回文件对应的内容
git cat-file -t d670460b4b4aece5915caf5c68d12f560a9fe3e4 返回文件对应的类型
```

## 三. 高层命令

工作目录(workspace下的目录)下的文件都分为已跟踪和未跟踪，

已跟踪的文件分为：已提交，已修改或者已暂存

### 1. 基本命令

```
1. 初始化
git init 初始化新仓库

2. 查看状态
git status 查看工作目录下文件的状态

git diff 可以查看做了哪些修改，还没有暂存
git diff --cached/--staged（较新版本git）查看哪些修改暂存了，还没有更新

3. 加入暂存区
git add ./ 跟踪所有文件，并将所有文件加入暂存区
git add + 文件名 跟踪该文件，并将该文件加入暂存区

4. 提交，加入版本库
git commit 提交所有暂存区的文件，提交之后会进入vim编辑器，在这添加注释
给文件做了修改之后，要重新`git add ./`不然`git commit`提交的就是修改之前的文件
git commit -m "message" 引号内添加注释信息
git commit -a 提交跟踪过的文件，不需要git add ./这个步骤

5. 删除文件
git rm + 文件路径：该文件必须是已经跟踪过的文件，会在工作目录中删除这个文件

6. 重命名文件
git mv + 旧文件名 + 新文件名

7. 查看提交记录
git log 显示提交记录，包含作者，邮箱，注释，哈希值
git log --oneline 提交记录变成一行显示，只包含注释，哈希值
git log --pretty=oneline 似乎只是哈希值显示的更完整（相比第二条）
git log --oneline --decorate 查看当前分支所指对象
git log --oneline --decorate --graph --all 查看项目分叉历史
git reflog 查看所有提交记录，包括撤回的
git log -g，显示的信息更完整
```

### 2. 分支操作

```
1. 创建分支 ：git branch + 分支名
	创建分支并不会自动切换
	
2. 查看所有分支：git branch

3. 删除分支	git branch -d + 分支名
		   git branch -D + 分支名：强制删除，可以删除未合并的分支
		   
4. 查看每一个分支的最后一次提交 git branch -v

5. 新建一个分支并且使分支指向对应的提交对象
    git branch + 新分支名 + 提交的哈希值
    利用这个命令似乎可以完成版本穿梭

6. 查看合并分支
    git branch –merged：查看哪些分支已经合并到当前分支
    git branch --no-merged：查看所有包含未合并工作的分支

7. 切换分支
    git checkout + 分支名：head指针移动到新分支上
    切换分支时，工作目录会被改变，最好切换前提交一次

8. 查看项目分叉历史
	git log --oneline --decorate --graph --all

9. 合并分支
    git merge + 分支名 
    合并分支         : git merge branchname (合并要回到主分支)
        快进合并 --> 不会产生冲突
        典型合并 --> 有机会产生冲突，修改冲突文件 add commit
        解决冲突 --> 打开冲突的文件 进行修改 add commit 
        
10. 查看提交对象/暂存区
    git cat-file -p HEAD 查看当前提交对象
    git ls-files -s 查看暂存区当前的样子
11. 重命名分支
	git branch -M main 将当前分支重命名为main
	
```

### 3. 存储配别名

```
git stash 命令会将未完成的修改保存到一个栈上，而你
可以在任何时候重新应用这些改动(git stash apply)
git stash list:查看存储
git stash apply stash@{2}
如果不指定一个储藏，Git 认为指定的是最近的储藏
git stash pop 来应用储藏然后立即从栈上扔掉它
git stash drop 加上将要移除的储藏的名字来移除它
```

配别名

```
$ git config --global alias.br branch
$ git config --global alias.ci commit // 需要输入git commit时，只需要输入git ci即可
$ git config --global alias.st status 
```

### 4. 撤销重置

```
git commit --amend 似乎只是修改提交的注释
git reset HEAD + 文件名 将文件从暂存区中撤回到工作目录
git checkout -- 文件名 将在工作目录中对文件的修改撤销，危险慎用
```

```
git reset --soft commithash    ---> 用commithash的内容重置HEAD内容
git reset [--mixed] commithash ---> 用commithash的内容重置HEAD内容 重置暂存区
git reset --hard commithash    ---> 用commithash的内容重置HEAD内容 重置暂存区 重置工作目录
```

数据恢复：恢复已经被删除的分支

```
1. git reflog 每一次HEAD的改变都会被记录
   或者git log -g，显示的信息更完整
2. git branch + 新分支名 + 删掉分支的哈希值
   这样就能恢复之前的分支了
```

### 5. tag

## 四. github

### 1. 忽略文件

```
一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪
文件列表。通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临
时文件等。我们可以创建一个名为 .gitignore 的文件，列出要忽略的文件模式。
*.[oa]
*~
第一行告诉 Git 忽略所有以 .o 或 .a 结尾的文件。一般这类对象文件和存档文
件都是编译过程中出现的，我们用不着跟踪它们的版本。第二行告诉 Git 忽略所
有以波浪符（~）结尾的文件
```

### 2. 基本操作

```
git remote add + 远程仓库别名(自己取) + 远程仓库url(https/ssh)：连接远程仓库
git remote -v：显示远程仓库别名和地址
git remote show：查看远程仓库别名
git remote show + 远程仓库别名：查看某一个远程仓库的更多信息
git remote rename + 远程仓库别名 + 新远程仓库别名：重命名
git remote rm [remote-name]：删除远程仓库
```



### 3. 团队协作流程

```
第一步: 项目经理创建一个空的远程仓库
第二步: 项目经理创建一个待推送的本地仓库
第三步: 为远程仓库配别名  配完用户名 邮箱
第四步: 在本地仓库中初始化代码 提交代码
第五步: 推送
第六步: 邀请成员
第七步: 成员克隆远程仓库
第八步: 成员做出修改
第九步: 成员推送自己的修改
第十步: 项目经理拉取成员的修改
```

```bash
1. 项目经理初始化远程仓库
    一定要初始化一个空的仓库（不勾选Initialize）; 在github上操作
2. 项目经理创建本地仓库
    git remote 别名 仓库地址(https)
    git init ; 将源码复制进来
    修改用户名 修改邮箱
    git add
    git commit 
3. 项目经理推送本地仓库到远程仓库
    清理windows凭据
    git push  别名 分支  (输入用户名 密码;推完之后会附带生成远程跟踪分支)
 4. 项目邀请成员 & 成员接受邀请
      在github上操作  

5. 成员克隆远程仓库
    git clone  仓库地址 (在本地生成.git文件 默认为远程仓库配了别名 orgin)
                只有在克隆的时候 本地分支master 和 远程跟踪分支别名/master 是有同步关系的
               (git clone会将github上的代码拷贝到当前目录中，所以应该是在一个空仓库执行这个命令)
               在test目录，cmd，会将仓库（包括仓库名night）test目录下
               即出现test > night > src... 目录结构
6. 成员做出贡献
    修改源码文件
    git add 
    git commit 
    git push  别名 分支 (输入用户名 密码;推完之后会附带生成远程跟踪分支) 
    有成员修改，就先git pull + 远程仓库名 + 远程分支名
    然后再git push
    git push -u origin master 上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。
7. 项目经理更新修改
    git fetch 别名 (将修改同步到远程跟踪分支上)
    git merge 远程跟踪分支
```

### 5. 推送其他分支

```
git push origin serverfix
向远程仓库origin推送本地分支serverfix，远程仓库中的分支也叫serverfix

git push origin serverfix:awesomebranch
向远程仓库origin推送本地分支serverfix，并将远程仓库中这个分支命名为awesomebranch

git fetch origin
下一次其他协作者从服务器上抓取数据时，他们会在本地生成一个远程 跟 踪 分 支 origin/serverfix ， 指 向 服 务 器的 serverfix 分支的引用。这种情况下，不会有一个新的 serverfix 分支 - 只有一
个不可以修改的 origin/serverfix 指针。

git merge origin/serverfix (其他协作者)
可以运行 git merge origin/serverfix 将这些工作合并到当前所在的分支。如果想要在自己的 serverfix 分支上工作，可以将其建立在远程跟踪分支之上：
git checkout -b serverfix origin/serverfix （其他协作者）
```

## 五. 问题

问题一：推送到了master分支，而不是main分支

本地`git init`之后还是master分支

1. `git add *`
2. `git commit -m '修改文件'`
3. `git remote add 远程git地址`
4. `git push origin master`

查看github的仓库地址，是推送到了master分支
需要合并到main分支

1. `git fetch origin`
2. `git checkout main`
3. `git merge master --allow-unrelated-histories`（合并分支解决冲突）
   ![在这里插入图片描述](https://img-blog.csdnimg.cn/20210113194939686.png)
   直接合并分支，显示
   `fatal:refusing to merge unrelated histories`
4. `git add *`
5. `git commit -m '合并分支'`
6. `git push`

```
// 切换分支，推送main
git checkout -b main

git push origin main
```

