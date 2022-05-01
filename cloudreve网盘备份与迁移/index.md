# Cloudreve网盘备份与迁移


## 备份

Cloudreve网盘使用内置数据库，实际所有的数据都保存在uploads目录里，而我的uploads使用软链接指向我的阿里云盘中的uploads目录，所以我实际只需要备份如下文件：

```bash
  cloudreve
  cloudreve.db
  conf.ini
```

实际只需要在Cloudreve目录创建git仓库，将上述文件推送到GitHub仓库即可，具体操作不详细叙述。

## 迁移

之前已经备份过了，所以当要在新服务器上重新搭建cloudreve，只需要pull上述仓库分支。

我的具体操作如下：

```bash
git pull git@github.com:eebond/banwagong.git master
```

之后按照官方教程安装配置<https://docs.cloudreve.org/getting-started/install>  

