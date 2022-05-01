# curl命令使用教程

## curl 命令行工具及参数

- curl是一个开源的用于数据传输的命令行工具与库，它使用URL语法格式，支持众多传输协议，包括：HTTP、HTTPS、FTP、FTPS、GOPHER、TFTP、SCP、SFTP、SMB、TELNET、DICT、LDAP、LDAPS、FILE、IMAP、SMTP、POP3、RTSP和RTMP。curl库提供了很多强大的功能，你可以利用它来进行HTTP/HTTPS请求、上传/下载文件等，且支持Cookie、认证、代理、限速等。  

### 1.curl的基本使用

- 1.使用curl访问一个网址（最基本用法）
  在命令行中输入“curl 网址”即可在命令显示界面显示该网址的内容。这种使用方式通常用来检测一个网址是否能够正常访问，因为Linux服务器最小化安装里没有浏览器，因此这种方式就是实现一种浏览器访问的功能。  

- 2.使用curl下载文件
  在命令行中输入“curl -O 一个word网络地址 ”这句命令的意思是将该word下载到本地。在命令行中输入“curl -o 2.jpg 一个1.jpg网络地址”这句话是将1.jpg下载保存到本地，并可以重命名为2.jpg。  

- .利用curl上传文件
  在命令行中输入“curl -T 1.JPG -u 用户名:密码 `ftp://FTP地址/img/` ” 这句命令的意思是将1.jpg上传到一个ftp的目录下，当然了使用该句命令需要知道ftp的基本信息如端口用户名密码等。

### 2.curl命令的进阶

```bash

目录：
1. curl的使用
1.1 URL访问
1.2 表单提交
1.3 其它HTTP请求方法
1.4 文件上传
1.5 HTTPS支持
1.6 添加请求头
1.7 Cookie支持
2. curl语法及选项
----------------

1. curl的使用
1.1 URL访问
访问一个网页时，可以使用curl命令后加上要访问的网址：

curl itbilu.com 
<html>
<head><title>301 Moved Permanently</title></head>
<body bgcolor="white">
<center><h1>301 Moved Permanently</h1></center>
<hr><center>nginx/1.6.2</center>
</body>
</html>
如上所示，我们就看到所访问网址的页面源码。

重定向跟踪

在上面示例中，页面使用了301重定向，这时我们可以添加-L参数来跟踪URL重定向： curl -L itbilu.com
页面保存

如果需要将页面源码保存到本地，可以使用-o参数：

curl -o [文件名] itbilu.com
查看头信息

如果需要查看访问页面的可以添加-i或--include参数： curl -i itbilu.com
添加-i参数后，页面响应头会和页面源码（响应体）一块返回。如果只想查看响应头，可以使用-I或--head参数：

curl -I itbilu.com
HTTP/1.1 301 Moved Permanently
Server: nginx/1.6.2
Date: Sun, 25 Jun 2017 02:03:45 GMT
Content-Type: text/html
Content-Length: 184
Connection: keep-alive
Location: https://itbilu.com/


1.2 表单提交
通过Form表单，可以将Web页面的表单数据提交到服务端。提交表单时，可以使用GET或POST提交方法。

curl同样支持表单数据提交，也可以使用GET或POST提交方法。

GET数据提交

当全用GET表单数据提交时，提交数据会被附加到请求URL的后面。类型如下： curl '//itbilu.com/?keyword=linux&page=3'
使用curl进行GET数据提交时，也可以直接把提交数据添加到URL后面：

curl https://itbilu.com/?keyword=linux&page=3
POST数据提交

curl使用POST提交表单数据时，除了-X参数指定请求方法外，还要使用--data参数添加提交数据：

curl -X POST --data 'keyword=linux' itbilu.com


1.3 其它HTTP请求方法
目前为止，我们使用GET和POST两种HTTP请求。curl支持所有HTTP请求方法，只要通过-X参数指定即可。

如，使用DELETE请求： curl -X DELETE itbilu.com/examlple.html
使用PUT请求，并指定请求数据：

curl -X PUT --data 'keyword=linux' itbilu.com


1.4 文件上传
curl支持文件上传，上传文件时使用-T或--upload-file参数： curl -T ./index.html www.uploadhttp.com/receive.cgi


1.5 HTTPS支持
对于使用了SSL/TLS加密的HTTPS协议，可以使用curl直接访问：

curl https://itbilu.com
如果使用的本地ssl证书认证的HTTPS，可以通过-E或--cert参数指定本地证书： curl -E mycert.pem https:/itbilu.com


1.6 添加请求头
有时在进行HTTP请求时，需要自定义请求头。在curl中，可以通过-H或--header参数来指定请求头。多次使用-H或--header参数可指定多个请求头。

如，指定Content-Type及Authorization请求头：

curl -H 'Content-Type:application/json' -H 'Authorization: bearer eyJhbGciOiJIUzI1NiJ9' itbilu.com


1.7 Cookie支持
Cookie是一种常用的保持服务端会话信息的方法，crul也支持使用Cookie。

可以通过--cookie参数指定发送请求时的Cookie值，也可以通过-b [文件名]来指定一个存储了Cookie值的本地文件： curl -b stored_cookies_in_file itbilu.com
Cookie值可能会被服务器所返回的值所修改，并应用于下次HTTP请求。这时，可以能过-c参数指定存储服务器返回Cookie值的存储文件：

$ curl -b cookies.txt -c newcookies.txt itbilu.com


除以上用法外，curl还可以设置用户代理（客户端）信息、使用代理服务器、指定认证用户名／密码等。详见：curl语法及选项

curl --silent -H "Host: www.test.com" "192.168.0.1/xxx/xxx/t.php"
curl "http://www.test.com/LiveUserCount.ac " -x 127.0.0.1:1080

```

