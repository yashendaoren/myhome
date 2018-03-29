---
layout: post
title: 如何使用Github搭建自己的静态网站
date: 2016-3-24
category: [web开发,IT其他]

---

### 首先登陆自己的Github账号 如果没有 注册一个 无需多说
![图片]({{ site.url }}/assets/images/github-page/01.png)

### 创建仓库 用于存放自己的页面代码
![图片]({{ site.url }}/assets/images/github-page/02.png)

### 切换到Settings页面
![图片]({{ site.url }}/assets/images/github-page/03.png)

### 往下拉 会看到一个选择主题的选项按钮
![图片]({{ site.url }}/assets/images/github-page/04.png)

### 选择一个自己喜欢的样式 点击选择
![图片]({{ site.url }}/assets/images/github-page/05.png)

### 上传自己选中的主题
![图片]({{ site.url }}/assets/images/github-page/06.png)

### 再切换到Settings页面 往下拉
![图片]({{ site.url }}/assets/images/github-page/07.png)

### 此时 Github 已经帮我们生成了自己网页的地址 点开看看！
![图片]({{ site.url }}/assets/images/github-page/08.png)

### 再看我们的仓库里面
我们的仓库里面也已经有了配置和第一个页面的文件（index.md）以后的每个博文 都会是一个.md文件 index.md里面有如何使用的介绍 功能不算丰富 但是只是写个博客 也足够了
![图片]({{ site.url }}/assets/images/github-page/09.png)

### 切换样式
如果自己不喜欢原本的模版，可以自己写，Github支持.html文件 支持js特效。当然，如果觉得自己的设计不够好，也可以去下载别的模版，比如楼主的模版，就是在[Jekyll](https://jekyllrb.com)下载的。对于[Jekyll](https://jekyllrb.com)的使用，里面有很详细的教程。

### 更换域名
如果想换成自己的域名，也很简单
<br/>
1，申请一个域名
<br/>
2，解析域名（选择CNAME方式解析，地址选择Github分配给我们的 “二级域名.github.io”）
<br/>
3，创建一个CNAME文件，里面写上自己的域名就行了（注意：CNAME字母要大写，没有后缀）
<br/>
4，上传到自己刚开始创建的仓库

### 搞定了 试试自己的网站吧！！









