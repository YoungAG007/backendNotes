# Git(GitHub/Gitee/GitLab)

## 课程介绍

![image-20211016133936467](Git(GitHubGiteeGitLab).assets/image-20211016133936467.png)





git ini 初始化当前工作区

git status 查看当前工作区状态

vim hello.txt 创建hello.txt文件

​                      进入编辑状态的快捷键  i

​                      退出编辑状态的快捷键 esc

​                      复制整行的快捷键         yy

​                      黏贴整行的快捷键         p

​                      保存并退出                    ：wq

cat hello.txt    查看文件全部内容

tail -n 1 hello.txt 查看文件最后一行的内容

git add hello.txt  添加文件进入暂存区(进行track)

git rm --cached hello.txt  从暂存区中将文件清除

git commit -m"my first commit" hello.txt  (其中-m"my first commit"是日志信息)

git reflog    查看版本信息

git log        查看完整日志信息（提交的完整版本号  以及 提交者）

git reset --hard  版本号     版本穿梭

git branch -v    查看当前分支

git branch 分支名     创建分支

git merge 分支名   合并分支

代码冲突问题： 修改了同一行

git merge 分支名    -->    git add hello.txt  --> git commit -m "merge test"  (不要带文件名，否则会出错)





git remote -v   查看远程库别名 

git remote add  git-demo(别名) https://github.com/YoungAG007/git-demo.git(远程链接)

git push 别名 分支           本地库提交到远程库

git clone 远程地址            clone 会做如下操作。 1、拉取代码。 2、初始化本地仓库。 3、创建别名  

git pull 远程地址 远程分支名       拉取远程仓库的最新文件



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



git config user.email   这个邮箱就是本机向github提交的账户



https://blog.csdn.net/qq_34466755/article/details/113748527

https://blog.csdn.net/qq_36614846/article/details/70280168?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0.no_search_link&spm=1001.2101.3001.4242.0
