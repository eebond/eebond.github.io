# VSCode在Ubuntu下无法使用中文输入


### 问题

今天遇到无法在Ubuntu20.04环境中下载了VSCode，却无法在VSCode中切换中文输入法，只能输入英文。

### 问题原因

VSCode是使用Ubuntu自带的商店下载的，所以无法使用，具体原因不知。

### 问题解决

1.先卸载已安装的VSCode

  ```bash
  snap remove code
  ```  

2.从VSCode官网下载对应版本的安装包重新安装

 > 下载地址链接：<https://code.visualstudio.com/Download>  

