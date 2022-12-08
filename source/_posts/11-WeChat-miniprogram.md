---
title: 11-WeChat_miniprogram
tags:
  - null
  - null
cover: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/wallhaven-wqgr9x_1920x1080.png?raw=true
top_img: >-
  https://github.com/Monaxi/Mona-study/blob/main/blog/default_top_img.png?raw=true
date: 2022-07-28 11:50:44
description:
---

# 小程序代码构成

## 包含文件类型

1. `.json`后缀的`JSON`**配置文件**
2. `.wxml` 后缀的 `WXML` **模板文件**
3. `.wxss` 后缀的 `WXSS` **样式文件**
4. `.js` 后缀的 `JS` **脚本逻辑文件**

### JSON配置

JSON 是一种数据格式，并不是编程语言，在小程序中，JSON扮演的静态配置的角色。

我们可以看到在项目的根目录有一个 `app.json` 和 `project.config.json`，此外在 `pages/logs` 目录下还有一个 `logs.json`，我们依次来说明一下它们的用途。

#### 小程序配置app.json

`app.json` 是当前小程序的全局配置，包括了小程序的所有页面路径、界面表现、网络超时时间、底部 tab 等。

QuickStart 项目里边的 `app.json` 配置内容如下：

```json
{
  "pages":[
    "pages/index/index",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "Weixin",
    "navigationBarTextStyle":"black"
  }
}
```

其中

1. `pages`字段 —— 用于描述当前小程序所有页面路径，这是为了让微信客户端知道当前你的小程序页面定义在哪个目录。
2. `window`字段 —— 定义小程序所有页面的顶部背景颜色，文字颜色定义等。
