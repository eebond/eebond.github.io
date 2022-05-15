# Linux实现免密登录以及密钥生成方法


## 前言  

本文主要讲述实现从一台Linux服务器实现对另一台Linux服务器的
免密登录，还有在linux上生成ssh key密钥的方法。

---

## 生成SSH KEY密钥文件

```bash
ssh-keygen -t rsa
```  

用户家目录中的.ssh目录下出现如下文件：

```txt
authorized_keys  #认证用户的公钥，认证用户可以免密登录当前账户
id_rsa           #当前用户的密钥
id_rsa.pub       #当前用户的公钥  
```  

## 设置免密登录

其实免密登录的原理就是把当前用户的公钥放到被登录用户的authorized_keys文件中去。

```bash
ssh-copy-id root@144.34.163.167 -p 29488
```  

期间需要输入被登录用户的密码才能将当前用户的公钥放入被登录用户的authorized_keys文件中去。  
还有个办法就是复制当前用户的公钥直接粘贴到被登录用户的authorized_keys文件中去。

如果被登录的Linux的端口不是默认的22端口，则要用 -p 选项来指定登录端口。  

## 尝试免密登录

```bash
ssh root@144.34.163.167 -p 29488
```  

成功免密登录！  

