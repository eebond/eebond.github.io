# CentOS7 安装部署Shadowsocks


## 前言

> 之前安装的shadowsocks-libev有点不太好用了，端口直接被封，可能是加密协议的问题。
> 现在，用网上通用的脚本方法安装shadowsocks。

## 安装

[秋水逸冰的一键安装脚本](https://github.com/teddysun/shadowsocks_install)

```bash
wget –no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks.sh && bash ./shadowsocks.sh  
```

直接默认配置就行，后期手动更改配置。

## 修改配置文件

```bash
vim /etc/shadowsocks.json
```

只需要修改服务端口和密码就行了，其他的不懂就不要动。

退出编辑页面，重启shadowsocks

```bash
systemctl restart shadowsocks
```

## 客户端下载使用

[shadowsocks-windows客户端直链](https://github.com/shadowsocks/shadowsocks-windows/releases)  

