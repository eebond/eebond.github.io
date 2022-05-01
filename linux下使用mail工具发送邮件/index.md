# Linux下使用mail工具发送邮件


 邮件常常是Linux下监控报警手段之一。Linux下的mail命令可以方便，快速的完成发送邮件。下面以CentOS为例

## 1.安装mailx

```bash
yum install mailx
```

## 2.配置

vi /etc/mail.rc   在文件尾加上如下配置  （注:因为163的设置相对简单些，以163邮箱为例，QQ邮箱等，其他邮箱因为安全等因素，需要设置的比较多，具体的可以搞下，本文不作重点。QQ邮箱也是可以的，需要开启smtp服务，并生成授权码）

```txt
set from=zabbix@163.com
set smtp=smtp.163.com
set smtp-auth-user=zabbix@163.com
set smtp-auth-password=邮箱密码
set smtp-auth=login
```

## 3.发送邮件测试

```bash
echo "Content" | mail -s "Title" abc@163.com
```

带附件的邮件(需要注意的是目标邮箱一定要放在最后写)

```bash
echo "Content" | mail -s "Title" -a <附件> abc@163.com
```

