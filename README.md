# GitFlow
![](https://files.mdnice.com/user/16877/5eeedb53-fdfa-4944-9aae-9706dd414d1a.png)
## 准备工作

**项目经理：**

![](https://files.mdnice.com/user/16877/e8c0fbef-0ee2-4734-8a17-18d88b70f483.png)

1. 在远程服务器执行git init --bare 初始化远程仓库（如github新建仓库）

2. 项目经理在自己的电脑，git clone 仓库 

3. 此时默认有main分支，添加初始化代码，

   （如新建一些基础文件，如index.js 先给个空文件）

   - git add .
   - git commit -m "feat: 项目经理初始化项目"
   - git push

4. 打标记，git tag -a v1.0.0 -m "feat: 项目经理初始化项目tag"

5. 新建一个dev分支

   - git branch dev
   - git switch dev
     - 这里步骤可不做
       - 修改index.js,添加一个console.log('123')
       - git add . 
       - git commit -m "feat: 新增入口"
   - git push --set-upstream origin dev  (远程没有dev分支，添加dev分支)

此时的分支
![](https://files.mdnice.com/user/16877/42325b2b-6d8e-4ba4-9e8c-08b483415cab.png)



## 开发阶段

![](https://files.mdnice.com/user/16877/b2dcbdb6-b80f-45d0-9bfe-8ff38b8a7f8e.png)


**开发人员A：**（比如负责项目首页home）

1. git clone 仓库 （git clone git@github.com:ywymoshi/git-flow-test.git）

2. 切换至项目目录， cd git-flow-test

3.  (可选，添加过了，就不要添加了) git config user.name 'ywy' ,   git config user.email  'ywy' 

4. git branch 查看本地分支，默认只有main分支

5. 基于dev创建一个分支feature/home, 

   - git branch -r 查看远程分支，有main和dev

   - git switch dev （本地没有创建dev直接switch, 因为远程已经有了dev分支，相当于将远程dev分支clone到了本地dev分支）
   - git branch feature/home
   - git switch  feature/home

6. 编写代码一些代码

   - git add .
   - git commit -m "feat： 开发人员A完成首页开发"
   - git push --set-upstream origin feature/home  (--set-upstream第一次远程没有feature/home分支，添加feature/home分支)
   - 此时远程服务器就多了feature/home分支

   

**开发人员B** （比如负责项目登录页）

1. git clone 仓库 （git clone git@github.com:ywymoshi/git-flow-test.git）

2. 切换至项目目录， cd git-flow-test

3. (可选，添加过了，就不要添加了) git config user.name 'B' ,   git config user.email  'B@qq.com' 

4. git branch 查看本地分支，默认只有main分支

5. 基于dev创建一个分支feature/login, 

   - git branch -r 查看远程分支，有main和dev， A的origin/feature/home

   - git switch dev （本地没有创建dev直接switch, 因为远程已经有了dev分支，相当于将远程dev分支clone到了本地dev分支）
   - git branch feature/login
   - git switch  feature/login

6. 编写代码一些代码

   - git add .
   - git commit -m "feat： 开发人员B完成登录页开发"
   - git push --set-upstream origin feature/login  (--set-upstream第一次远程没有feature/login分支，添加feature/login分支)
   - 此时远程服务器就多了feature/login分支



**项目经理：**

1. 打开自己的电脑 切换到项目  cd git-flow-test

2. git pull 更新代码

3. git branch -r 查看远程分支（此时就有了feature/home分支和 origin/feature/login分支）

4. 合并feature/home分支

   1. git switch feature/home检查代码没有问题后
   2. git switch dev 切换至dev分支
   3. git merge feature/home合并分支

5. 合并feature/login分支

   1. git switch feature/login检查代码没有问题后
   2. git switch dev 切换至dev分支
   3. git merge feature/login合并分支
   

![](https://files.mdnice.com/user/16877/8c7f033b-277c-49ac-bdc2-4d5faf93dd9e.png)



## 准备上线阶段
![](https://files.mdnice.com/user/16877/1f0e3432-0731-4d82-8079-8bf6b8e442e3.png)



**项目经理：**

1.  打开自己的电脑 切换到项目  cd git-flow-test
2. git switch dev在dev分支中新建release分支， git branch release/v1.0.0
3. 提交release/v1.0.0到远程服务器 
   1. git switch release/v1.0.0
   2. git push --set-upstream origin release/v1.0.0
4. 测试人员在 release/v1.0.0上进行测试
5. 如果此时发现开发人员A的代码有bug, 提了一个issue名称为 issue32

开发人员A:

1. git pull 更新代码
2. 基于release/v1.0.0新建bugfix分支 
   1. git switch release/v1.0.0 
   2. git branch bugfix/issue32   (issue32   就对应issue的名称)
   3. git switch bugfix/issue32
   4. 开始修复bug
      1. git status 查看当前状态
      2. git add .
      3. git commit -m "bug： A开发人员修复了issue32"
   5. 将修复后的代码，合并到release/v1.0.0 中
      1. git switch release/v1.0.0 
      2. git merge bugfix/issue32
      3. git status
      4. git push
3. 修复完成后，测试人员继续测试，如果有问题，重复上面的步骤，
4. 如果没问题了，项目经理将release/v1.0.0合并到dev，将dev合并到main

**项目经理：**

1. 打开自己的电脑 切换到项目  cd git-flow-test
2. git pull
3. 合并release/v1.0.0到dev
   1. git switch dev
   2. git merge release/v1.0.0
4. 合并dev到main
   1. git switch main
   2. git merge release/v1.0.0   或者 （git merge dev） 此时两个版本是一致的

![](https://files.mdnice.com/user/16877/4ee29ca0-d0ac-4e9e-9932-e6fa0e43e89a.png)


## 上线阶段
![](https://files.mdnice.com/user/16877/02f2672c-e1e7-4075-960f-4c2de83f2f5d.png)



**项目经理：**

1. 打开自己的电脑 切换到项目  cd git-flow-test
2. git switch main
3. git tag -a v1.0.1 -m "项目经理打tag - 项目第一次上线"    打标记 
4. git push origin v1.0.1 提交tag到线上
5. git push



## 上线后紧急bug

![](https://files.mdnice.com/user/16877/62c9779f-9f1f-4ad6-bff3-6863108d1453.png)


发布后分为出现普通bug和紧急bug,

普通bug可以等待下一个版本修复，紧急bug需要立即修复。

**修复紧急bug**   （注意这里的角色可能不是项目经理，对应的开发人员）

**项目经理：**

1. 打开自己的电脑 切换到项目  cd git-flow-test
2. git switch main
3.  git branch hotfix/issue66     基于main新建hotfix分支
4. git switch hotfix/issue66 
5. 修复代码（比如是login页面有问题）
   1. 修改login页面代码
   2. git add .
   3. git commit -m "bug： 项目经理修复了登录页的bug"
   4. 合并到dev
      1. git switch dev
      2. git merge hotfix/issue66
   5. 合并到main
      1. git switch main
      2. git merge hotfix/issue66
   6. 在main中打tag
      1. git tag -a v1.1.0 -m "项目经理：修复了issue66的bug"
      2. 同步到远程仓库  git push origin v1.1.0
      
   7. 删除hotfix分支 

![](https://files.mdnice.com/user/16877/864d8701-7d99-4d77-8b27-b90fa92f043a.png)

## 上线后继续迭代

此时dev开发分支是最新的代码，只需要再基于dev分支创建新的feature/功能分支进行开发，重复前面的流程。

# 注意点及技巧

**注意点**

1. 不能将不能运行的代码提交到本地和远程服务器

2. 如果服务器上有其它开发人员的更新内容，那么我们不能直接通过，push将我们的代码提交到服务器

   如果服务器上有其他开发人员更新的内容，我们必须先将其他开发人员更新的内容更新到本地之后，才能通过push提交我们的内容

3. 如果我们更新的内容和其他同事更新的内容有冲突修改了同一个文件的同一行代码，这个时候需要我们自己手动修改冲突，修改完冲突之后才能将代码提交到远程服务器

**开发技巧：**

​	只要开发完一个功能就要立即提交代码

# 功能

**查看所有分支：**

git branch -r



**合并分支（merge）：**

例： 当前在main分支中使用 git merge dev ,代表需要将dev分支中的代码合并到main



**删除分支：**

git branch -d 分支名称    ， 删除指定分支。

例： 当前在main分支中使用 git branch -d  dev

注意： 只是删除本地的分支，并不会删除github服务器上的分支



**删除远程分支** （用的少）：

git push origin --delete dev



等等等。。。。