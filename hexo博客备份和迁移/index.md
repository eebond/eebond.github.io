# Hexo博客备份和迁移


## 为什么需要备份和迁移？

一般情况下，我们博客的相关配置信息都是在本地操作的,  但是当我们更换了设备或者电脑出现故障了等，那么我们便无法再维护我们的博客了。因而为了保护我们的劳动成果以及将来能更方便的维护博客，我们需要对博客进行备份和迁移，也就是将博客的相关配置信息上传到github上进行托管。日后有必要的时候可以从github上克隆到本地进行博客的维护等操作。

我在windows10上安装的Hexo博客，如果要换电脑或者换系统就要转移Hexo博客，之后我将Hexo博客转移到Ubuntu20.04上以便我在使用Ubuntu系统时也可以写博客。

## 备份

### 思路

在搭建博客的时候，我们已经将博客部署到了github上去，其实部署上去只是生成的静态文件。因而还需要将hexo生成的网站源文件也push到github上。这个时候需要再github上创建分支，其中主分支master已经存放了生成的静态网页。

以下操作在windows10上进行

### 处理过程

#### 删除.git

将hexo的主题下的.git删除，比如删除themes/next/目录下的.git否则无法将主题文件夹push。

#### 创建.gitignore

在本地blog文件夹下创建文件.gitignore，正常情况这个文件是自动生成的，打开后写入：

```txt
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

这个文件的存在是指在push的时候忽略文件中的文件格式。

#### 创建本地分支

在本地blog文件夹中执行命令

```cmd
#git初始化
git init
#创建hexo分支，用来存放源码
git checkout -b hexo
#git 文件添加
git add .
#git 提交
git commit -m "backup"
#添加远程仓库，github上的博客仓库
git remote add origin 博客仓库地址
#push到hexo分支
git push origin hexo
```

## 迁移

以下操作在Ubuntu系统上进行

### 安装Hexo

#### 1.安装git

```bash
sudo apt-get install git -y
```

#### 2.安装Node.js

安装Node.js的最佳方式是使用nvm.

```bash
curl https://raw.github.com/creationix/nvm/master/install.sh | bash
```

安装完成后，重启终端并执行下列命令即可安装Node.js。

```bash
source ~./profile
nvm install stable
```

安装Hexo

```bash
npm install hexo-cli -g
```

至此，Hexo的安装完成了。

### 从GitHub下载备份并安装

#### 1.配置远程仓库的访问权限
```bash
ssh-keygen -t rsa
```
将生成的公钥添加到GitHub的SSH-Keyd的列表中。

#### 2.下载备份
```bash
mkdir myblog
git clone -b hexo  <远程仓库地址>
```

#### 3.安装
进入myblog目录
```bash
npm install
```

之后就可以正常使用了。
