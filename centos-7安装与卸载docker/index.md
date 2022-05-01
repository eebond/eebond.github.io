# CentOS 7安装与卸载docker


### 一、 安装Docker环境

---

#### 1.使用官方安装脚本自动安装

安装命令如下：

```bash
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

也可以使用国内daocloud一键安装命令：

```bash
curl -sSL https://get.daocloud.io/docker | sh
```

#### 2.使用Docker仓库进行安装

- 下载关于Docker的依赖环境

```bash
yum -y install yum-utils device-mapper-persistent-data lvm2
```

- 设置下载Docker的镜像源
  - 使用官方源（国内较慢）

  ```bash
  yum-config-manager  --add-repo https://download.docker.com/linux/centos/docker-ce.repo
  ```

  - 国内的源地址
    - 阿里云

    ```bash
    yum-config-manager  --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
    ```

    - 清华大学源

    ```bash
    yum-config-manager --add-repo https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/centos/docker-ce.repo
    ```

- 安装Docker

```bash
yum makacache fast
yum -y install docker-ce
```

### 二、清理卸载Docker环境

---
1.杀死所有运行容器

```bash
docker kill $(docker ps -a -q)
```

2.删除所有容器

```bash
docker rm $(docker ps -a -q)
```

3.删除所有镜像

```bash
docker rmi $(docker images -q)
```

4.停止docker服务

```bash
systemctl stop docker
```

5.删除存储目录

```bash
rm -rf /etc/docker
rm -rf /run/docker
rm -rf /var/lib/docker
```

6.卸载docker

- 查看已安装的docker包
  
```bash
yum list installed | grep docker
```

- 卸载相关包

```bash
yum remove docker*
```  

