---
title: 使用GitHub、NodeJs和Hexo搭建博客 (Windows版)
date: 2016-04-19 11:13:42
categories:
- technology
- hexo
tags: ['GitHub', 'NodeJs', 'Hexo', 'Blog']
---
## 什么是 [Hexo](https://hexo.io/ "访问官网")？
![](/images/hexoIntro.png)
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
## 安装 Hexo
### 安装前提
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
* [Node.js](http://nodejs.org/ "访问官网")
* [Git](http://git-scm.com/ "访问官网")

如果您的电脑中已经安装上述必备程序，那么恭喜您！接下来只需要使用 npm 即可完成 Hexo 的安装。
```
//打开Git Bash命令行，执行以下指令
npm install -g hexo-cli
```
如果您的电脑中尚未安装所需要的程序，请根据以下安装指示完成安装。
### 安装 Git
下载并安装 [git](https://git-scm.com/download/win "点击下载git").
本教程采用的安装文件是：Git-2.8.1-64-bit.exe
### 安装 Node.js
下载并安装 [Node.js](http://nodejs.org/ "点击下载Node.js").
本教程采用的安装文件是：node-v4.4.3-x64.msi
### 安装 Hexo
所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。
```
//打开Git Bash命令行，执行以下指令
npm install -g hexo-cli
```
### 创建 [GitHub](https://github.com/) 仓库
注意：仓库名字一定为YourGitHubUserName.github.io或者YourGitHubUserName.github.com，因为网站必须是一个正确的域名格式，到时候绑定你自己的域名的时候，设置CNAME也必须是要求是域名格式。
如果没有GitHub账号，则要申请一个GitHub的账号，因为博客是把静态内容发布到你自己的GitHub分支上的。
## 建站
安装 Hexo 完成后，在硬盘上新建博客文件夹，然后在使用鼠标右键点击博客文件夹，选择Git Bash Here然后执行下列命令：
```
hexo init
npm install
```
Hexo 将会在指定文件夹中新建所需要的文件，目录如下：

![](/images/hexoStructure.png)

#### _config.yml
网站的 [配置](https://hexo.io/zh-cn/docs/configuration.html) 信息，您可以在此配置大部分的参数。

#### scaffolds
[模版](https://hexo.io/docs/writing.html#Scaffolds) 文件夹。当您新建文章时，Hexo 会根据 scaffold 来建立文件。

#### source
资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。

#### themes
[主题](https://hexo.io/docs/themes.html) 文件夹。Hexo 会根据主题来生成静态页面。

## 生成文件
```
hexo clean
hexo generate
```
### 服务器
Hexo 3.0 把服务器独立成了个别模块，您必须先安装 [hexo-server](https://github.com/hexojs/hexo-server) 才能使用。
```
npm install hexo-server --save
```
安装完成后，输入以下命令以启动服务器，您的网站会在 http://localhost:4000 下启动。在服务器启动期间，Hexo 会监视文件变动并自动更新，您无须重启服务器。
```
hexo server
```
更多有关服务器的内容：[传送门](https://hexo.io/zh-cn/docs/server.html)。
## 部署
Hexo 提供了快速方便的一键部署功能，让您只需一条命令就能将网站部署到服务器上。
Hexo 3.0 把服务器独立成了个别模块，您必须先安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git) 才能使用。
```
npm install hexo-deployer-git --save
```
在开始之前，您必须先在 _config.yml 中修改参数，一个正确的部署配置中至少要有 type 参数，例如：
deploy:
  type: git
您可同时使用多个 deployer，Hexo 会依照顺序执行每个 deployer。
本教程使用的是GitHub，所以，我的_config.yml配置如下：
```
deploy:
- type: git
  repo: https://github.com/pengsamkee/pengsamkee.github.io.git
  branch: master
```
然后执行指令
```
hexo deploy
```
如果此时在Git Bash出现以下错误信息不要慌张，听我娓娓道来：
```
*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.
```
这个是因为Git Bash还没搞懂你要上传的地址在哪里，这是使用https repo的问题，所以deploy之前，需要配置GitHub的user.name以及user.email
例如我的配置是这样的：
```
git config --global user.email "samkee@qq.com"
git config --global user.name "pengsamkee"
```
更多有关部署的内容：[传送门](https://hexo.io/docs/deployment.html)。
## 结尾
这个时候，你已经可以通过访问 http://pengsamkee.github.io 来访问博客了。
写到这里，其实很多内容都是从 [Hexo 官网](https://www.hexo.io)那里搬过来的，写在博客里仅仅是为了学习Markdown语法，以及给自己留个念想。