# wget命令使用教程


## wget命令详解

- 导读： wget是Linux中的一个下载文件的工具，wget是在Linux下开发的开放源代码的软件，作者是Hrvoje Niksic，后来被移植到包括Windows在内的各个平台上。

- 它用在命令行下。对于Linux用户是必不可少的工具，尤其对于网络管理员，经常要下载一些软件或从远程服务器恢复备份到本地服务器。如果我们使用虚拟主机，处理这样的事务我们只能先从远程服务器下载到我们电脑磁盘，然后再用ftp工具上传到服务器。这样既浪费时间又浪费精力，那不没办法的事。而到了Linux VPS，它则可以直接下载到服务器而不用经过上传这一步。wget工具体积小但功能完善，它支持断点下载功能，同时支持FTP和HTTP下载方式，支持代理服务器和设置起来方便简单。下面我们以实例的形式说明怎么使用wget。

### 1.安装wget

```bash
yum install -y wget
```

查看帮助手册

```bash
[root@network test]# wget --help
GNU Wget 1.14，非交互式的网络文件下载工具。
用法： wget [选项]... [URL]...

长选项所必须的参数在使用短选项时也是必须的。

# 1.启动：
  -V,  --version           显示 Wget 的版本信息并退出。
  -h,  --help              打印此帮助。
  -b,  --background        启动后转入后台。
  -e,  --execute=COMMAND   运行一个“.wgetrc”风格的命令。

# 2.日志和输入文件：
  -o,  --output-file=FILE    将日志信息写入 FILE。
  -a,  --append-output=FILE  将信息添加至 FILE。
  -d,  --debug               打印大量调试信息。
  -q,  --quiet               安静模式 (无信息输出)。
  -v,  --verbose             详尽的输出 (此为默认值)。
  -nv, --no-verbose          关闭详尽输出，但不进入安静模式。
       --report-speed=TYPE   Output bandwidth as TYPE.  TYPE can be bits.
  -i,  --input-file=FILE     下载本地或外部 FILE 中的 URLs。
  -F,  --force-html          把输入文件当成 HTML 文件。
  -B,  --base=URL            解析与 URL 相关的
                             HTML 输入文件 (由 -i -F 选项指定)。
       --config=FILE         Specify config file to use.

# 3.下载：
  -t,  --tries=NUMBER            设置重试次数为 NUMBER (0 代表无限制)。
       --retry-connrefused       即使拒绝连接也是重试。
  -O,  --output-document=FILE    将文档写入 FILE。
  -nc, --no-clobber              skip downloads that would download to
                                 existing files (overwriting them).
  -c,  --continue                断点续传下载文件。
       --progress=TYPE           选择进度条类型。
  -N,  --timestamping            只获取比本地文件新的文件。
  --no-use-server-timestamps     不用服务器上的时间戳来设置本地文件。
  -S,  --server-response         打印服务器响应。
       --spider                  不下载任何文件。
  -T,  --timeout=SECONDS         将所有超时设为 SECONDS 秒。
       --dns-timeout=SECS        设置 DNS 查寻超时为 SECS 秒。
       --connect-timeout=SECS    设置连接超时为 SECS 秒。
       --read-timeout=SECS       设置读取超时为 SECS 秒。
  -w,  --wait=SECONDS            等待间隔为 SECONDS 秒。
       --waitretry=SECONDS       在获取文件的重试期间等待 1..SECONDS 秒。
       --random-wait             获取多个文件时，每次随机等待间隔
                                 0.5*WAIT...1.5*WAIT 秒。
       --no-proxy                禁止使用代理。
  -Q,  --quota=NUMBER            设置获取配额为 NUMBER 字节。
       --bind-address=ADDRESS    绑定至本地主机上的 ADDRESS (主机名或是 IP)。
       --limit-rate=RATE         限制下载速率为 RATE。
       --no-dns-cache            关闭 DNS 查寻缓存。
       --restrict-file-names=OS  限定文件名中的字符为 OS 允许的字符。
       --ignore-case             匹配文件/目录时忽略大小写。
  -4,  --inet4-only              仅连接至 IPv4 地址。
  -6,  --inet6-only              仅连接至 IPv6 地址。
       --prefer-family=FAMILY    首先连接至指定协议的地址
                                 FAMILY 为 IPv6，IPv4 或是 none。
       --user=USER               将 ftp 和 http 的用户名均设置为 USER。
       --password=PASS           将 ftp 和 http 的密码均设置为 PASS。
       --ask-password            提示输入密码。
       --no-iri                  关闭 IRI 支持。
       --local-encoding=ENC      IRI (国际化资源标识符) 使用 ENC 作为本地编码。
       --remote-encoding=ENC     使用 ENC 作为默认远程编码。
       --unlink                  remove file before clobber.

# 4.目录：
  -nd, --no-directories           不创建目录。
  -x,  --force-directories        强制创建目录。
  -nH, --no-host-directories      不要创建主目录。
       --protocol-directories     在目录中使用协议名称。
  -P,  --directory-prefix=PREFIX  以 PREFIX/... 保存文件
       --cut-dirs=NUMBER          忽略远程目录中 NUMBER 个目录层。

# 5.HTTP 选项：
       --http-user=USER        设置 http 用户名为 USER。
       --http-password=PASS    设置 http 密码为 PASS。
       --no-cache              不在服务器上缓存数据。
       --default-page=NAME     改变默认页
                               (默认页通常是“index.html”)。
  -E,  --adjust-extension      以合适的扩展名保存 HTML/CSS 文档。
       --ignore-length         忽略头部的‘Content-Length’区域。
       --header=STRING         在头部插入 STRING。
       --max-redirect          每页所允许的最大重定向。
       --proxy-user=USER       使用 USER 作为代理用户名。
       --proxy-password=PASS   使用 PASS 作为代理密码。
       --referer=URL           在 HTTP 请求头包含‘Referer: URL’。
       --save-headers          将 HTTP 头保存至文件。
  -U,  --user-agent=AGENT      标识为 AGENT 而不是 Wget/VERSION。
       --no-http-keep-alive    禁用 HTTP keep-alive (永久连接)。
       --no-cookies            不使用 cookies。
       --load-cookies=FILE     会话开始前从 FILE 中载入 cookies。
       --save-cookies=FILE     会话结束后保存 cookies 至 FILE。
       --keep-session-cookies  载入并保存会话 (非永久) cookies。
       --post-data=STRING      使用 POST 方式；把 STRING 作为数据发送。
       --post-file=FILE        使用 POST 方式；发送 FILE 内容。
       --content-disposition   当选中本地文件名时
                               允许 Content-Disposition 头部 (尚在实验)。
       --content-on-error      output the received content on server errors.
       --auth-no-challenge     发送不含服务器询问的首次等待
                               的基本 HTTP 验证信息。

# 6.HTTPS (SSL/TLS) 选项：
       --secure-protocol=PR     choose secure protocol, one of auto, SSLv2,
                                SSLv3, TLSv1, TLSv1_1 and TLSv1_2.
       --no-check-certificate   不要验证服务器的证书。
       --certificate=FILE       客户端证书文件。
       --certificate-type=TYPE  客户端证书类型，PEM 或 DER。
       --private-key=FILE       私钥文件。
       --private-key-type=TYPE  私钥文件类型，PEM 或 DER。
       --ca-certificate=FILE    带有一组 CA 认证的文件。
       --ca-directory=DIR       保存 CA 认证的哈希列表的目录。
       --random-file=FILE       带有生成 SSL PRNG 的随机数据的文件。
       --egd-file=FILE          用于命名带有随机数据的 EGD 套接字的文件。

# 7.FTP 选项：
       --ftp-user=USER         设置 ftp 用户名为 USER。
       --ftp-password=PASS     设置 ftp 密码为 PASS。
       --no-remove-listing     不要删除‘.listing’文件。
       --no-glob               不在 FTP 文件名中使用通配符展开。
       --no-passive-ftp        禁用“passive”传输模式。
       --preserve-permissions  保留远程文件的权限。
       --retr-symlinks         递归目录时，获取链接的文件 (而非目录)。

# 8.WARC options:
       --warc-file=FILENAME      save request/response data to a .warc.gz file.
       --warc-header=STRING      insert STRING into the warcinfo record.
       --warc-max-size=NUMBER    set maximum size of WARC files to NUMBER.
       --warc-cdx                write CDX index files.
       --warc-dedup=FILENAME     do not store records listed in this CDX file.
       --no-warc-compression     do not compress WARC files with GZIP.
       --no-warc-digests         do not calculate SHA1 digests.
       --no-warc-keep-log        do not store the log file in a WARC record.
       --warc-tempdir=DIRECTORY  location for temporary files created by the
                                 WARC writer.

# 9.递归下载：
  -r,  --recursive          指定递归下载。
  -l,  --level=NUMBER       最大递归深度 (inf 或 0 代表无限制，即全部下载)。
       --delete-after       下载完成后删除本地文件。
  -k,  --convert-links      让下载得到的 HTML 或 CSS 中的链接指向本地文件。
  --backups=N   before writing file X, rotate up to N backup files.
  -K,  --backup-converted   在转换文件 X 前先将它备份为 X.orig。
  -m,  --mirror             -N -r -l inf --no-remove-listing 的缩写形式。
  -p,  --page-requisites    下载所有用于显示 HTML 页面的图片之类的元素。
       --strict-comments    用严格方式 (SGML) 处理 HTML 注释。

# 10.递归接受/拒绝：
  -A,  --accept=LIST               逗号分隔的可接受的扩展名列表。
  -R,  --reject=LIST               逗号分隔的要拒绝的扩展名列表。
       --accept-regex=REGEX        regex matching accepted URLs.
       --reject-regex=REGEX        regex matching rejected URLs.
       --regex-type=TYPE           regex type (posix|pcre).
  -D,  --domains=LIST              逗号分隔的可接受的域列表。
       --exclude-domains=LIST      逗号分隔的要拒绝的域列表。
       --follow-ftp                跟踪 HTML 文档中的 FTP 链接。
       --follow-tags=LIST          逗号分隔的跟踪的 HTML 标识列表。
       --ignore-tags=LIST          逗号分隔的忽略的 HTML 标识列表。
  -H,  --span-hosts                递归时转向外部主机。
  -L,  --relative                  只跟踪有关系的链接。
  -I,  --include-directories=LIST  允许目录的列表。
  --trust-server-names             use the name specified by the redirection
                                   url last component.
  -X,  --exclude-directories=LIST  排除目录的列表。
  -np, --no-parent                 不追溯至父目录。
```

