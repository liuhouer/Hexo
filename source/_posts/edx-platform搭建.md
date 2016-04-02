title: "edx-platform搭建"
date: 2014/5/24 14:32:36
tags: [edx,mooc]
categories: [edx,mooc]
---

edx-platform搭建[Ubuntu 12.04.2 LTS]
1. Ubuntu下的默认软件包格式是deb，所以下载的Vagrant和VirtualBox最好是deb格式的，另外要根据自己电脑的硬件配置和系统环境选择正确的安装包。

PS：如果想要在虚拟机中安装Ubuntu12.04，然后再使用Vagrant方式搭建开发环境，请确保这个虚拟机可以使用2GB的内存，否则容易报一个IOError。上一篇EdX搭建教程结尾便是使用的虚拟机，感兴趣的可以看下里面的出错：

（http://hujiaweiyinger.diandian.com/post/2013-07-16/edx_build_edx_platform）

<!--more-->

参考网址：

（1）在Ubuntu中安装RPM格式软件包的方法：

http://my.oschina.net/renyuansoft/blog/28187

（2）在ubuntu中开启root用户并实现root登录问题的解决方法：

http://os.51cto.com/art/200709/56719.htm

（3）在ubuntu 12.04中设置root登录的办法：

http://blog.chinaunix.net/uid-26517277-id-3217839.html

（4）Linux中uname命令的使用详解：

http://baike.baidu.com/view/1374878.htm

 

2. 下载内容

（1）precise32.box  （提前下载好虚拟机）

网址：http://files.vagrantup.com/precise32.box

（2）vagrant  1.2.2  （虽然最新的是1.2.5了，但是还是推荐使用1.2.2）

网址：http://downloads.vagrantup.com/tags/v1.2.2

我选择的是 vagrant_1.2.2_i686.deb

（3）virtualbox  4.2.12  （千万不要使用最新的4.2.16，注意版本！）

网址：http://download.virtualbox.org/virtualbox/4.2.12/

或者是 https://www.virtualbox.org/wiki/Download_Old_Builds_4_2

需要两个：

① 对应系统的virtualbox安装包

   virtualbox-4.2_4.2.12-84980~Ubuntu~precise_i386.deb

② 扩展包 （虽然不知道是否有影响，但是建议安装）

  Oracle_VM_VirtualBox_Extension_Pack-4.2.12-84980.vbox-extpack
 

3. 准备操作

（1）以普通用户的身份登录Ubuntu

*** 最好不要使用 root 账户登录 ***

（2）安装git，运行 “ sudo apt-get install git ”

（3）安装nfs，运行 “ sudo apt-get install nfs-common nfs-kernel-server”

***  安装NFS，避免后面提示系统不支持NFS，然后才安装NFS，提前安装好  ***

（4）新建目录 edx，将下载的所有安装包放在 edx 目录下

（5）安装Vagrant和VirtualBox及其扩展，“ cd edx”-> " dpkg -i vagrant_xxx.deb" -> " dpkg -i virtualbox-xxx.deb"  -> 双击运行 extpack 文件安装扩展包

（6）clone repository，运行 "git clone git://github.com/edx/edx-platform.git "

*** 千万不要只是将别的电脑中的源代码或者通过其他用户登录下载得到的源代码拿过来使用，因为这样的话当前用户执行vagrant up很容易出现权限不够的问题！  ***

（7）完了之后进入edx-platform目录，修改以下三个地方：

① 找到 requirements目录下的 base.txt 中的 “polib = 1.0.3" ，把这一行注释掉或者删除；

② 找到 requirements目录下的 github.txt 中的 Third-party 项目，将 前面的 "git" 改成 "git+https"

例如：原来是 ”-e git://github.com/edx/django-staticfiles.git@6d2504e5c8#egg=django-staticfiles“

改成 ”-e git+https://github.com/edx/django-staticfiles.git@6d2504e5c8#egg=django-staticfiles“

③ 找到scripts目录下的create_dev_env.sh文件，找到

”pip install -r $BASE/edx-platform/requirements/edx/pre.txt“，在后面添加一行

”pip install http://bitbucket.org/izi/polib/get/1.0.3.tar.gz“

*** 原因：polib依赖项经过pypi的解析得到的下载地址是：http://bitbucket.org/izi/polib/downloads/polib-1.0.3.tar.gz，但是在天朝，这个地址上访问不上的，然而前面的downloads是可以的，polib是必须的依赖项，所以只能是使用变相的方式将其安装上去。详细内容可见我的这个问题（https://github.com/edx/edx-platform/issues/427） ***

（8）添加box，运行 "cd edx-platform" -> "vagrant box add precise32 ../precise32.box"

*** 之所以直接下载下来然后添加是为了防止网络不稳定出现故障而导致下载失败 ***

（9）开始运行"vagrant up"，一定要保证过程中网络稳定！

*** 如果需要输出更加详细的内容可以使用 "VAGRANT_LOG=DEBUG vagrant up"  ***

（10）你要是有咖啡泡，建议去泡一杯，或者和朋友打盘三国杀，放心，过程中只有开始的时候要输入一次账号密码，其他时候都是不停地打印中，等着吧......

最后出现的画面就是下面的 “ Success ”了！哈哈哈


