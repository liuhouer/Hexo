title: "论git的好处"
date: 2015-12-24 17:26:57
tags: [git,代码同步]
categories: git
---

linus威武? .好吧,其实我也不知道.真正扎实在用git也就这几个月.之前都是svn.曾经用过cvs.

git给我带来的好处:

##- 本地的版本管理

不需要远程或架设服务器就能做到本地版本管理.

不污染子目录的track文件

svn每个子目录都要扔一个.svn.这个实在是.. .(我想很多人都碰到过svn lock folder的情况.实在让人气急败坏.实际上.svn文件就是罪魁祸首.各种clean up无果. delelte后svn up异常.真是.. 摔!.去你妹的svn.)

##- 强大的branch

超轻量级的branch建立.(实际只是建立文件指针).推荐根据的git workflow的开发流程.将workspace分成几区.master dev feature hotfix区等.开发层次就出来了.

<!--more-->

##- merge工具的强力

git根据commit ticket依次再进行一次merge.提高了merge成功率.避免svn merge中的难堪.即使merge失败.也不会生成乱七八糟的版本文件.简单修改后.commit就是.

##- 神奇的git gc

由于git本身不保存文件之前的差异文件.只保存每个文件的快照.所以在频繁修改大文件的情况下会造成git目录变得肥大不堪.git早就有了解决方案.git gc后,会在.git目录下生成一个packfile与idx文件.只保存文件差异.满塞!.

##- 清爽实用的cli

嗯,我在离开"乌龟"之后是彻底就不会用svn了.看来应该还是内心在抵触.没有学习.

计算机世界所有的问题都可以通过添加一个间接层来实现

git确实增加了一层间接层,实现了去中心化scm工具.当然增加了一点学习的成本.初次接触可能不知道push跟pull的作用.set origin的意义.当然只在本地做管理的话,是基本没有所谓成本的.

##- github

github作为新一代的程序员靠实力,凭作品交流的sns+code host平台.将geek精神贯彻整站.cool!.相对而言google code则正在走下坡路.

可能还有一些好处或者弊端.没看到一个产品的弊端说明你没真正理解它?我可能也只是单纯的崇拜.这篇还有待继续编辑.待我有了更多的经验.或许这篇文章会变成"git为什么这么烂?"也说不定.

svn 是中央集权，没了服务器就都没了。git是分布式，没了服务器，自己本地也能查看历史记录，分支操作什么的。

---

cvs, git, svn都用过，以下完全是主观感受：
1. git比svn快，用起来更流畅。
2. git在本地就可以用，可以随便保存各种历史痕迹，不用担心污染服务器。svn commit就到服务器了，有时候发现commit错了或不全就得再来一遍，有review更吐血了。（git commit --amend太好用了）
3. git拉branch和在branch之间切换都非常简单，可以随便折腾。svn一个branch就是一个copy。
4. git绝对不会有被lock了不能commit的情况。。
5. svn老版本每个目录都有一个.svn目录，非常繁琐。（新版本svn也只在最上层有.svn目录了，不过仍然比较难用，不在.svn同目录下很多命令不给用，比如svn info）
------------------------------------