### 2.使用wget下载文件

#### 1)下载单个文件

以下的例子是从网络下载一个文件并保存在当前目录

在下载的过程中会显示进度条，包含（下载完成百分比，已经下载的字节，当前下载速度，剩余下载时间）。

```bash
wget http://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
```

#### 2）以不同的文件名保存 -O

```bash
[root@network test]# wget https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
[root@network test]# ls
wordpress-4.9.4-zh_CN.tar.gz
```

- 我们可以使用参数-O来指定一个文件名：

```bash
wget -O wordpress.tar.gz  http://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
```

#### 3）断点续传 -c

```bash
# 使用wget -c重新启动下载中断的文件:
对于我们下载大文件时突然由于网络等原因中断非常有帮助，我们可以继续接着下载而不是重新下载一个文件
wget -c https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
```

#### 4）后台下载 -b

```bash
# 对于下载非常大的文件的时候，我们可以使用参数-b进行后台下载
[root@network test]# wget -b https://cn.wordpress.org/wordpress-4.9.4-zh_CN.tar.gz
继续在后台运行，pid 为 1463。
将把输出写入至 “wget-log”。
```

```bash
# 可以使用以下命令来察看下载进度:
[root@network test]# tail -f wget-log
  8550K .......... .......... .......... .......... .......... 96%  814K 0s
  8600K .......... .......... .......... .......... .......... 97% 9.53M 0s
  8650K .......... .......... .......... .......... .......... 98% 86.8M 0s
  8700K .......... .......... .......... .......... .......... 98%  145M 0s
  8750K .......... .......... .......... .......... .......... 99% 67.4M 0s
  8800K .......... .......... .......... .......... .......... 99%  107M 0s
  8850K .......... .........                                  100% 1.95M=16s

2018-11-10 15:39:07 (564 KB/s) - 已保存 “wordpress-4.9.4-zh_CN.tar.gz.2” [9082696/9082696])
```

