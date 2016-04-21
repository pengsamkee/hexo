---
title: 人靠衣装，美靠靓装，为你的Hexo Blog换个主题吧
date: 2016-04-19 18:50:59
categories:
- technology
- hexo
tags: ['Hexo', 'Theme']
---
最近使用 Hexo 搭建了一个博客，但是发现 Hexo 的默认主题还是太丑了，不忍直视，所以去 [Hexo Theme](https://hexo.io/themes/) 预览了很多别人做的优秀主题。
发现自己比较喜欢 [Minos] 这个主题，所以朕今天就翻它的牌了，为此记录下具体更换步骤。
拉到下面一点，找到 Minos 。
![](/images/hexoTheme.png)
按照图片指示①，点击可以进入网站预览主题效果：
![](/images/minosTheme.png)
很漂亮是不是，我已经爱上这个主题了，233333。
## 更换步骤
![](/images/hexoTheme.png)
按照图片指示②，点击可以进入 Minos 主题的GitHub仓库，[传送门](https://github.com/ppoffice/hexo-theme-minos)：
查看README.md文件，找到安装引导：

### Installation
#### Install
```
git clone https://github.com/ppoffice/hexo-theme-minos.git themes/minos
```
##### Minos requires Hexo 3.0 and above.
Enable
Modify theme setting in _config.yml to minos.
Update
```
cd themes/minos
git pull
```
### Configuration
#### Theme configuration example
```
# Header
menu:
  Home: /
  Archives: archives
  Categories: categories
  Tags: tags
  About: about

# Content
excerpt_link: Read More
toc: false
fancybox: true

# Miscellaneous
google_analytics:
favicon: /favicon.png
```
+ excerpt_link - Cooperate with <!-- more --> tag to show only part of the article in index pages.
+ toc - Whether to show the table of contents in articles. 
+ fancybox - Enable Fancybox. 
+ google_analytics - Google Analytics ID. 
+ favicon - Favicon path. 

**Don't forget to rename _config.yml.example to _config.yml to enable theme config!**
### Custom Categories & Tags Pages
To enable custom categories page and tags page, just copy the categories folder and tags folder under your theme's _source foler into your site's source folder. Then edit theme's _config.yml and add the following lines:
```
# Header
menu:
  ...
  Categories: categories # -> add this line
  Tags: tags # -> and add this line
  ...
```
### Languages
English and Simplified Chinese are the default languages of the theme. You can add translations in the languages folder and change the default language in site's _config.yml.
```
language: zh-CN
```
### Features
#### Custom Categories & Tags Pages
Get your categories and tags listed in single pages to make your blog more methodic.
#### Responsive Layout
Minos knows on what screen size you are browsering the website, and reorganize the layout to fit your device.