### 3.curl语法及选项

```bash
# curl语法结构如下：
===================
curl [options...] <url>
参数选项

curl（7.29.0）所支持的选项（options）参数如下：

在以下选项中，(H) 表示仅适用 HTTP/HTTPS ，(F) 表示仅适用于 FTP
    --anyauth       选择 "any" 认证方法 (H)
-a, --append        添加要上传的文件 (F/SFTP)
    --basic         使用HTTP基础认证（Basic Authentication）(H)
    --cacert FILE   CA 证书，用于每次请求认证 (SSL)
    --capath DIR    CA 证书目录 (SSL)
-E, --cert CERT[:PASSWD] 客户端证书文件及密码 (SSL)
    --cert-type TYPE 证书文件类型 (DER/PEM/ENG) (SSL)
    --ciphers LIST  SSL 秘钥 (SSL)
    --compressed    请求压缩 (使用 deflate 或 gzip)
-K, --config FILE   指定配置文件
    --connect-timeout SECONDS  连接超时设置
-C, --continue-at OFFSET  断点续转
-b, --cookie STRING/FILE  Cookies字符串或读取Cookies的文件位置 (H)
-c, --cookie-jar FILE  操作结束后，要写入 Cookies 的文件位置 (H)
    --create-dirs   创建必要的本地目录层次结构
    --crlf          在上传时将 LF 转写为 CRLF
    --crlfile FILE  从指定的文件获得PEM格式CRL列表
-d, --data DATA     HTTP POST 数据 (H)
    --data-ascii DATA  ASCII 编码 HTTP POST 数据 (H)
    --data-binary DATA  binary 编码 HTTP POST 数据 (H)
    --data-urlencode DATA  url 编码 HTTP POST 数据 (H)
    --delegation STRING GSS-API 委托权限
    --digest        使用数字身份验证 (H)
    --disable-eprt  禁止使用 EPRT 或 LPRT (F)
    --disable-epsv  禁止使用 EPSV (F)
-D, --dump-header FILE  将头信息写入指定的文件
    --egd-file FILE  为随机数据设置EGD socket路径(SSL)
    --engine ENGINGE  加密引擎 (SSL). "--engine list" 指定列表
-f, --fail          连接失败时不显示HTTP错误信息 (H)
-F, --form CONTENT  模拟 HTTP 表单数据提交（multipart POST） (H)
    --form-string STRING  模拟 HTTP 表单数据提交 (H)
    --ftp-account DATA  帐户数据提交 (F)
    --ftp-alternative-to-user COMMAND  指定替换 "USER [name]" 的字符串 (F)
    --ftp-create-dirs  如果不存在则创建远程目录 (F)
    --ftp-method [MULTICWD/NOCWD/SINGLECWD] 控制 CWD (F)
    --ftp-pasv      使用 PASV/EPSV 替换 PORT (F)
-P, --ftp-port ADR  使用指定 PORT 及地址替换 PASV (F)
    --ftp-skip-pasv-ip 跳过 PASV 的IP地址 (F)
    --ftp-pret      在 PASV 之前发送 PRET (drftpd) (F)
    --ftp-ssl-ccc   在认证之后发送 CCC (F)
    --ftp-ssl-ccc-mode ACTIVE/PASSIVE  设置 CCC 模式 (F)
    --ftp-ssl-control ftp 登录时需要 SSL/TLS (F)
-G, --get           使用 HTTP GET 方法发送 -d 数据  (H)
-g, --globoff       禁用的 URL 队列 及范围使用 {} 和 []
-H, --header LINE   要发送到服务端的自定义请求头 (H)
-I, --head          仅显示响应文档头
-h, --help          显示帮助
-0, --http1.0       使用 HTTP 1.0 (H)
    --ignore-content-length  忽略 HTTP Content-Length 头
-i, --include       在输出中包含协议头 (H/F)
-k, --insecure      允许连接到 SSL 站点，而不使用证书 (H)
    --interface INTERFACE  指定网络接口／地址
-4, --ipv4          将域名解析为 IPv4 地址
-6, --ipv6          将域名解析为 IPv6 地址
-j, --junk-session-cookies 读取文件中但忽略会话cookie (H)
    --keepalive-time SECONDS  keepalive 包间隔
    --key KEY       私钥文件名 (SSL/SSH)
    --key-type TYPE 私钥文件类型 (DER/PEM/ENG) (SSL)
    --krb LEVEL     启用指定安全级别的 Kerberos (F)
    --libcurl FILE  命令的libcurl等价代码
    --limit-rate RATE  限制传输速度
-l, --list-only    只列出FTP目录的名称 (F)
    --local-port RANGE  强制使用的本地端口号
-L, --location      跟踪重定向 (H)
    --location-trusted 类似 --location 并发送验证信息到其它主机 (H)
-M, --manual        显示全手动
    --mail-from FROM  从这个地址发送邮件
    --mail-rcpt TO  发送邮件到这个接收人(s)
    --mail-auth AUTH  原始电子邮件的起始地址
    --max-filesize BYTES  下载的最大文件大小 (H/F)
    --max-redirs NUM  最大重定向数 (H)
-m, --max-time SECONDS  允许的最多传输时间
    --metalink      处理指定的URL上的XML文件
    --negotiate     使用 HTTP Negotiate 认证 (H)
-n, --netrc         必须从 .netrc 文件读取用户名和密码
    --netrc-optional 使用 .netrc 或 URL; 将重写 -n 参数
    --netrc-file FILE  设置要使用的 netrc 文件名
-N, --no-buffer     禁用输出流的缓存
    --no-keepalive  禁用 connection 的 keepalive
    --no-sessionid  禁止重复使用 SSL session-ID (SSL)
    --noproxy       不使用代理的主机列表
    --ntlm          使用 HTTP NTLM 认证 (H)
-o, --output FILE   将输出写入文件，而非 stdout
    --pass PASS     传递给私钥的短语 (SSL/SSH)
    --post301       在 301 重定向后不要切换为 GET 请求 (H)
    --post302       在 302 重定向后不要切换为 GET 请求 (H)
    --post303       在 303 重定向后不要切换为 GET 请求 (H)
-#, --progress-bar  以进度条显示传输进度
    --proto PROTOCOLS  启用/禁用 指定的协议
    --proto-redir PROTOCOLS  在重定向上 启用/禁用 指定的协议
-x, --proxy [PROTOCOL://]HOST[:PORT] 在指定的端口上使用代理
    --proxy-anyauth 在代理上使用 "any" 认证方法 (H)
    --proxy-basic   在代理上使用 Basic 认证  (H)
    --proxy-digest  在代理上使用 Digest 认证 (H)
    --proxy-negotiate 在代理上使用 Negotiate 认证 (H)
    --proxy-ntlm    在代理上使用 NTLM 认证 (H)
-U, --proxy-user USER[:PASSWORD]  代理用户名及密码
     --proxy1.0 HOST[:PORT]  在指定的端口上使用 HTTP/1.0 代理
-p, --proxytunnel   使用HTTP代理 (用于 CONNECT)
    --pubkey KEY    公钥文件名 (SSH)
-Q, --quote CMD     在传输开始前向服务器发送命令 (F/SFTP)
    --random-file FILE  读取随机数据的文件 (SSL)
-r, --range RANGE   仅检索范围内的字节
    --raw           使用原始HTTP传输，而不使用编码 (H)
-e, --referer       Referer URL (H)
-J, --remote-header-name 从远程文件读取头信息 (H)
-O, --remote-name   将输出写入远程文件
    --remote-name-all 使用所有URL的远程文件名
-R, --remote-time   将远程文件的时间设置在本地输出上
-X, --request COMMAND  使用指定的请求命令
    --resolve HOST:PORT:ADDRESS  将 HOST:PORT 强制解析到 ADDRESS
    --retry NUM   出现问题时的重试次数
    --retry-delay SECONDS 重试时的延时时长
    --retry-max-time SECONDS  仅在指定时间段内重试
-S, --show-error    显示错误. 在选项 -s 中，当 curl 出现错误时将显示
-s, --silent        Silent模式。不输出任务内容
    --socks4 HOST[:PORT]  在指定的 host + port 上使用 SOCKS4 代理
    --socks4a HOST[:PORT]  在指定的 host + port 上使用 SOCKSa 代理
    --socks5 HOST[:PORT]  在指定的 host + port 上使用 SOCKS5 代理
    --socks5-hostname HOST[:PORT] SOCKS5 代理，指定用户名、密码
    --socks5-gssapi-service NAME  为gssapi使用SOCKS5代理服务名称 
    --socks5-gssapi-nec  与NEC Socks5服务器兼容
-Y, --speed-limit RATE  在指定限速时间之后停止传输
-y, --speed-time SECONDS  指定时间之后触发限速. 默认 30
    --ssl           尝试 SSL/TLS (FTP, IMAP, POP3, SMTP)
    --ssl-reqd      需要 SSL/TLS (FTP, IMAP, POP3, SMTP)
-2, --sslv2         使用 SSLv2 (SSL)
-3, --sslv3         使用 SSLv3 (SSL)
    --ssl-allow-beast 允许的安全漏洞，提高互操作性(SSL)
    --stderr FILE   重定向 stderr 的文件位置. - means stdout
    --tcp-nodelay   使用 TCP_NODELAY 选项
-t, --telnet-option OPT=VAL  设置 telnet 选项
     --tftp-blksize VALUE  设备 TFTP BLKSIZE 选项 (必须 >512)
-z, --time-cond TIME  基于时间条件的传输
-1, --tlsv1         使用 => TLSv1 (SSL)
    --tlsv1.0       使用 TLSv1.0 (SSL)
    --tlsv1.1       使用 TLSv1.1 (SSL)
    --tlsv1.2       使用 TLSv1.2 (SSL)
    --trace FILE    将 debug 信息写入指定的文件
    --trace-ascii FILE  类似 --trace 但使用16进度输出
    --trace-time    向 trace/verbose 输出添加时间戳
    --tr-encoding   请求压缩传输编码 (H)
-T, --upload-file FILE  将文件传输（上传）到指定位置
    --url URL       指定所使用的 URL
-B, --use-ascii     使用 ASCII/text 传输
-u, --user USER[:PASSWORD]  指定服务器认证用户名、密码
    --tlsuser USER  TLS 用户名
    --tlspassword STRING TLS 密码
    --tlsauthtype STRING  TLS 认证类型 (默认 SRP)
    --unix-socket FILE    通过这个 UNIX socket 域连接
-A, --user-agent STRING  要发送到服务器的 User-Agent (H)
-v, --verbose       显示详细操作信息
-V, --version       显示版本号并退出
-w, --write-out FORMAT  完成后输出什么
    --xattr        将元数据存储在扩展文件属性中
-q                 .curlrc 如果作为第一个参数无效

```

