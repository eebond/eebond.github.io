# Linux中安装配置JDK8


## 前言

> 学习JAVA需要安装配置jdk环境，太多数时候会在Linux环境中配置jdk，本篇以Centos7为例配置jdk8，以便日后参考，快速配置环境。

--------

## 安装配置

### 下载jdk8

1.通过[oracle官网下载](https://www.oracle.com/java/technologies/downloads/#java8)  

![Alt Text]({{< param cdnPrefix >}}/images/Markdown/20220417201328.png)

但是官网下载，需要Oracle账号。网上找到得账号：  

```txt
1287019365@qq.com
Oracle@1234
```

2.通过wget下载我的私有云盘里的jdk8备份

```bash
wget jdk-8u321-linux-x64.tar.gz https://cloud.eebond.xyz/api/v3/file/get/194/jdk-8u321-linux-x64.tar.gz?sign=bvDVYkfqvYU4jv86no9qpC6Sw7n2opQLA6CtcU_W-EI%3D%3A0
```

### 配置  

1、 解压jdk-8u321-linux-x64.tar.gz到指定目录

```bash
mkdir /usr/java && tar -zxvf jdk-8u321-linux-x64.tar.gz -C /usr/java/
```

2、 配置环境变量

打开配置文件  

```bash
vim /etc/profile
```

编写配置文件内容

```txt
export JAVA_HOME=/usr/java/jdk1.8.0_321
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```  

使更新后的配置文件生效

```bash
source /etc/profile
```  

### 验证使用jdk

```bash
java -version 
javac
```
![Alt Text]({{< param cdnPrefix >}}/images/Markdown/20220417203744.png)
