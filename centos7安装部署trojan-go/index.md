# CentOS7安装部署Trojan-go


## 前言

> Trojan-goTrojan-Go是使用Go语言实现的完整的Trojan代理，和Trojan协议以及原版的配置文件格式兼容。支持并且兼容Trojan-GFW版本的绝大多数功能，并扩展了更多的实用功能。

> Trojan-Go的的首要目标是保障传输安全性和隐蔽性。在此前提下，尽可能提升传输性能和易用性。

> Trojan-Go[官方文档](https://p4gefau1t.github.io/trojan-go/),可以详细阅读，了解trojan的原理。

> Trojan-Go和V2ray + WS + TLS一样是目前比较稳定的科学上网方式，shadowsocks已经能被墙识别，搬瓦工秒被封端口。

## 安装Trojan-Go

### 申请TLS证书

```bash
certbot certonly --standalone -d trojan.eebond.xyz
```

> 如果当前有nginx正在运行，先关闭nginx。

查看证书是否安装成功

```bash
certbot certificates
```  

证书安装路径

```bash
Certificate Path: /etc/letsencrypt/live/trojan.eebond.xyz/fullchain.pem
Private Key Path: /etc/letsencrypt/live/trojan.eebond.xyz/privkey.pem
```  

### 安装配置Nginx

```bash
yum install nginx -y
```

网站配置文件

```bash
vim /etc/nginx/conf.d/trojan.conf
```

文件内容

```txt
server {
        listen       1239;
        root         /srv/www/blog;

    }

    server {
        listen       1001;
        root         /srv/www/blog;
    }
```  

### 安装配置trojan-go

#### 下载安装trojan-go

> 在 <https://github.com/p4gefau1t/trojan-go/releases> 查看下载链接，下载解压至 /usr/local/trojan-go目录  

```bash
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.10.6/trojan-go-linux-amd64.zip  
```  

#### 配置trojan-go

将example目录下的 server.json 复制到/usr/local/trojan-go目录，修改为如下内容

```txt
{
    "run_type": "server",
    "local_addr": "0.0.0.0",
    "local_port": 447,
    "remote_addr": "127.0.0.1",
    "remote_port": 1001,
    "password": [
        "5f45307f-a867-05de-3913-ff539f92325f"
    ],
    "ssl": {
        "verify": true,
        "verify_hostname": true,
        "cert": "/etc/letsencrypt/live/trojan.eebond.xyz/fullchain.pem",
        "key": "/etc/letsencrypt/live/trojan.eebond.xyz/privkey.pem",
        "sni": "trojan.eebond.xyz",
	"fallback_addr": "127.0.0.1",
	"fallback_port": 1239
    },
    "router": {
        "enabled": true,
        "block": [
            "geoip:private"
        ],
        "geoip": "/usr/local/trojan-go/geoip.dat",
        "geosite": "/usr/local/trojan-go/geosite.dat"
    }
}
```  

#### 创建systemctl启动文件

1.复制启动文件至系统服务目录

```bash
cp /usr/local/trojan-go/example/trojan-go.service /usr/lib/systemd/system/
```

2.修改启动文件，

```txt
[Unit]
Description=Trojan-Go - An unidentifiable mechanism that helps you bypass GFW
Documentation=https://p4gefau1t.github.io/trojan-go/
After=network.target nss-lookup.target

[Service]
User=root
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true
ExecStart=/usr/local/trojan-go/trojan-go -config /usr/local/trojan-go/server.json
Restart=on-failure
RestartSec=10s
LimitNOFILE=infinity

[Install]
WantedBy=multi-user.target
```

> 注意修改User和ExecStart，否则无法自启动

3.启动服务&设置开机启动  

```bash
systemctl daemon-reload
systemctl start trojan-go
systemctl enable trojan-go
```  

之后就可以在客户端快乐的使用了！  

