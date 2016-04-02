title: "brew指令总结"
date: 2015-11-16 16:13:46
tags: [mac,brew]
categories: [mac,shell]
---


* 查看brew的帮助

`brew –help`

* 安装软件【git】

`brew install git`

* 卸载软件【git】

`brew uninstall git`

* 搜索软件【git】

`brew search git`

<!--more-->

* 显示已经安装软件列表

`brew list`

* 更新软件，把所有的Formula目录更新，并且会对本机已经安装并有更新的软件用*标明。

`brew update`

* 更新某具体软件【git】

`brew upgrade git`

* 查看软件信息

`brew [info | home] [FORMULA...]`

* 删除程序，和upgrade一样，单个软件删除和所有程序老版删除。

```
brew cleanup git 
brew cleanup
```
* 查看那些已安装的程序需要更新

`brew outdated`




* 其它Homebrew指令:

```
brew list   —列出已安装的软件

brew update   —更新Homebrew

brew home  *—用浏览器打开

brew info   *—显示软件内容信息

brew deps * — 显示包依赖

brew server *  —启动web服务器，可以通过浏览器访问http://localhost:4567/ 来同网页来管理包

brew -h brew   —帮助
```
