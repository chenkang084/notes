---
title: vscode中使用印象笔记
tags: vscode markdown
notebook: blogs
---

# vscode中使用markdown

- **vscode** 是微软推出一款轻量级的文本编辑工具，类似于sublime，由于其拥有丰富的插件，安装使用也非常简单，所以深受广大程序员的喜爱。
- **markdown** 是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
- **印象笔记** 是一款具有笔记、写作、日程管理等诸多功能为一体的强大、易用型笔记本，拥有非常丰富的插件，其中比较好用的有**印象笔记·剪藏**，**马克飞象**，具体如何使用，请自行查阅相关资料。

markdown有许多衍生产品，本文主要针对大家比较熟知的**印象笔记**为例，说明如何在vscode中使用*markdown语法编辑文章，同时将文章同步到印象笔记中*。

## 安装配置步骤：

### 1.准备工作。
本机已安装vscode、印象笔记或者国际版Evernote，如果没有安装请自行安装以上软件。同时，您已经注册了印象笔记或者国际版Evernote的账号。
### 2.安装插件。
在vscode中安装**EverMonkey**，**Auto-Open Markdown Preview**。
> vscode安装插件可以从左侧的工具栏中选择`Extensions`，或者使用快捷键`Ctrl+Shift+X`。EverMonkey插件的作用是将vscode中的文件同步到印象笔记或者国际版Evernote。Auto-Open Markdown Preview插件的作用是在vscode中，当你打开*.md格式的文件时，自动开启预览，方便你在编辑的过程中实时的看到结果。
![Auto-Open Markdown Preview 预览效果](https://raw.githubusercontent.com/chenkang084/notes/master/imgs/blogs/vscode-1.png)
### 3.EverMonkey配置说明。
EverMonkey插件是本文的**重点**，该插件主要负责将vscode中的文章同步到印象笔记。在vscode中安装完EverMonkey插件后，我们安装官方说明，来一步一步的配置。
- a.获取token、noteStoreUrl。快捷键`Ctrl+Shift+P`，打开command，输入`ever token`，这里我使用的是国际版Evernote，所以我选择的是International。
![Auto-Open Markdown Preview 预览效果](https://raw.githubusercontent.com/chenkang084/notes/master/imgs/blogs/vscode-2.gif)
- b.输入你的印象笔记的账号密码，然后授权，就可以看到`token、noteStoreUrl`。
- c.将 `token、noteStoreUrl`配置到vscode的用户设置里面。步骤为`File --> Preferences --> Settings`，左边是系统默认设置，右边是用户自定义设置，在右侧配置token、noteStoreUrl，按照标准的json格式输入，key和value都需要加英文的双引号。
> evermonkey.token: your developer token<br>
  evermonkey.noteStoreUrl: your API url
- d.完成以上步骤，基本就算ok了，建议重启一下vscode，然后输入快捷键`Ctrl+Shift+P`打开command,输入`ever sync`，左下角显示synchronize successfully!(第一次同步可能有点慢，请耐心等待一下)，代表EverMonkey插件已经和印象笔记通信成功！如果报错，请去[git issue](https://github.com/michalyao/evermonkey/issues) 上面先找是否已经有人提过该问题，如果没有，你可以开个issue给作者。一般你遇到的问题，很多人也遇到了，请在close 里面仔细查找。
- e.上传vscode本地文件。新建本地文件，后缀为.md。在文件内容的最上方输入一下内容。
```javascript
---
  title: 文件名称
  tags: 标签（多个标签用逗号分隔）
  notebook: （所属的目录）
---
//下面是正文内容
...
  # xxx
```
完成文章内容编写之后，输入`Ctrl+Shift+P`打开command,输入`ever publish`,提示成功后，就可以在印象笔记客户端看到文章加入到了指定的目录里（如果客户端没有自动更新，请尝试手动更新）。
> 重要提示：如果报`Evernote Error: 5 - Note.title`，错误（这个错误坑了好一会）。说明是换行符有问题，请将vscode右下角的换行符从`CRLF`切换成`LF`,然后再次执行`ever publish`，就会有`blogs>>vscode中使用印象笔记 created successfully.`提示。如果还有错误，请到[git issue](https://github.com/michalyao/evermonkey/issues)查找相关问题。
![Auto-Open Markdown Preview 预览效果](https://raw.githubusercontent.com/chenkang084/notes/master/imgs/blogs/vscode-3.png)

## 总结：
马克飞象是一款非常好用的印象笔记插件，完全可以胜任以上所有功能，而且界面非常华丽，使用也很简单，但是，**要收费。。。** 之前用sublime的时候，就使用过相关插件，在sublime 里面实现编辑文章，同步到印象笔记。现在使用vscode比较多，就试试看有没有相关的功能，结果是肯定的（vscode这么优秀，怎能没有呢）。所以就尝试了一下，但是，遇到了个坑，就是`Evernote Error: 5 - Note.title`，查了半天说是`CRLF`切换成`LF`，我也确实在用户配置里面设置了，但是，就是不生效。最后，无意间发现，右下角显示的还是`CRLF`,然后尝试着点了一下，竟然可以改为`LF`,再然后呢，就没有然后了，OK了。<br>

虽然费了一大会功夫，但是还是有收获的，就是，这个不要钱。。。<br>

还有一个功能是马克飞象、印象笔记做不到的，印象笔记的内容保存在哪里我们并不知道，如果你想将这些内容同步到git上面，貌似是没有办法的。而使用vscode写*.md，文件内容都是在你指定的目录中，你可以随时将这些文件add,commit,push到git上面，是不是很方便。。。

> ps:这篇文章我就是用上述方法编辑的，非常方便，而且我直接push到我的[git仓库了](https://github.com/chenkang084/notes/blob/master/blogs/vscode%E4%B8%AD%E4%BD%BF%E7%94%A8markdown.md)。在git页面，直接复制该dom的html源码，贴到博客园就可以自动呈现格式化后的样子（虽然代码块、区域文本显示不太正常），还是很方便。

### 本文原创，欢迎转载，但请指明出处。如有错误，欢迎指正。