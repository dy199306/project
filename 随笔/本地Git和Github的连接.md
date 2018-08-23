# 1）本地Git和Github的连接

### 1、本地配置用户名和邮箱（--global配置的全局的用户名和邮箱）

- git config --global user.name "你的用户名"	
- git config --global user.email "你的邮箱"

### 2、生成ssh key

ssh-keygen -t rsa -C "346075646@qq.com"

执行完之后会提示储存ssh-key的文件路径   

 “Your public key has been saved in /home/dy/.ssh/id_rsa.pub.”

查看储存ssh-key的文件

cat /home/dy/.ssh/id_rsa.pub

复制ssh-key

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDGcCnL9AMDijxeHcE0xETEFGpQwYtlv3fTHA0BdAJbLOLzJDJQT3Re+7llEASLn1bmg0OIDOZNhMkk7T4D2OBRZ9EFiQqEvbSe1NxRFj+Uh93a+cP+K0CudPuJbuRU02Qe+x7VvRxui9FMXXUQsNFgih/AjGHK6NCdKhBeb9IOcH2uXMrJyfZcx6fjAVlkY9Pt8owvEf0mczAMPm8nIxuxqZx53XBlnkqFI7JD978PGdTduIWrS7bSGKJAQMxUeR7gn48MAOFGAow+f1wEKecFY8gN+JHhlkOXODcZpECdJhkdf4qV5BsES/UZ1ULpNE0p4SCWMV7faSVssK6TIbzB 346075646@qq.com
```

### 3、打开Github，进入Settings

点击左边的 `SSH and GPG keys` ，将ssh key粘贴到右边的Key里面。Title随便命名即可。

### 4、测试是否连接成功

执行 `ssh -T git@github.com` ，输入github密码，出现以下代码表示连接成功

Warning: Permanently added the RSA host key for IP address '13.229.188.59' to the list of known hosts.
Hi dy199306! You've successfully authenticated, but GitHub does not provide shell access.

# 2）创建远程仓库并与本地关联

### 1、在github上创建一个远程仓库，复制ssh的克隆地址

### 2、在本地创建一个文件夹，在文件夹内执行git init 初始化一个本地仓库

### 3、在本地仓库执行 git clone ”ssh地址“





# https连接

1、在本地仓库建立一个文件夹，执行git clone https://github.com/dy199306/gittest.git 就可以连接到远端仓库

然后执行git push origin master  指定分支推送到远端服务器 的时候输入github的账号密码即可



```
echo "# project" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:dy199306/project.git
git push -u origin master
```

### …or push an existing repository from the command line

```
git remote add origin git@github.com:dy199306/project.git
git push -u origin master
```

### …or import code from another repository

You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

## 本地操作

1. git init  初始化一个本地仓库。会在文件夹里新建一个.git隐藏文件

2. git status  查看状态

3. git add <filenmae>  把一个文件从工作区添加到暂存区

4. git commit -m <comment>  把暂存区的文件添加到本地仓库 

5. git log 查看提交的详细的历史记录；但是有可能不全

6. git reflog  查看简要的历史记录;完整的记录

7. git diff 查看修改的内容。  - 代表删除的代码  +  代表新增的代码

8. git commit -am <comment> 直接把git add . 和git commit -m 合成了一步

9. git show <commit_id> 显示指定commit_id的修改

10. git reset --hard <commit_id> 回退到指定的版本。

11. git branch  显示当前所有的分支。会存在一个默认分支master

12. git branch <branch_name> 新建一个分支

13. git checkout <branch_name>  切换到指定的分支

14. git merge <branch_name>  把指定分支的代码合并到当前分支。先尝试自动解决冲突

+ git add . 把解决完冲突的代码提交到暂存区
+ git commit -m <comment> 提交到本地仓库

15. git rebase <branch_name> 需要完全手动的解决冲突

+ git add . 把解决完冲突的代码提交到暂存区
+ git rebase --continue 直接就把代码提交到了本地仓库
   + git rebase --abort 可以取消分支的合并

16. git branch -d/D <branch_name> 删除/强制删除 指定分支

17. git config 命令的使用。设置配置项，用来指定用户名和邮箱

+ git config --global 用来设置全局的用户名和邮箱。本质其实就是把配置项写入到一个文件里，这个文件的目录默认是  ~/.gitconfig
     + git config --global user.email <email>  配置用户的邮箱
     + git config --global user.name <name> 配置用户名
+ git config  如果不添加 --global参数，用来指定当前项目的用户名和邮箱。它默认保存在当前项目/.git/config 文件里


## 服务器操作

1. git clone <uri>  使用远端地址，克隆远端服务的工程。只执行一次
2. git push <origin master>   把本地的代码推送到远端服务器
3. git push -u origin master  指定分支推送到远端服务器
4. git pull 从服务器里拉取最新的代码。相当于 git fetch + git merge
5. git fetch 从服务器拿到最新的版本，并不会合并。需要调用git merge进行手动合并。



### 查看本地分支和远程分支的映射关系。

```
git branch -vv
```
1. 创建远程分支

   ```
   方式一:
   # 直接将本地develop分支的内容推送到远端的develop,并且将本地的develop分支和远端建立映射关系
   git push --set-upstream origin develop

   方式二:
   git push origin test:test  # 将本地的test分支推送到远端分支 test
   git branch --set-upstream-to origin/test # 将本地的test分支与与远端的test分支关联
   ```

2. 将远程分支拉取到本地

   ```
   git pull origin develop:develop  # 将远端上的develop分支拉取到本地的develop分支
   ```

3. 删除远端分支

   ```
   git push origin --delete develop  # 将远端分支develop删除
   ```

4. 分支的其他相关指令-

   - git fetch只是拿到远端的分支的最新commit_id，并不会自动合并，需要手动进行合并。

   - ```
     git fetch origin test  # 将远端test的最新commit_id获取到本地
     git merge origin/test test  # 将远端的test的修改内容合并到本地的test分支
     ```

   - git rebase用法和git merge 类似。

     ```
     git rebase 如果在合并时有冲突，需要手动解决冲突
     git add . # 标记为已解决所有冲突
     git rebase --abort  # 中止合并
     git rebase --continue 解决完冲突以后，不再git commit，而是使用git rebase --continue提交修改冲突后的数据
     ```

   # SSH链接

   码云生成部署SSH Key:http://git.mydoc.io/?t=154712

   gitHub生成SSH Key:https://help.github.com/articles/connecting-to-github-with-ssh/