# VMware虚拟机中CentOS7系统网络配置


**在VMware虚拟机中安装CentOS 7的mini版本，没有UI界面，需要手动配置网络，才能联网。**

## VMware虚拟网络编辑器设置

在< 编辑 >选项 中打开虚拟网络编辑器：
![ ](/images//Markdown/20211201200533.png)

虚拟网络编辑器如图所示：
![ ](/images/Markdown/20211201200712.png)  

点击右下方的< 更改设置 >选项，然后如图设置
![ ](/images/Markdown/20211201201141.png)
![ ](/images/Markdown/20211201201247.png)  

## CentOS 中配置文件

```shell
ip addr
```

查看网卡名称为ens33：
![ ](/images/Markdown/20211201201512.png)  

更改配置文件/etc/sysconfig/network-scripts/ifcfg-ens33

```txt
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static   #改为static，表示静态IP，IP不会变
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens33
UUID=9a344100-a68f-48bd-8c76-104ca488d1e9
DEVICE=ens33
ONBOOT=yes        #开机自动读取该配置文件
IPADDR=192.168.70.110  ##自己设置的静态IP
NETMASK=255.255.255.0  ##子网掩码
GATEWAY=192.168.70.2   ##网关
DNS1=8.8.8.8           ## DNS
DNS2=8.8.4.4
```

```shell
systemctl restart network
```

让网络配置生效

## 对于本地宿主机的配置

![ ](/images/Markdown/20211201202140.png)  

配置结束虚拟机就可以上网了  

