---
title: Hexo个人博客搭建
tags:
  - hexo
top: 2
category: 学习
abbrlink: hexo_build
---
---
# Hexo个人博客搭建



---

1. 安装nodejs、git

首先、需要安装 Node.js(http://nodejs.cn/download/) 和 Git(https://git-scm.com/downloads)，大家尽量去下载.exe扩展名的可执行文件，这样的好处是一键安装、省去了一些配置。安装版本也可以安装最新的版本。如果有问题卸载安装旧版。 

`验证安装是否成功`，打开CMD



```python
验证Node.js的方法
node -v
npm -v
输入后能够显示版本说明安装成功
```



1. 在文件夹中右击 `Git Bash Here` 

2. 使用npm命令安装Hexo，输入：

   ```python
   npm install -g hexo-cli # 安装hexo可略过
   hexo init
   hexo clean # 清除标签
   hexo new 我的新博客
   hexo g #生成
   hexo s #启动服务预览
   git config --global user.name "wbupt"
   git config --global user.email "34@qq.com"
   npm install hexo-deployer-git --save # 安装上传文件
   npm install hexo-renderer-pug hexo-renderer-stylus # 安装 butterfly插件
   npm install hexo-generator-index-pin-top --save # 文章置顶插件
   npm install hexo-hide-posts --save # 隐藏文章
   npm install hexo-math --save # 安装公式
   npm install hexo-wordcount --save # 安装字数统计
   npm install hexo-tag-dplayer --save # 播放本地视频
   hexo d #部署
   ```
   
   `hexo 插件`
   
   ```python
   https://hexo.io/zh-cn/
   ```
   
   
   
   
   

## 配置文件设置：

`_config.yml` 

```python
skip_render: #跳过文件
  - "HTML/*"
  - "about/*"
banner_img: ""
```

```python
---
title: Hexo个人博客搭建
tags: [标签,标签2,标签3] # 标题
category: 沧州植物鱼		# 分类
isTop: true
abbrlink: 20929	# 文章地址
date: 2023-03-14 10:19:08	# 写作时间
feature:
sticky: 101
top: 101
---
```



## Hexo Github 添加网站

[使用GitHub Pages制作个人博客及自定义域名设置 - 半亩方塘](https://www.bluepost.cn/jianzhan/github-pages-custom-domain-site.html)

```python
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

![image-20250918191456377](https://s2.loli.net/2025/09/18/KfPTrROeLtSUXZA.png)


