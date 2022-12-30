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
