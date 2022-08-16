# Hugo博客服务器迁移操作记录


## 前言

博客一开始部署在搬瓦工的美西服务器上，但是线路太差，博客访问延迟比较高，现在将博客迁移到DMIT上。

## 迁移  

以下操作在DMIT服务器上操作，由于DMIT服务器的访问特殊性，只能通过密钥或者添加公钥去登录访问服务器（以root身份），创建的普通用户无法通过密码或者公密钥访问（一个大坑，目前还不知道如何解决），所以这次是git仓库建在git用户目录下，通过root身份去访问。

### 新增git用户

```bash
useradd git
```

### 创建git版本库

```bash  
su git && cd
mkdir blog.git && cd blog.git && git init --bare
```  

### 编写钩子文件

```bash
cd /home/git/blog.git/hooks && vim post-receive
```  

添加下面的命令

```txt
git --work-tree=/srv/www/blog checkout -f
```

最后给钩子文件添加执行权限

```bash
chmod +x post-receive
```  

### 创建blog文件目录  

```bash
mkdir /srv/www/blog
```  

修改blog目录的所有者

```bash
chown git:git /srv/www/blog
```  

### 本地博客仓库处理  

