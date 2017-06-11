---
title: vscode中使用印象笔记
tags: vscode markdown
notebook: blogs
---

# vscode中使用markdown

- **vscode** 是微软推出一款轻量级的文本编辑工具，类似于sublime，由于其拥有丰富的插件，安装使用也非常简单，所以深受广大程序员的喜爱。
- **markdown** 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。

markdown有许多衍生产品，本文主要针对大家比较熟知的**印象笔记**为例，说明如何在vscode中使用*markdown语法编辑文章，同时将文章同步到印象笔记中*。

## 安装配置步骤：

### 1.准备工作。
本机已安装vscode、印象笔记或者国际版Evernote，如果没有安装请自行安装以上软件。同时，您已经注册了印象笔记或者国际版Evernote的账号。
### 2.安装插件。
在vscode中安装**EverMonkey**，**Auto-Open Markdown Preview**。
> vscode安装插件可以从左侧的工具栏中选择`Extensions`，或者使用快捷键`Ctrl+Shift+X`。EverMonkey插件的作用是将vscode中的文件同步到印象笔记或者国际版Evernote。Auto-Open Markdown Preview插件的作用是在vscode中，当你打开*.md格式的文件时，自动开启预览，方便你在编辑的过程中实时的看到结果。
![Auto-Open Markdown Preview 预览效果](https://raw.githubusercontent.com/chenkang084/notes/master/imgs/vscode-1.png)
### 3.EverMonkey配置说明。
EverMonkey插件是本文的**重点**，该插件主要负责将vscode中的文章同步到印象笔记。在vscode中安装完EverMonkey插件后，我们安装官方说明，来一步一步的配置。
- a.获取token、noteStoreUrl。快捷键`Ctrl+Shift+P`，打开command，输入`ever token`。
![Auto-Open Markdown Preview 预览效果](https://raw.githubusercontent.com/chenkang084/notes/master/imgs/vscode-2.gif)


evermonkey.token: your developer token
evermonkey.noteStoreUrl: your API url