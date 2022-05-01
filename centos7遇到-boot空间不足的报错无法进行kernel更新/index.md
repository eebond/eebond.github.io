# centos7遇到/boot空间不足的报错，无法进行kernel更新

安装系统时，如果 /boot 分区给的空间太少，在更新时可能就会显示 /boot 空间不足的 error，因为 Linux 更新时会保留近几次更新的 kernel，因此出现错误时，我们可以把一些比较旧的 kernel 删掉，帮 /boot 释放空间。

以下方法适用于 CentOS 7 与 CentOS 8

开机时可以看到目前系统里有 4 个版本的 kernel（rescue 不算）

![]({{< param cdnPrefix >}}/images/Markdown/20211125223359.png)

编辑 /etc/yum.conf，并将下面的参数更改为 2 或 3  

```txt
installonly_limit=2
```

这个参数表示让 package manager 保留多少个 kernel 档，一般的使用者保留目前的与前一、两个版本其实就够了。

安裝 yum-utils

```bash
yum install yum-utils
```

清除旧的 kernels

```bash
package-cleanup --oldkernels --count=2
```

完成后，系统就只会留下近两个版本的 kernel，/boot 的空间也就释放出来啦！
（个人建议 /boot 空间最好大于 500MB，就不太会碰到这个问题了）

重开机看一下
![]({{< param cdnPrefix >}}/images/Markdown/20211125223805.png)

剩下两个 kernel 了

