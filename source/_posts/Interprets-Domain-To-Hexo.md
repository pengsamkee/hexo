---
title: 把自己的域名解析到gh-pages
date: 2016-04-20 11:05:12
categories:
- technology
- hexo
tags: ['Hexo', 'Domain']
---
## 前言
在 [第一篇教程](http://www.samkee.me/2016/04/19/Use-NodeJs-Hexo-GitHub-To-Build-Blog-Windows/) 中已经把博客部署到GitHub上面去了，我设置的仓库名字是pengsamkee.github.io，所以gh-pages会把我的博客发布到pengsamkee.github.io这个域名上面去，访问pengsamkee.github.io可以预览到博客内容。
***
## 如何解析自己的域名
如果自己购买了域名，可以把自己的域名解析到pengsamkee.github.io，当在浏览器输入www.samkee.me，域名商会帮我们实现url跳转。
如下图所示，添加一条CNAME类型的高级解析：主机名为www，解析类型为CNAME记录，对应值为pengsamkee.github.io，TTL为200
![](/images/hexoDomain.png)
### 前方高能
虽然你把域名解析到pengsamkee.github.io了，但是输入www.samkee.me之后，还是无法发生跳转。这是因为博客根目录必须也要提供一个CNAME文件这样才能互相对应起来，也就是说域名商一定要在网站根目录找到这个CNAME文件才给你跳转过去的。
#### 还记得 [第一篇教程](http://www.samkee.me/2016/04/19/Use-NodeJs-Hexo-GitHub-To-Build-Blog-Windows/) 中，提到某一个文件夹的说明吗？
#### source
资源文件夹是存放用户资源的地方。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
把CNAME文件放到这里是最合适了，每次发布hexo都会从这里拷贝到public，然后发布到gh-pages。
##### CNAME内容如下
```
www.samkee.me
```
这样就大功告成了：www.samkee.me能解析到pengsamkee.github.io同时pengsamkee.github.io也能跳转到www.samkee.me。
## 结尾
当以上步骤都操作完成之后，发现还是无法对你的域名进行解析的话，可能是因为域名商还没真正把你域名解析了，需要等待较短的时候方可生效，不过一般都会很快。