title: "学习shell script"
date: 2015-4-20 10:03:20
tags: [linux,shell,script]
categories: [shell]
---

1.为什么要学习shell script？

如果你是真的想要玩清楚 Linux 的来龙去脉， 那么 shell script 就不可不知，为什么呢？因为：

#自动化管理的重要依据：

不用鸟哥说你也知道，管理一部主机真不是件简单的事情，每天要进行的任务就有： 查询登录档、追踪流量、监控使用者使用主机状态、主机各项硬件设备状态、 主机软件升级查询、更不要说得应付其他使用者的突然要求了。而这些工作的进行可以分为： (1)自行手动处理，或是 (2)写个简单的程序来帮你每日自动处理分析这两种方式，你觉得哪种方式比较好？ 当然是让系统自动工作比较好，对吧！呵呵～这就得要良好的 shell script 来帮忙的啦！

<!--more-->

#追踪与管理系统的重要工作：

虽然我们还没有提到服务启动的方法，不过，这里可以先提一下，我们 Linux 系统的服务 (services) 启动的介面是在 /etc/init.d/ 这个目录下，目录下的所有文件都是 scripts ； 另外，包括启动 (booting) 过程也都是利用 shell script 来帮忙搜寻系统的相关配置数据， 然后再代入各个服务的配置参数啊！举例来说，如果我们想要重新启动系统登录档， 可以使用：『/etc/init.d/syslogd restart』，那个 syslogd 文件就是 script 啦！

另外，鸟哥曾经在某一代的 Fedora 上面发现，启动 MySQL 这个数据库服务时，确实是可以启动的， 但是萤幕上却老是出现『failure』！后来才发现，原来是启动 MySQL 那个 script 会主动的以『空的密码』去尝试登陆 MySQL ，但为了安全性鸟哥修改过 MySQL 的密码罗～当然就登陆失败～ 后来改了改 script ，就略去这个问题啦！如此说来， script 确实是需要学习的啊！



#简单入侵侦测功能：

当我们的系统有异状时，大多会将这些异状记录在系统记录器，也就是我们常提到的『系统登录档』， 那么我们可以在固定的几分钟内主动的去分析系统登录档，若察觉有问题，就立刻通报管理员， 或者是立刻加强防火墙的配置守则，如此一来，你的主机可就能够达到『自我保护』的聪明学习功能啦～ 举例来说，我们可以通过 shell script 去分析『当该封包尝试几次还是连线失败之后，就予以抵挡住该 IP』之类的举动！

#连续命令单一化：

其实，对於新手而言， script 最简单的功能就是：『汇整一些在 command line 下达的连续命令，将他写入 scripts 当中，而由直接运行 scripts 来启动一连串的 command line 命令输入！』例如： 防火墙连续守则 (iptables)，启动加载程序的项目 (就是在 /etc/rc.d/rc.local 里头的数据) ，等等都是相似的功能啦！ 其实，说穿了，如果不考虑 program 的部分，那么 scripts 也可以想成『仅是帮我们把一大串的命令汇整在一个文件里面， 而直接运行该文件就可以运行那一串又臭又长的命令段！』就是这么简单啦！

#简易的数据处理：

你可以发现， awk 可以用来处理简单的数据数据呢！例如薪资单的处理啊等等的。 shell script 的功能更强大，例如鸟哥曾经用 shell script 直接处理数据数据的比对啊， 文字数据的处理啊等等的，撰写方便，速度又快(因为在 Linux 效能较佳)，真的是很不错用的啦！

#跨平台支持与学习历程较短：

几乎所有的 Unix Like 上面都可以跑 shell script ，连 MS Windows 系列也有相关的 script 模拟器可以用， 此外， shell script 的语法是相当亲和的，看都看的懂得文字 (虽然是英文)，而不是机器码， 很容易学习～这些都是你可以加以考量的学习点啊！
上面这些都是你考虑学习 shell script 的特点～此外， shell script 还可以简单的以 vim 来直接编写，实在是很方便的好东西！所以，还是建议你学习一下啦。

