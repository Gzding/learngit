# learngit

learn git 

git clone git@github.com:Gzding/learngit.git





## 本地Git环境配置

### 在Windows安装Git

略

### 参数配置

配置用户名和邮箱

```bash
git config --global user.name "name"
git config --global user.email "email"
```

生成ssh密钥

```bash
ssh-keygen -t rsa -C "your_email@example.com"
```

默认在用户目录下的.ssh目录内生成公钥id_rsa.pub



## 使用GitHub远程仓库

### 密钥设置

注册登录GitHub，在我的设置中添加ssh公钥【公钥就是本地生成的公钥】。

### 新建远程仓库

在GitHub上New即可，设置仓库名和简介，并选择 Add a README file，以及 Add .gitignore文件，这样就创建好了远程仓库。 

### 在本地克隆远程仓库

在GitHub的仓库界面获得clone命令，在本地命令行中输入命令，克隆远程仓库。

然后就能正常使用此仓库了，其中本地和远程的main分支是关联的。





## 搭建Git私有服务

### 在自己的服务器上安装Git

安装Git和ssh

```shell
sudo apt-get install git
sudo apt-get install ssh
```



启动服务

```sh
service ssh start
```



给服务器增加管理Git的用户

```sh
sudo adduser git
# 可以为git用户创建用户组gits
```

此时，生成了用户 git 的用户文件夹：/home/git



创建ssh证书认证文件

```sh
cd /home/git
sudo mkdir /home/git/.ssh
sudo touch /home/git/.ssh/authorized_keys
```

并修改此文件的权限

```sh
sudo chmod 700 /home/git
sudo chmod 700 /home/git/.ssh
sudo chmod 600 /home/git/authorized_keys
sudo chown -R git:git /home/git
sudo chown -R git:git /home/git/.ssh
sudo chown -R git:git /home/git/.ssh/authorized_keys
```



创建仓库路径

```sh
sudo mkdir github # 此路径在/home/git中创建
sudo chown git:git github
```



新建空白仓库

```sh
cd /home/git/github
sudo git init --bare test.git
sudo chown -R git:gits test.git # 用户:用户组
```

此 test.git 仓库的地址就是：git@ip:/home/git/github/test.git





禁止git用户登录服务器

先使用下命令找到git-shell的位置

```sh
which git-shell
# 比如 /usr/local/git/bin/git-shell
```

修改/etc/passwd中的内容

```sh
vim /etc/passwd
```

将 `git:x:1004:1004:,,,:/home/git:/bin/bash` 修改成 `git:x:1004:1004:,,,:/home/git:/usr/local/git/bin/git-shell`，其中的路径就是刚刚查到的。



### 将本地的公钥复制到服务器

将本地生成的公钥复制到服务器上的 /home/git/.ssh/authorized_keys文件中。



### 在服务器上新建空白仓库

新建空白仓库

```sh
cd /home/git/github
sudo git init --bare test.git
sudo chown -R git:gits test.git # 用户:用户组
```

此 test.git 仓库的地址就是：git@ip:/home/git/github/test.git

然后，就可以在本地克隆此仓库，并正常使用了。





## 本地Git仓库操作流程

### 新建本地仓库

新建本地仓库方式有：

- 克隆远程仓库：git clone git@ip:/path/test.git
  - 推荐使用这个，免得后续添加remote的操作了
- 初始化本地文件夹为git仓库：git init

新建仓库后添加的第一个文件建议为：.gitignore

- 此文件中说明了此仓库不追踪哪些文件，也就是这些文件都会被忽略，

### 向仓库添加提交文件

在仓库中，新建文件、修改文件都会更新仓库的状态，

可以使用命令 git status 查看当前仓库的状态，

使用命令 git add filename 存储此文件的更新，

或者使用命令 git add . 存储所有新更新的文件的状态。

当所有已更新的文件都重新add后，就可以使用命令 git commit -m ”update text“ 提交。 

查看历史提交记录，可以使用命令 git log 或 git log --pretty=oneline

### 仓库版本线

每次提交都会使得仓库维护一个版本，每个版本都有一个 id，

使用记录查看命令 git log 或 git log --pretty=oneline 可以查看历史提交记录，

其中的一长串数字就是版本 id，

另外，HEAD 指向当前最新版本。

### 版本的回退

若想回退到以前的版本，可以使用命令 git reset --hard id

记住：此命令执行时最好是在刚刚执行提交命令之后，即没有未添加和提交的更新时。

回退到上一版本：git reset --hard HEAD^

HEAD：当前最新版本，

HEAD^：上一版本，

HEAD^^：上上一版本，

回退版本之后，HEAD指向了上一版本，上一版本就成了此时的最新版本，

但是，之前的最新版本并不会消失，只要记住其版本 id，还能再回到那个版本，

命令是：git reset --hard id



### 核心概念

git有两个区域，分别是 工作区、版本库，

工作区就是 .git 文件夹所在的整个文件夹，但不包括 .git 文件夹，也不包括 .gitignore 文件中声明的所有文件及文件夹，

- 工作区中的文件可以说是当前分支的当前最新版本加上最新的修改

版本库就是 .git 文件夹，其中分两部分：暂存区、分支区

- 暂存区：存储 git add 的所有更新，
  - git commit 时，就会把暂存区中的内容提交到当前分支，形成一个新版本。
- 分支区：存储所有分支的所有版本，



### 更新的撤销

更新有以下几种状态：

- 还没有此更新：对工作区的文件没有修改；
  - git status：nothing to commit, working tree clean

- 已更新未暂存：对工作区中的文件修改了，但没有使用 git add 命令暂存到暂存区；
  - changes not staged for commit: 
  - ​                    modified:      filename

- 已更新并暂存：对工作区中的文件修改了，并且使用 git add 命令暂存到了暂存区；
  - changes to be committed:
  - ​                     modified:      filename

- 已提交：此更新在暂存区时，使用了 git commit 命令，将此更新提交了。



对于上述几种更新状态的撤销方法是：

- 已提交的：
  - 使用版本回退命令 git reset --hard id 回退版本
- 已更新并暂存的：
  - 使用命令 git reset HEAD filename 将文件filename的暂存的更新取消暂存
  - 但此文件的内容并不会变，只是更新的状态取消暂存了，变成了已更新未暂存
- 已更新未暂存的：
  - 使用命令 git checkout -- filename 撤销此文件的新的更新
  - 此时文件的内容就回到了上一个提交后，
  - 注意，此操作不可逆的，因为此更新还未被git记录为一个版本



### 分支

使用命令 git branch 查看分支情况，

其中当前所在分支的名字前有星号*标识，

新建分支：git branch new-branch-name

切换分支：git checkout branch-name

合并某分支到分支：git merge branch-name

删除分支：git branch -d branch-name

### 分支合并时的冲突

当将某分支合并到当前分支时，可能会产生冲突，冲突是git无法自主决定去留的两个分支的不同内容，

在文件中，冲突内容形式为：

- 以 `=======` 分开，
- 上面至 `<<<<<<<< branch-name` 是当前分支的内容，
- 下面至 `>>>>>>>> branch-name` 是某分支的内容，

解决冲突，就是手动决定两个分支的不同内容的去留，

手动解决好所有冲突之后，使用 git add 和 git commit 提交，至此，冲突解决好了。





### 常用命令