#### 5）伪装代理名称下载

```txt
有些网站能通过根据判断代理名称不是浏览器而拒绝你的下载请求。
不过你可以通过–user-agent参数伪装。
```

#### 6）测试下载链接

```bash
# 进行定时下载，应该在预定时间测试下载链接是否有效。我们可以增加–spider参数进行测试检查:
wget --spider URL 

1.如果下载链接正确，将会显示 
wget –spider URL 
Spider mode enabled. Check if remote file exists. 
HTTP request sent, awaiting response… 200 OK 
Length: unspecified [text/html] 
Remote file exists and could contain further links, 
but recursion is disabled — not retrieving. 

2.这保证了下载能在预定的时间进行，但当你给错了一个链接，将会显示如下错误 
wget –spider url 
Spider mode enabled. Check if remote file exists. 
HTTP request sent, awaiting response… 404 Not Found 
Remote file does not exist — broken link!!! 
```

你可以在以下几种情况下使用spider参数：

```txt
1. 定时下载之前进行检查
2. 间隔检测网站是否可用
3. 检查网站页面的死链接
```

#### 7）增加重试次数

```bash
如果网络有问题或下载一个大文件也有可能失败。wget默认重试20次连接下载文件。如果需要，你可以使用–tries增加重试次数。
wget –tries=40 URL
```

