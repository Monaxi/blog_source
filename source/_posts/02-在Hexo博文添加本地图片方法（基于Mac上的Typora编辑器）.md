---
title: 在Hexo博文添加本地图片方法（基于Mac上的Typora编辑器）
date: 2022-05-12 11:36:33
tags:
  - Hexo
cover: https://github.com/Monaxi/Mona-study/blob/main/blog/%E9%BB%98%E8%AE%A4%E5%9B%BE%E7%89%87.png?raw=true
top_img: https://github.com/Monaxi/Mona-study/blob/main/blog/cover1.png?raw=true
---

参考了：[Hyemin Kim’s blog](https://hyemin-kim.github.io/2020/05/12/Hexo-Insert-local-images/#%E7%BC%96%E5%86%99%E5%8D%9A%E5%AE%A2%E5%89%8D-%E8%BF%9B%E8%A1%8C%E9%85%8D%E7%BD%AE)

​		在Markdown文档中添加网络图片时，可以使用命令`!['图片名称'](图片网络地址)`实现，

​		在Markdown文档中添加本地图片，可以进行以下设置

### 第一步 配置

1. 建立<font color=ligblue>**资源文件夹(Asset Floder)**</font>，用来保存添加到博文中的本地图片

​		在本地Hexo根目录下的source文件夹中创建一个名为 images 的文件夹

2. 在Typora中设置图片的相对路径

​		打开Typora的偏好设置 > 图像，进行如下设置：

![01-设置图片相对路径](../images/%E5%9C%A8Hexo%E5%8D%9A%E6%96%87%E6%B7%BB%E5%8A%A0%E6%9C%AC%E5%9C%B0%E5%9B%BE%E7%89%87%E6%96%B9%E6%B3%95%EF%BC%88%E5%9F%BA%E4%BA%8EMac%E4%B8%8A%E7%9A%84Typora%E7%BC%96%E8%BE%91%E5%99%A8%EF%BC%89/01-%E8%AE%BE%E7%BD%AE%E5%9B%BE%E7%89%87%E7%9B%B8%E5%AF%B9%E8%B7%AF%E5%BE%84.png)

​		此设置会使source/images文件夹下新增一个与所编辑的markdown文档同名的文件夹，文档中所添加的 本地图片 都将存档于此（即拥有了如下路径：'hexo根目录'/source/images/'md文档名'/'图片名称'）)。

​		撰写markdown文档时配置 图片根目录 ，使其能够同步到hexo博客中去

​		撰写博文时，先点击Typora菜单栏中的格式 —> 图像 —> 设置图片根目录 , 将根目录配置为'hexo根目录'/source。然后再撰写博文。【注：每篇需要添加本地图片的博文都要先进行此步骤】

### 第二步 图片导入方法

#### 方法一 直接拖拽

​		将原本存放于其他本地文件夹中的图片直接拖拽到文档中的相应位置中去

​		此时图片会被自动存档至生成的同名文件夹'hexo根目录'/source/images/'md文档名'中

​		文档中图片地址的代码会显示成 自动生成的相对路径，即/images/'md文档名'/'图片名称'

#### 方法二 利用相对路径调取

​		当利用方法一插入了至少一张图片时（即已生成同名文件夹时），便可以把接下来要插入的图片复制到此同名文件夹中，在文档中利用相对路径调取图片。
​		所使用的命令是：`![图片显示名称](/images/'md文档名'/'图片名称')`

<font color=ligblue>**NOTE：**</font>

1. 这里的图片显示名称不必与文件夹中保存的图片名称保持一致，'图片名称'中要记得包含图片格式（例如：tupian.jpg 或 picture.png 等)

2. 当还没有利用方法1插入过图片时（即同名文件夹尚未生成时），不可以自己创建同名文件夹保存图片。亲测不好使！！（.md文档中可以显示，但是hexo博文中无法显示）

### 第三步 图片存档结果

​		在利用上述方法完成了含有本地图片的markdown博文后，我们的资源文件夹'hexo根目录'/source/images/内最终会显示成什么样子呢？

​		每一篇配置了图片根目录的博文，都会在'hexo根目录'/source/images/文件夹中有一个与文档名称同名的文件夹'hexo根目录'/source/images/'md文档名'。

​		该文件夹中会保存博文编写中曾经添加的所有本地图片。

​		所有的含义是：即使编辑过程中某些本地图片在添加后又被删除了，它们也仍然会保留在文件夹中，即该文件夹会备份你在博文中添加的 所有本地图片历史
​		本地图片的含义是：这里只会保存插入的本地图片，而不会保存插入的网络图片。

