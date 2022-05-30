# vps上挂载阿里云盘并用aria2实现离线下载


## vps挂载阿里云盘并用aria2实现离线下载

之前用cloudreve搭建了一个私人网盘，性能可以，但是vps的空间太小，无法存储很多文件。该网盘还有离线下载功能，需要用aria2来实现。所以我想把私人网盘的存储放在阿里云盘里，并且通过aria2实现两个网盘的离线下载。

### 首先获得阿里云盘 refreshToken

登录阿里云网页版，点击F12

![ ](https://fastly.jsdelivr.net/gh/eebond/images/Markdown/20220330162759.png)

### 让阿里云盘实现WebDAV功能

使用开源项目[aliyundrive-WebDAV](https://github.com/messense/aliyundrive-WebDAV)来实现功能。

以CentOS7为例：

```bash
yum install python3-pip -y

pip3 install aliyundrive-WebDAV
```

升级项目：

```bash
pip install --upgrade aliyundrive-WebDAV
```

后台启动命令实现WebDAV功能

```bash
nohup aliyundrive-webdav -I -U 用户名 -W 密码 -r 你的token &
```

### 挂载使用阿里云盘

Rclone 是一个用于在多平台进行文件同步的命令行工具，支持多家网盘及文件传输协议。这里主要介绍 Linux 端挂载 WebDAV 的使用方式：

使用官方脚本安装最新 rclone：

```bash
curl https://rclone.org/install.sh | sudo bash
```

接下来使用 rclone 挂载 WebDAV，输入后回车，一步一步跟着 rclone 的提示来即可，下方#号后面的内容为步骤翻译注释，不要全部复制粘贴进去了，仔细看提示根据自己的实际情况来修改，输入命令 rclone config 开始配置：

```bash
[root@eebond ~]# rclone config
2022/03/30 16:46:46 NOTICE: Config file "/root/.config/rclone/rclone.conf" not found - using defaults
No remotes found, make a new one?
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> aliyun
Option Storage.
Type of storage to configure.
Choose a number from below, or type in your own value.
 1 / 1Fichier
   \ (fichier)
 2 / Akamai NetStorage
   \ (netstorage)
 3 / Alias for an existing remote
   \ (alias)
 4 / Amazon Drive
   \ (amazon cloud drive)
 5 / Amazon S3 Compliant Storage Providers including AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Lyve Cloud, Minio, RackCorp, SeaweedFS, and Tencent COS
   \ (s3)
 6 / Backblaze B2
   \ (b2)
 7 / Better checksums for other remotes
   \ (hasher)
 8 / Box
   \ (box)
 9 / Cache a remote
   \ (cache)
10 / Citrix Sharefile
   \ (sharefile)
11 / Compress a remote
   \ (compress)
12 / Dropbox
   \ (dropbox)
13 / Encrypt/Decrypt a remote
   \ (crypt)
14 / Enterprise File Fabric
   \ (filefabric)
15 / FTP Connection
   \ (ftp)
16 / Google Cloud Storage (this is not Google Drive)
   \ (google cloud storage)
17 / Google Drive
   \ (drive)
18 / Google Photos
   \ (google photos)
19 / Hadoop distributed file system
   \ (hdfs)
20 / Hubic
   \ (hubic)
21 / In memory object storage system.
   \ (memory)
22 / Jottacloud
   \ (jottacloud)
23 / Koofr, Digi Storage and other Koofr-compatible storage providers
   \ (koofr)
24 / Local Disk
   \ (local)
25 / Mail.ru Cloud
   \ (mailru)
26 / Mega
   \ (mega)
27 / Microsoft Azure Blob Storage
   \ (azureblob)
28 / Microsoft OneDrive
   \ (onedrive)
29 / OpenDrive
   \ (opendrive)
30 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ (swift)
31 / Pcloud
   \ (pcloud)
32 / Put.io
   \ (putio)
33 / QingCloud Object Storage
   \ (qingstor)
34 / SSH/SFTP Connection
   \ (sftp)
35 / Sia Decentralized Cloud
   \ (sia)
36 / Storj Decentralized Cloud Storage
   \ (storj)
37 / Sugarsync
   \ (sugarsync)
38 / Transparently chunk/split large files
   \ (chunker)
39 / Union merges the contents of several upstream fs
   \ (union)
40 / Uptobox
   \ (uptobox)
41 / Webdav
   \ (webdav)
42 / Yandex Disk
   \ (yandex)
43 / Zoho
   \ (zoho)
44 / http Connection
   \ (http)
45 / premiumize.me
   \ (premiumizeme)
46 / seafile
   \ (seafile)
Storage> 41
Option url.
URL of http host to connect to.
E.g. https://example.com.
Enter a value.
url> http://127.0.0.1:8080
Option vendor.
Name of the Webdav site/service/software you are using.
Choose a number from below, or type in your own value.
Press Enter to leave empty.
 1 / Nextcloud
   \ (nextcloud)
 2 / Owncloud
   \ (owncloud)
 3 / Sharepoint Online, authenticated by Microsoft account
   \ (sharepoint)
 4 / Sharepoint with NTLM authentication, usually self-hosted or on-premises
   \ (sharepoint-ntlm)
 5 / Other site/service or software
   \ (other)
vendor> 5
Option user.
User name.
In case NTLM authentication is used, the username should be in the format 'Domain\User'.
Enter a value. Press Enter to leave empty.
user> eebond
Option pass.
Password.
Choose an alternative below. Press Enter for the default (n).
y) Yes, type in my own password
g) Generate random password
n) No, leave this optional password blank (default)
y/g/n> y
Enter the password:
password:
Confirm the password:
password:
Option bearer_token.
Bearer token instead of user/pass (e.g. a Macaroon).
Enter a value. Press Enter to leave empty.
bearer_token> 
Edit advanced config?
y) Yes
n) No (default)
y/n> 
--------------------
[aliyun]
type = webdav
url = http://127.0.0.1:8080
vendor = other
user = eebond
pass = *** ENCRYPTED ***
--------------------
y) Yes this is OK (default)
e) Edit this remote
d) Delete this remote
y/e/d> 
Current remotes:

Name                 Type
====                 ====
aliyun               webdav

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q
```

创建挂载目录

```bash
mkdir aliyun
```

安装fuse

```bash
yum install fuse
```

后台挂载

```bash
nohup rclone mount aliyun:/ /root/aliyun --cache-dir /tmp --allow-other --vfs-cache-mode writes --allow-non-empty &
```

### 安装Aria2

下载脚本

```bash
wget -N git.io/aria2.sh && chmod +x aria2.sh
```

运行脚本安装

```bash
./aria2.sh
```

安装成功后修改配置文件

```bash
vim /root/.aria2c/aria2.conf
# 下载完成后执行的命令
on-download-complete=/root/.aria2c/upload.sh


vim /root/.aria2c/script.conf
# 网盘名称(RCLONE 配置时填写的 name)
drive-name=aliyun

```

之后在[aria2-for-chrome](https://chrome.google.com/webstore/detail/aria2-for-chrome/mpkodccbngfoacfalldjimigbofkhgjn?hl=zh-CN)上使用。

### 私人云盘将文件存储放在阿里云盘中

```bash
ln -s /root/aliyun/uploads/ uploads
```  

