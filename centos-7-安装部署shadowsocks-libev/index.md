# CentOS 7 安装部署Shadowsocks-libev


参考文章：<https://gfw.report/blog/ss_tutorial/zh/>

### 一、安装snap应用商店

#### 1.安装EPEL

```bash
yum install epel-release -y
```

#### 2.安装snapd

```bash
yum install snapd -y
```

#### 3.添加snap启动通信socket

```bash
systemctl enable --now snapd.socket
```

#### 4.创建链接（snap软件包一般安装在/snap目录·下）

```bash
ln -s /var/lib/snapd/snap /snap
```

#### 5.安装snap core(更新)

```bash
snap snap install core
```

### 二、安装Shadowsocks-libev

#### 1.安装最新版的Shadowsocks-libev

```bash
snap install shadowsocks-libev --edge
```

#### 2.编写配置文件

##### 快速获取配置文件

```bash
cd /var/snap/shadowsocks-libev/common/etc/shadowsocks-libev/ &&  wget -O config.json https://raw.githubusercontent.com/eebond/banwagong/main/shadowsocks-libev/config.json
```

##### 配置文件位置

```bash
/var/snap/shadowsocks-libev/common/etc/shadowsocks-libev/config.json
```

##### 配置文件内容

```txt
{
    "server":["::0","0.0.0.0"],
    "server_port":8389,
    "encryption_method":"chacha20-ietf-poly1305",
    "password":"ks5g+uP4eYBto8rxfy5gJg==",
    "mode":"tcp_and_udp",
    "fast_open":false
}
```

##### 生成强密码

```bash
openssl rand -base64 16
```

### 三、运行Shadowsocks-libev

#### 启动Shadowsocks-libev

```bash
systemctl start snap.shadowsocks-libev.ss-server-daemon.service
```

#### 设置开机自启动

```bash
systemctl enable snap.shadowsocks-libev.ss-server-daemon.service
```

### 四、维护

#### 检查运行状态和日志

```bash
systemctl status snap.shadowsocks-libev.ss-server-daemon.service
```

#### 重新加载配置文件

```bash
systemctl restart snap.shadowsocks-libev.ss-server-daemon.service
```

#### 配置备用端口来缓解端口封锁

使用以下命令来将服务器从12000到12010端口接收到的TCP和UDP流量全部转发到8389端口：

```bash
iptables -t nat -A PREROUTING -p tcp --dport 12000:12010 -j REDIRECT --to-port 8389
iptables -t nat -A PREROUTING -p udp --dport 12000:12010 -j REDIRECT --to-port 8389
```

输入一下命令，查看是否配置成功

```bash
iptables -t nat -L PREROUTING -nv --line-number
```  