### 4. curl命令的使用范例

```bash
curl是一种命令行工具，作用是发出网络请求，然后得到和提取数据，显示在"标准输出"（stdout）上面。

它支持多种协议，下面举例讲解如何将它用于网站开发。

一、查看网页源码

直接在curl命令后加上网址，就可以看到网页源码。我们以网址www.sina.com为例（选择该网址，主要因为它的网页代码较短）：

　　curl www.sina.com

　　<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
　　<html><head>
　　<title>301 Moved Permanently</title>
　　</head><body>
　　<h1>Moved Permanently</h1>
　　<p>The document has moved <a href="http://www.sina.com.cn/">here</a>.</p>
　　</body></html>

如果要把这个网页保存下来，可以使用`-o`参数，这就相当于使用wget命令了。

　　 curl -o [文件名] www.sina.com

二、自动跳转

有的网址是自动跳转的。使用`-L`参数，curl就会跳转到新的网址。

　　curl -L www.sina.com

键入上面的命令，结果就自动跳转为www.sina.com.cn。

三、显示头信息

`-i`参数可以显示http response的头信息，连同网页代码一起。

　　 curl -i www.sina.com

　　HTTP/1.0 301 Moved Permanently
　　Date: Sat, 03 Sep 2011 23:44:10 GMT
　　Server: Apache/2.0.54 (Unix)
　　Location: http://www.sina.com.cn/
　　Cache-Control: max-age=3600
　　Expires: Sun, 04 Sep 2011 00:44:10 GMT
　　Vary: Accept-Encoding
　　Content-Length: 231
　　Content-Type: text/html; charset=iso-8859-1
　　Age: 3239
　　X-Cache: HIT from sh201-9.sina.com.cn
　　Connection: close

　　<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
　　<html><head>
　　<title>301 Moved Permanently</title>
　　</head><body>
　　<h1>Moved Permanently</h1>
　　<p>The document has moved <a href="http://www.sina.com.cn/">here</a>.</p>
　　</body></html>

`-I`参数则是只显示http response的头信息。

四、显示通信过程

`-v`参数可以显示一次http通信的整个过程，包括端口连接和http request头信息。

　　curl -v www.sina.com

　　* About to connect() to www.sina.com port 80 (#0)
　　* Trying 61.172.201.195... connected
　　* Connected to www.sina.com (61.172.201.195) port 80 (#0)
　　> GET / HTTP/1.1
　　> User-Agent: curl/7.21.3 (i686-pc-linux-gnu) libcurl/7.21.3 OpenSSL/0.9.8o zlib/1.2.3.4 libidn/1.18
　　> Host: www.sina.com
　　> Accept: */*
　　>
　　* HTTP 1.0, assume close after body
　　< HTTP/1.0 301 Moved Permanently
　　< Date: Sun, 04 Sep 2011 00:42:39 GMT
　　< Server: Apache/2.0.54 (Unix)
　　< Location: http://www.sina.com.cn/
　　< Cache-Control: max-age=3600
　　< Expires: Sun, 04 Sep 2011 01:42:39 GMT
　　< Vary: Accept-Encoding
　　< Content-Length: 231
　　< Content-Type: text/html; charset=iso-8859-1
　　< X-Cache: MISS from sh201-19.sina.com.cn
　　< Connection: close
　　<
　　<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
　　<html><head>
　　<title>301 Moved Permanently</title>
　　</head><body>
　　<h1>Moved Permanently</h1>
　　<p>The document has moved <a href="http://www.sina.com.cn/">here</a>.</p>
　　</body></html>
　　* Closing connection #0

如果你觉得上面的信息还不够，那么下面的命令可以查看更详细的通信过程。

　　 curl --trace output.txt www.sina.com

或者

　　curl --trace-ascii output.txt www.sina.com

运行后，请打开output.txt文件查看。

五、发送表单信息

发送表单信息有GET和POST两种方法。GET方法相对简单，只要把数据附在网址后面就行。

　　 curl example.com/form.cgi?data=xxx

POST方法必须把数据和网址分开，curl就要用到--data参数。

　　curl -X POST --data "data=xxx" example.com/form.cgi

如果你的数据没有经过表单编码，还可以让curl为你编码，参数是`--data-urlencode`。

　　 curl -X POST--data-urlencode "date=April 1" example.com/form.cgi

六、HTTP动词

curl默认的HTTP动词是GET，使用`-X`参数可以支持其他动词。

　　curl -X POST www.example.com

　　 curl -X DELETE www.example.com

七、文件上传

假定文件上传的表单是下面这样：

　　<form method="POST" enctype='multipart/form-data' action="upload.cgi">
　　　　<input type=file name=upload>
　　　　<input type=submit name=press value="OK">
　　</form>

你可以用curl这样上传文件：

　　curl --form upload=@localfilename --form press=OK [URL]

八、Referer字段

有时你需要在http request头信息中，提供一个referer字段，表示你是从哪里跳转过来的。

　　 curl --referer http://www.example.com http://www.example.com

九、User Agent字段

这个字段是用来表示客户端的设备信息。服务器有时会根据这个字段，针对不同设备，返回不同格式的网页，比如手机版和桌面版。

iPhone4的User Agent是

　　Mozilla/5.0 (iPhone; U; CPU iPhone OS 4_0 like Mac OS X; en-us) AppleWebKit/532.9 (KHTML, like Gecko) Version/4.0.5 Mobile/8A293 Safari/6531.22.7

curl可以这样模拟：

　　curl --user-agent "[User Agent]" [URL]

十、cookie

使用`--cookie`参数，可以让curl发送cookie。

　　 curl --cookie "name=xxx" www.example.com

至于具体的cookie的值，可以从http response头信息的`Set-Cookie`字段中得到。

`-c cookie-file`可以保存服务器返回的cookie到文件，`-b cookie-file`可以使用这个文件作为cookie信息，进行后续的请求。

　　curl -c cookies http://example.com
　　 curl -b cookies http://example.com

十一、增加头信息

有时需要在http request之中，自行增加一个头信息。`--header`参数就可以起到这个作用。

　　curl --header "Content-Type:application/json" http://example.com

十二、HTTP认证

有些网域需要HTTP认证，这时curl需要用到`--user`参数。

　　 curl --user name:password example.com

不带有任何参数时，curl 就是发出 GET 请求。


curl https://www.example.com
上面命令向www.example.com发出 GET 请求，服务器返回的内容会在命令行输出。

-A
-A参数指定客户端的用户代理标头，即User-Agent。curl 的默认用户代理字符串是curl/[version]。 curl -A 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.100 Safari/537.36' https://google.com
上面命令将User-Agent改成 Chrome 浏览器。


curl -A '' https://google.com
上面命令会移除User-Agent标头。

也可以通过-H参数直接指定标头，更改User-Agent。 curl -H 'User-Agent: php/1.0' https://google.com
-b
-b参数用来向服务器发送 Cookie。


curl -b 'foo=bar' https://google.com
上面命令会生成一个标头Cookie: foo=bar，向服务器发送一个名为foo、值为bar的 Cookie。 curl -b 'foo1=bar;foo2=bar2' https://google.com
上面命令发送两个 Cookie。


curl -b cookies.txt https://www.google.com
上面命令读取本地文件cookies.txt，里面是服务器设置的 Cookie（参见-c参数），将其发送到服务器。

-c
-c参数将服务器设置的 Cookie 写入一个文件。 curl -c cookies.txt https://www.google.com
上面命令将服务器的 HTTP 回应所设置 Cookie 写入文本文件cookies.txt。

-d
-d参数用于发送 POST 请求的数据体。


curl -d'login=emma＆password=123'-X POST https://google.com/login
# 或者 curl -d 'login=emma' -d 'password=123' -X POST  https://google.com/login
使用-d参数以后，HTTP 请求会自动加上标头Content-Type : application/x-www-form-urlencoded。并且会自动将请求转为 POST 方法，因此可以省略-X POST。

-d参数可以读取本地文本文件的数据，向服务器发送。


curl -d '@data.txt' https://google.com/login
上面命令读取data.txt文件的内容，作为数据体向服务器发送。

--data-urlencode
--data-urlencode参数等同于-d，发送 POST 请求的数据体，区别在于会自动将发送的数据进行 URL 编码。 curl --data-urlencode 'comment=hello world' https://google.com/login
上面代码中，发送的数据hello world之间有一个空格，需要进行 URL 编码。

-e
-e参数用来设置 HTTP 的标头Referer，表示请求的来源。


curl -e 'https://google.com?q=example' https://www.example.com
上面命令将Referer标头设为https://google.com?q=example。

-H参数可以通过直接添加标头Referer，达到同样效果。


curl -H 'Referer: https://google.com?q=example' https://www.example.com
-F
-F参数用来向服务器上传二进制文件。


curl -F 'file=@photo.png' https://google.com/profile
上面命令会给 HTTP 请求加上标头Content-Type: multipart/form-data，然后将文件photo.png作为file字段上传。

-F参数可以指定 MIME 类型。 curl -F 'file=@photo.png;type=image/png' https://google.com/profile
上面命令指定 MIME 类型为image/png，否则 curl 会把 MIME 类型设为application/octet-stream。

-F参数也可以指定文件名。


curl -F 'file=@photo.png;filename=me.png' https://google.com/profile
上面命令中，原始文件名为photo.png，但是服务器接收到的文件名为me.png。

-G
-G参数用来构造 URL 的查询字符串。 curl -G -d 'q=kitties' -d 'count=20' https://google.com/search
上面命令会发出一个 GET 请求，实际请求的 URL 为https://google.com/search?q=kitties&count=20。如果省略--G，会发出一个 POST 请求。

如果数据需要 URL 编码，可以结合--data--urlencode参数。


curl -G --data-urlencode 'comment=hello world' https://www.example.com
-H
-H参数添加 HTTP 请求的标头。 curl -H 'Accept-Language: en-US' https://google.com
上面命令添加 HTTP 标头Accept-Language: en-US。


curl -H 'Accept-Language: en-US' -H 'Secret-Message: xyzzy' https://google.com
上面命令添加两个 HTTP 标头。 curl -d '{"login": "emma", "pass": "123"}' -H 'Content-Type: application/json' https://google.com/login
上面命令添加 HTTP 请求的标头是Content-Type: application/json，然后用-d参数发送 JSON 数据。

-i
-i参数打印出服务器回应的 HTTP 标头。


curl -i https://www.example.com
上面命令收到服务器回应后，先输出服务器回应的标头，然后空一行，再输出网页的源码。

-I
-I参数向服务器发出 HEAD 请求，然会将服务器返回的 HTTP 标头打印出来。 curl -I https://www.example.com
上面命令输出服务器对 HEAD 请求的回应。

--head参数等同于-I。


curl --head https://www.example.com
-k
-k参数指定跳过 SSL 检测。 curl -k https://www.example.com
上面命令不会检查服务器的 SSL 证书是否正确。

-L
-L参数会让 HTTP 请求跟随服务器的重定向。curl 默认不跟随重定向。


curl -L -d 'tweet=hi' https://api.twitter.com/tweet
--limit-rate
--limit-rate用来限制 HTTP 请求和回应的带宽，模拟慢网速的环境。 curl --limit-rate 200k https://google.com
上面命令将带宽限制在每秒 200K 字节。

-o
-o参数将服务器的回应保存成文件，等同于wget命令。


curl -o example.html https://www.example.com
上面命令将www.example.com保存成example.html。

-O
-O参数将服务器回应保存成文件，并将 URL 的最后部分当作文件名。 curl -O https://www.example.com/foo/bar.html
上面命令将服务器回应保存成文件，文件名为bar.html。

-s
-s参数将不输出错误和进度信息。


curl -s https://www.example.com
上面命令一旦发生错误，不会显示错误信息。不发生错误的话，会正常显示运行结果。

如果想让 curl 不产生任何输出，可以使用下面的命令。 curl -s -o /dev/null https://google.com
-S
-S参数指定只输出错误信息，通常与-s一起使用。


curl -s -o /dev/null https://google.com
上面命令没有任何输出，除非发生错误。

-u
-u参数用来设置服务器认证的用户名和密码。 curl -u 'bob:12345' https://google.com/login
上面命令设置用户名为bob，密码为12345，然后将其转为 HTTP 标头Authorization: Basic Ym9iOjEyMzQ1。

curl 能够识别 URL 里面的用户名和密码。


curl https://bob:12345@google.com/login
上面命令能够识别 URL 里面的用户名和密码，将其转为上个例子里面的 HTTP 标头。 curl -u 'bob' https://google.com/login
上面命令只设置了用户名，执行后，curl 会提示用户输入密码。

-v
-v参数输出通信的整个过程，用于调试。


curl -v https://www.example.com
--trace参数也可以用于调试，还会输出原始的二进制数据。 curl --trace - https://www.example.com
-x
-x参数指定 HTTP 请求的代理。


curl -x socks5://james:cats@myproxy.com:8080 https://www.example.com
上面命令指定 HTTP 请求通过myproxy.com:8080的 socks5 代理发出。

如果没有指定代理协议，默认为 HTTP。 curl -x james:cats@myproxy.com:8080 https://www.example.com
上面命令中，请求的代理使用 HTTP 协议。

-X
-X参数指定 HTTP 请求的方法。


$ curl -X POST https://www.example.com
上面命令对https://www.example.com发出 POST 请求。

```  

