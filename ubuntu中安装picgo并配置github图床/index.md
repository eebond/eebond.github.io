# Ubuntu中安装PicGo并配置GitHub图床


PicGo在windows中安装较为简单，本文详细说明PicGo在Ubuntu中的安装配置过程，Windows端可以参考。

### 下载PicGo软件包

项目地址：[PicGo](https://github.com/Molunerfinn/PicGo/releases)

Ubuntu端需要下载后缀为.AppImage的软件包。

本文所下软件包为[PicGo-2.3.0.AppImage](https://github.com/Molunerfinn/PicGo/releases/download/v2.3.0/PicGo-2.3.0.AppImage)

### 安装运行软件包

给PicGo-2.3.0.AppImage权限：

```bash
chmod 777 PicGo-2.3.0.AppImage
```

右键单击，选择run，运行PicGo。

### 配置PicGo和GitHub图床

事先准备在GitHub新建一个仓库，用来存放图片，并且添加Token。

![ ](https://gitee.com/eebond0327/images/raw/main/Markdown/20220331205442.png)

其中自定义域名为  

```bash
https://raw.githubusercontent.com/eebondhttps://gitee.com/eebond0327/images/raw/main/main
```

还可以对图片链接进行CDN加速，需要改变自定义域名为：

```bash
https://cdn.jsdelivr.net/gh/用户名/图床仓库名
```

![ ](https://gitee.com/eebond0327/images/raw/main/Markdown/20220331185426.png)

指定存储路径是在仓库中创建的一个文件夹。

快捷键设置：
![ ](https://gitee.com/eebond0327/images/raw/main/Markdown/20220331141114.png)

### 安装必要支撑软件

之前的操作后，PicGo无法正常使用快捷键，缺失支撑软件。

```bash
sudo apt install xclip
```

至此，我们可以正常使用图床了。  

