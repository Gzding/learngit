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
