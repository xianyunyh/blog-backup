title: Nodejs安装
tags:
  - js
categories:
  - Nodejs
date: 2017-11-15 00:00:00
---


## Nodejs安装

 nodejs的安装在linux可以使用源码。但是我习惯通过nvm 安装。nvm可以更改node的版本。

### 1. 下载nvm

curl的方式

```shell
curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

wget的方式

```shell
wget -qO- https://raw.github.com/creationix/nvm/master/install.sh | sh
```
<!--more-->
### 2. 更改nvm的下载源

nvm默认使用官方的mirrors。很慢，我们可以更改使用淘宝的镜像加速node的安装

更改bash的环境变量

```shell
sudo vim ~/.bashrc
export NVM_NODEJS_ORG_MIRROR=http://npm.taobao.org/mirrors/node #更改源
source ~/.bashrc #生效配置
```

### 3. 使用nvm下载nodejs

```shell
nvm install stable # 下载最新的稳定版 
nvm install v6.4.2 #下载6.4.2的版本
```

### 4. 验证nodejs和nvm

```shell
node -v #查看nodejs的版本
npm -v #查看npm的版本
```

### 5. 配置npm的淘宝镜像

加速npm的依赖的下载。配置淘宝镜像

```shell
npm config set registry https://registry.npm.taobao.org #配置镜像
npm config get registry #验证是否成功
```



经过上面的步骤下面就可以愉快的使用nodejs和npm了。