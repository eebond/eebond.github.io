# Hexo博客GitHub Actions自动部署


## 问题分析

本地Hexo博客源码备份到GitHub的一个仓库中，博客展示有两个地方，一个是GitHub Pages，一个是自己VPS建的博客网站。每次先将本地源码备份推送到GitHub仓库，然后再deploy出网站内容发布到两个服务端。通过GitHub的Actions，可以让我只需要每次推送源码，GitHub帮我自动部署网站。**甚至本地都不需要安装Hexo**。

## 实现方案  

blog_backup仓库用于存放Hexo博客源码，eebond.github.io仓库存放博客网站文件，vps自建git私服存放博客网站文件。

需要一对公私钥，私钥放在blog_backup仓库，公钥放在eebond.github.io仓库和vps私服仓库所在用户。

### 生成公钥私钥

```bash
ssh-keygen -f hexo-deploy-key -t rsa
```

命令执行后会生成两个文件hexo-deploy-key（私钥）和hexo-deploy-key.pub（公钥）。

### 添加公钥

#### 添加公钥到GitHub Pages仓库中（eebond.github.io)

![ ](/images/Markdown/20220331135108.png)

#### 添加公钥到私服仓库所在用户

我的私服是在git用户中创建的，所以添加到git用户下的.ssh/authorized_keys文件中。

### 添加私钥

将私钥添加到blog_backup仓库的Actions secrets：
![ ](/images/Markdown/20220331142405.png)

### 配置workflow文件

在blog_backup仓库根目录下创建.github/workflows/autodeploy.yml文件，文件名随意设置。
文件内容：

```bash
# 当有改动推送到main分支时，启动Action
name: 自动部署

on:
  push:
    branches:
      - master #2020年10月后github新建仓库默认分支改为main，注意更改

  release:
    types:
      - published

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: 检查分支
        uses: actions/checkout@v2
        with:
          ref: master #2020年10月后github新建仓库默认分支改为main，注意更改

      - name: 安装 Node
        uses: actions/setup-node@v1
        with:
          node-version: '12.x'

      - name: 安装 Hexo
        run: |
          export TZ='Asia/Shanghai'
          npm install hexo-cli -g
      - name: 缓存 Hexo
        uses: actions/cache@v1
        id: cache
        with:
          path: node_modules
          key: ${{runner.OS}}-${{hashFiles('**/package-lock.json')}}

      - name: 安装依赖
        if: steps.cache.outputs.cache-hit != 'true'
        run: |
          npm install --save
      - name: 生成静态文件
        run: |
          hexo clean
          hexo generate
      - name: 服务器验证
        env:
          ACTION_DEPLOY_KEY: ${{ secrets.HEXO_DEPLOY_KEY }}
        run: |
          sudo timedatectl set-timezone "Asia/Shanghai"
          mkdir -p ~/.ssh/
          echo "$ACTION_DEPLOY_KEY" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan -p 29488 23.105.219.155 >> ~/.ssh/known_hosts  #此处填写你的服务器IP
          git config --global user.name "eebond"
          git config --global user.email "1422797591@qq.com" #修改为你的GitHub用户名邮箱
      - name: 部署
        run: |
          hexo deploy
```

之后就可以本地推送后，GitHub自动部署网站了。  

