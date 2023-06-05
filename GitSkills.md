#Git
<img decoding="async" src="img/Git.png" width="50%">

>**查看 ssh 公钥方法**：
打开你的 git bash 窗口
进入 .ssh 目录：cd ~/.ssh
找到 id_rsa.pub 文件：ls
查看公钥：cat id_rsa.pub 或者 vim id_rsa.pub
粘贴公钥至gitee的个人设置-SSH公钥中

>**生成密钥**
查找不到公钥就先生成
git输入ssh-keygen -t rsa命令，指定RSA算法生成密钥，然后敲三次回车键，期间不需要输入密码，之后就就会生成两个文件，分别为>id_rsa和id_rsa.pub，即密钥id_rsa和公钥id_rsa.pub. 对于这两个文件，其都为隐藏文件，默认生成在以下目录：
Linux 系统：~/.ssh
Mac 系统：~/.ssh
Windows 系统：C:\Documents and Settings\username\.ssh
Windows 10 ThinkPad：C:\Users\think.ssh
```
git add .
git commit -m "wrote a readme file"
git push origin main
```

离线工具链代码管理规范：
1. 远程已经创建好master和dev分支，开发阶段请切换到dev分支进行提交
2. 在本地克隆
    git clone http://gitee.zhejianglab.com:80/enterprise/csyt-toolchain.git
    中途会跳出认证页面 输入实验室邮箱账号和密码
3. 克隆成功后会在当前目录下出现子目录csyt-toolchain，进入该子目录并切换dev分支
   git checkout develop
4. 本地开发修改代码前：先拉取远程代码保持同步，以免提交时出现冲突
   git pull origin develop
   进入个人对应子目录上传和修改代码，如compler目录为编译器代码
5. 本地修改完成后：提交代码至远程库
   ```
    git add .
    git commit -m "此处写修改人名字和主要修改内容"
    git push # 提交dev分支代码远程
    ```
6. 最终上线采用master分支
   ```
   git checkout master
   git merge dev  # 把dev分支的更改和master合并
   git push  # 提交主分支代码远程
   ```


##创建版本库

1. 新建目录，通过git init命令把这个目录变成Git可以管理的仓库
2. 在该目录或子目录下新建文件
3. 添加到仓库，把文件修改添加到暂存区git add readme.txt
4. 提交到仓库，暂存区的所有内容提交到当前分支git commit -m "wrote a readme file"
*HEAD指向的版本就是当前版本 上一个版本就是HEAD\^，上上一个版本就是HEAD\^^，上100个版本写成HEAD~100*

##远程库
###远程库关联
1. 要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
2. 关联一个远程库时必须给远程库指定一个名字，origin是默认习惯命名；
3. 关联后，使用命令git push -u origin main第一次推送main分支的所有内容；
4. 此后，每次本地提交后，可以使用命令git push origin main推送最新修改


##撤销修改
**工作区**：git checkout -- filename  // 清理工作区（回到最近一次git commit或git add时的状态，可以恢复误删的文件）
**stage**：
1. git reset HEAD filename  // stage → 工作区（把上次add但未提交的stage中的内容退回工作区）
2. git checkout -- filename清理工作区

**分支**：
1. git reset --soft HEAD^  // HEAD^也可写作HEAD~1；soft表示不删除工作区改动代码，撤销commit，不撤销add；
2. git reset --hard HEAD^或git reset --hard 版本号[^1] // hard表示删除工作区改动代码，撤销commit和add
3. git commit --amend  // 只修改注释，进入vim编辑器，修改完注释后保存即可
4. 已经提交：git pull origin <远程分支名>  //将远程指定分支拉取到本地当前分支上
[^1]:输入前几位即可

##分支管理

- 查看分支：git branch
- 创建分支：git branch \<name>
- 删除分支：git branch -d \<name>
- 切换分支：git checkout \<name>或者git switch \<name>
- 创建+切换分支：git checkout -b \<name>或者git switch -c \<name>
- 合并某分支到当前分支：git merge \<name>


##多人协作
1. 查看远程库信息，使用git remote -v；本地新建的分支如果不推送到远程，对其他人就是不可见的；
2. 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
3. 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
4. 如果有冲突，要先本地处理冲突再重新提交
5. git pull失败说明未关联，先建立本地分支和远程分支的关联，使用git branch --set-upstream-to branch-name origin/branch-name；


##常用命令
```git
git status	查看工作区的状态	
git diff	查看工作区和版本库里面最新版本的区别	
git log	历史提交版本记录	--pretty=oneline
git reflog	历史命令	
git rm	删除版本库中的文件	
git remote -v	查看远程库信息
git remote rm	删除远程库（断开关联）	
git branch	查看当前分支,当前分支前面会标一个*号	
git branch <name>	创建分支	
git log --graph 看分支合并图。
```

###密钥SSH
[windows下GitHub的SSH key配置](https://www.jianshu.com/p/9317a927e844)
[Github配置ssh key的步骤](https://blog.csdn.net/weixin_42310154/article/details/118340458)
		
 参考
 [Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
 [Git使用master和dev分支](https://www.zhihu.com/question/21995370)
 [Git 开发流程](https://www.cnblogs.com/midworld/p/13608008.html)



		
		