#### 8）下载多个文件 -i

```bash
1.首先，保存一份下载链接文件 
cat > filelist.txt 
url1 
url2 
url3 
url4 

2.接着使用这个文件和参数-i下载 
wget -i filelist.txt
```

#### 9）镜像网站 –mirror

#### 10）过滤指定格式下载 –reject

```bash
下载一个网站，但你不希望下载图片:
wget –reject=gif url 
```

#### 11）把下载信息存入日志文件 -o

```bash
你不希望下载信息直接显示在终端而是在一个日志文件，可以使用以下命令：
wget -o download.log URL

使用wget -O下载并以不同的文件名保存(-O：下载文件到对应目录，并且修改文件名称)
wget -O wordpress.zip http:``//www``.minjieren.com``/download``.aspx?``id``=1080
```

#### 12）其他参数

- 限制总下载文件大小 -Q

```bash
当你想要下载的文件超过5M而退出下载，你可以使用以下命令：
wget -Q5m -i filelist.txt

注意：这个参数对单个文件下载不起作用，只能递归下载时才有效。
```

- 下载指定格式文件 -r -A

```bash
可以在以下情况使用该功能:
1.下载一个网站的所有图片
2.下载一个网站的所有视频
3.下载一个网站的所有PDF文件
wget -r -A.pdf url
```

- 使用wget FTP下载

```bash
使用wget来完成ftp链接的下载。 使用wget匿名ftp下载
wget ftp-url

使用wget用户名和密码认证的ftp下载
wget --ftp-user=USERNAME --ftp-password=PASSWORD url
```  