不过，虽然 shell script 号称是程序 (program) ，但实际上， shell script 处理数据的速度上是不太够的。因为 shell script 用的是外部的命令与 bash shell 的一些默认工具，所以，他常常会去呼叫外部的函式库，因此，运算速度上面当然比不上传统的程序语言。 所以罗， shell script 用在系统管理上面是很好的一项工具，但是用在处理大量数值运算上， 就不够好了，因为 Shell scripts 的速度较慢，且使用的 CPU 资源较多，造成主机资源的分配不良。还好， 我们通常利用 shell script 来处理服务器的侦测，倒是没有进行大量运算的需求啊！所以不必担心的啦！










		2.

		撰写 shell script 的良好习惯创建
		script 的功能；
		script 的版本资讯；
		script 的作者与联络方式；
		script 的版权宣告方式；
		script 的 History (历史纪录)；
		script 内较特殊的命令，使用『绝对路径』的方式来下达；
		script 运行时需要的环境变量预先宣告与配置。

3. 我的练习程序

			@1
			#!/bin/bash
			# Program:
			#       This program shows "Hello World!" in your screen and "duang!" de voice!
			# History:
			# 2015/04/17    bruce   First release
			# 使用 exit 0 ，这代表离开 script 并且回传一个 0 给系统， 所以我运行完这个 script 后，若接著下达 echo $? 则可得到 0 的值喔！
			#  echo 必须要加上 -e 的选项才行 会看到萤幕是这样，而且应该还会听到『咚』的一声


			PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
			export PATH
			echo -e  "Hello World! \a \n"
			exit 0

---------------------------------------------------------------------------------------------------------------------------------------------------------
			@2 
			#!/bin/bash

			# Programe

			# 功能描述：请你以 read 命令的用途，撰写一个 script ，他可以让使用者输入：1. first name 与 2. last name， 最后并且在萤幕上显示：『Your full name is: 』的内容

			# History:

			# 时间:2015-4-17        作者：bruce     版本:version 1

			PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin

			export PATH

			read -p "please input your first name within 30 s:  " -t 30 first
			read -p "please input your last name within 30 s: " -t 30 last
			echo -e "\n your full name is:$first $last"
			exit 0
---------------------------------------------------------------------------------------------------------------------------------------------------------
			@3:

			#!/bin/bash

			# Program:

			# 功能描述:假设我想要创建三个空的文件 (透过 touch) ，档名最开头由使用者输入决定，假设使用者输入 filename 好了，那今天的日期是 2009/02/14 ， 我想要以前天、昨天、今天的日期来创建>这些文件，亦即 filename_20090212, filename_20090213, filename_20090214 ，该如何是好？


			# History:

			# 时间  作者:bruce 版本:version 1

			PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin

			export PATH

			#第一步：让输入者输入文件名
			read -p " please write your filename: "  fileuser

			#第二步：判断用户的是否随便输入enter键利用变量功能分析档名是否有配置？
			filename=${fileuser:-"filename"}

			#第三步：利用date取得时间
			date1=$(date --date='2 days ago' +%Y%m%d)
			date2=$(date --date='1 days ago' +%Y%m%d)
			date3=$(date +%Y%m%d)
			file1=${filename}${date1}
			file2=${filename}${date2}
			file3=${filename}${date3}

			#@4 :创建文档
			touch "$file1"
			touch "$file2"
			touch "$file3"

			echo -e "已经创建了3个文档"

			exit 0

--------------------------------------------------------------------------------------------------------------------------------------------------------- 
			sh 运行shell脚本和source运行shell脚本的区别
			sh 运行的脚本的变量是局部变量，source运行的脚本的变量是全局变量。
			sh运行完以后，在bash最父级echo调用不到子bash的定义变量
			source运行完以后，在bash最父级echo可以调用bash的定义变量
			利用 source 来运行脚本：在父程序中运行
			如果你使用 source 来运行命令那就不一样了！同样的脚本我们来运行看看：

			[root@www scripts]# source sh02.sh

			Please input your first name: VBird

			Please input your last name:  Tsai



			Your full name is: VBird Tsai

			[root@www scripts]# echo $firstname $lastname

			VBird Tsai  <==嘿嘿！有数据产生喔！


<!-- 未写完 -->


              
 
































