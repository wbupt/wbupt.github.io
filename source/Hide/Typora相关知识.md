---
title: 'Typora 相关设置'
date: 2023-03-14 10:51:40
tags: [Tpyora, Markdown]
published: true
hideInList: false
hidden: true
hide: false
feature: 
isTop: true
---


[Typora更新问题](https://blog.csdn.net/yyywxk/article/details/125133205)

# 2022.7.16 更新

今天又不行了，只能安装一个老版本试试[4](https://blog.csdn.net/yyywxk/article/details/125133205#fn4)：[下载地址【提取密码：c1se】](https://pan.baidu.com/s/1DE1onEjoGYyGDUnXHI8BvQ?pwd=c1se)

2022.7.24 更新
经网友评论提醒，又提供了一个新方案。

方案四：修改 Typora 相应注册表的权限56 (详细步骤见参考博文，作为旧版本失效后的一种补充)：
打开注册表：按Windows+R打开运行窗口，输入 regedit。
进入路径计算机\HKEY_CURRENT_USER\SOFTWARE\Typora。
在右键菜单选择权限，把各个用户的权限全部设置为 拒绝。
————————————————
版权声明：本文为CSDN博主「yyywxk」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yyywxk/article/details/125133205

`操作`

`获取SESSDATA: 登录哔哩哔哩→F12打开控制台→Application→Cookies→SESSDATA`

`获取csrf: 登录哔哩哔哩→F12打开控制台→Application→Cookies→bili_jct`



```python
D:\Downloads\typora-plugin-bilibili-win.exe token=你的SESSDATA csrf=你的bili_jct
```

```python
‪C:\Windows\main.exe token=9796b94a%2C1694331119%2Cc7a4d%2A32 csrf=f780fb4a980d7203f0e61bba0cf34fd1
```

```python
‪C:\Windows\main.exe token=d63e6f5b%2C1694331008%2C28318%2A32 csrf=e54ac5c6cd02e9f968054d3a8402160f
```

```python
‪C:\Windows\main.exe token=86c56aea%2C1694329649%2Ce15ee%2A32 csrf=c4728b0aa2fe78fed8b3a55e5de82795
```

```python
‪C:\Windows\main.exe token=86c56aea%2C1694329649%2Ce15ee%2A32 csrf=c4728b0aa2fe78fed8b3a55e5de82795
```



‪C:\Program Files\Typora\main.exe





## typora-plugin-bilibili

哔哩哔哩图片上传, Typora插件，实现图片粘贴即可上传到哔哩哔哩，并替换链接

### 重要提示

**由于B站相簿的上传API自身出现问题，现在切换到动态的图片API，因此需要多加一个参数csrf(为Cookie里面的bili_jct)**

示例

```
插件客户端路径 token=0829d25Cdd19b*b1 csrf=cb397c0fbf619237
```

### 用Go重写，产物缩小5倍体积，点击下载即可





# B站图床、短链(Firefox、Chrome、Edge)

哔哩哔哩图床插件，速度快,多种图片压缩格式选择，自动读取Bilibili的Cookie，不再需要手动输入。 基于[vitesse-webext](https://github.com/xlzy520/vitesse-webext) 重构

### 在线安装

[Chrome、Edge](https://chrome.google.com/webstore/detail/b站图床/domljbndjbjgpkhdbmfgmiclggdfojnd?hl=zh-CN)

[Firefox](https://addons.mozilla.org/addon/哔哩哔哩图床/)

### 本地安装

[下载](https://jiali0126.oss-cn-shenzhen.aliyuncs.com/share/extension.zip)

### 安装步骤

1. 进入`拓展程序`,可以通过地址栏输入`chrome://extensions/`，也可以从 `更多工具`->`拓展程序`进入
2. 右上角开启`开发者模式`
3. 左侧点击 `加载已解压的拓展程序`,然后选择上面下载好的压缩包解压后的文件夹即可。

### 本地开发(支持热更新)

1. 执行`npm i`或者`pnpm i`, 执行`npm run dev`或`pnpm run dev`
2. 上一步(安装步骤)将文件夹选择为`extension`文件夹

### 构建

执行`npm run build`或`pnpm run build`

### 截屏

