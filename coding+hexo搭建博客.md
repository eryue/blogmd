---
title: coding+hexo搭建博客
date: 2016-11-11
categories: technology
tags: 博客
---


github国内速度比较慢，改用coding。主题改用maupassant-hexo

<!--more-->

### 创建Coding Pages

``` bash
mkdir eryue  
cd eryue  
git init  
echo "# eryue" >> README.md  
git add README.md  
git commit -m "first commit"    
git remote add origin https://git.coding.net/eryue/eryue.git    
git push -u origin master   
git checkout -b coding-pages    
git push origin coding-pages    
```

### 安装git

### 安装Node.js

### 安装Hexo
国内的速度... 所以使用淘宝npm镜像https://npm.taobao.org/，

``` bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

安装hexo

``` bash
cnpm install -g hexo-cli
```

### 建站

``` bash
hexo init <folder>
cd <folder>
cnpm install
```

### 安装主题

``` bash
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
cnpm install hexo-renderer-jade --save
cnpm install hexo-renderer-sass --save
cnpm install hexo-deployer-git --save
cnpm install hexo-generator-search --save
```

### 修改配置

#### 修改Hexo配置文件_config.yml， 

language: zh-CN 
url: http://eryue.coding.me 
theme: maupassant   
deploy: 
    type: git   
    repo: https://git.coding.net/eryue/eryue.git    
    branch: coding-pages    

#### 修改主题配置文件_config.yml 

站内搜索    
self_search: true   
多说    
duoshuo：lizhangcheng   
链接    
links:  
  - title: Stack Overflow   
    url: http://www.stackoverflow.com/  
  - title: Cocos    
    url: http://www.cocos.com/  
  - title: 廖雪峰   
    url: http://www.liaoxuefeng.com/    
  - title: 云风的blog   
    url: http://blog.codingnow.com/ 
  - title: maupassant-hexo  
    url: https://www.haomwei.com/technology/maupassant-hexo.html    

####  添加about
在根目录的source目录下建立about文件夹，然后在文件夹中建立index.md文件，并在index.md的front-matter中设置layout为layout: page。
若需要单栏页面，就将layout设置为 layout: single-column

#### 添加Favicon
将favicon.ico放到根目录的source目录下

#### 文章配置
title: 标题 
date: 2016-04-05 14:16:00   
categories: technology  
tags:   
目录    
toc: true   
评论    
comments: true 

#### 文章摘要  
  
description:来设置想显示的摘要，或者直接在文章内容中插入<!--more-->以隐藏后面的内容。
若两者都未设置，则自动截取文章第一段作为摘要

### 生成静态页面

``` bash
hexo g
```

### 部署到coding

``` bash
hexo d
```

