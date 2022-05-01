# V2ray+WS+TLS手动配置教程

### 准备工作

如果是新的vps，需要安装必要的工具：

```bash
yum update
yum install -y vim wget curl
```

需要将系统时间改为东八区，即上海时间：

```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 一、V2ray官方一键安装脚本

#### 1.获取V2ray官方脚本

```bash
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh
```

#### 2.执行V2ray官方脚本

```bash
bash install-release.sh
```

#### 3.安装文件分析

- /usr/local/bin/v2ray：V2Ray 程序；
- /usr/local/bin/v2ctl：V2Ray 工具；
- /usr/local/etc/v2ray/config.json：配置文件；
- /usr/local/share/v2ray/geoip.dat：IP 数据文件
- /usr/local/share/v2ray/geosite.dat：域名数据文件

#### 4.安装最新的geoip.dat 和 geosite.dat

```bash
wget https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh && bash install-dat-release.sh
```

#### 5.移除V2ray

```bash
bash install-release.sh --remove
```

### 二、V2ray配置文件

#### 直接获取配置文件

```bash
cd /usr/local/etc/v2ray && wget -O config.json https://raw.githubusercontent.com/eebond/banwagong/main/V2rayN/config.json
```

#### 手动编写配置文件并粘贴配置文件

```bash
vim /usr/local/etc/v2ray/config.json
```

#### 配置文件内容

```txt
{
  "log": {
        "access": "/var/log/v2ray/access.log",
        "error": "/var/log/v2ray/error.log",
        "loglevel": "warning"
  },
  "inbounds": [
   {
    "listen": "127.0.0.1",
    "port": 10086,
    "protocol": "vmess",
    "settings": {
      "clients": [
        {
          "id": "2e82cf13-4e82-4935-9ec7-23d0e0eb56b3",
          "level": 1,
          "alterId": 0 
        }
      ]
    },
    "streamSettings": {
        "network": "ws",
        "security": "none",
        "wsSettings": {
          "path": "/vmess"
        }
    }
   },
   {
    "listen": "127.0.0.1",
    "port": 12345,
    "protocol": "vless",
    "settings": {
      "decryption": "none",
      "clients": [
        {
          "id": "b123c75b-ebdf-006a-eeea-bcf6a8242e42",
          "level": 0
        }
      ]
    },
    "streamSettings": {
      "network":"ws",
      "security": "none",
      "wsSettings":{
        "path":"/vless",
        "headers":{}
      }
    }
   },
   {
    "port": 51888,
     "protocol": "shadowsocks",
     "settings": {
        "method": "aes-256-gcm",
        "password": "www.bannedbook.org",
        "network": "tcp,udp",
        "level": 0
      }
   }
  ],
  "outbounds": [{
    "protocol": "freedom",
    "settings": {}
  },{
    "protocol": "blackhole",
    "settings": {},
    "tag": "blocked"
  }],
  "routing": {
    "rules": [
      {
        "type": "field",
        "ip": ["geoip:private"],
        "outboundTag": "blocked"
      }
    ]
  }
}
```

#### V2ctl生成uuid

```bash
v2ctl uuid
```

### 三、运行V2ray服务端

#### 启动V2ray服务

```bash
systemctl start v2ray
```

#### 设置开机自启动

```bash
systemctl enable v2ray
```

#### 测试V2ray配置文件正确性

```bash
v2ray -test -config /usr/local/etc/v2ray/config.json
```

#### 控制 V2Ray 的运行的常用命令

```bash
service v2ray restart | force-reload |start|stop|status|reload
```

> TIPS  
> 此时已经可以通过V2ray实现科学上网了。

### 四、Nginx反向代理+TLS

#### Nginx+TLS配置教程

<https://eebond.github.io/centos7/centos-xia-shi-yong-certbot-shen-qing-bu-shu-let-s-encrpyt-mian-fei-ssl-zheng-shu-ngingx-fu-wu-qi/>

#### Nginx配置文件

##### 快速获取

```bash
wget -O /etc/nginx/conf.d/cloud.conf  https://raw.githubusercontent.com/eebond/banwagong/main/Nginx/conf.d/blog.conf
```

##### 手动输入

```bash
vim /etc/nginx/conf.d/blog.conf
```

##### 配置文件

```txt
    server {
        listen       80;
        listen       [::]:80;
        server_name  blog.eebond.xyz;
        rewrite ^(.*)$ https://$host$1 permanent;
    }

# Settings for a TLS enabled server.

    server {
        listen       443 ssl http2;
        listen       [::]:443 ssl http2;
        server_name  blog.eebond.xyz;
        root         /srv/www/blog;

        ssl_certificate "/etc/letsencrypt/live/blog.eebond.xyz/fullchain.pem";
        ssl_certificate_key "/etc/letsencrypt/live/blog.eebond.xyz/privkey.pem";
        ssl_session_cache shared:SSL:1m;
        ssl_session_timeout  10m;
        ssl_ciphers HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }

        location /vmess {
            if ($http_upgrade != "websocket") {
            return 404;
            }
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10086;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;

            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        location /vless {
            if ($http_upgrade != "websocket") {
                return 404;
            }
            proxy_redirect off;
            proxy_pass http://127.0.0.1:12345;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;

            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        
    }
```

