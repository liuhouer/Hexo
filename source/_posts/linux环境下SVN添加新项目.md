title: "linux环境下SVN添加新项目"
date: 2015-12-21 11:08:50
tags: [代码同步,svn]
categories: SVN
---


* 1.登陆到SVN服务器
```
帐号： ssh  test@192.168.1.100
密码： 123456
```
* 2.新建SVN项目
a、 进入svn创建项目的目录 
```
cd /etc/apache2/mods-enabled/ 
```
b、 编辑文件 
```
sudo vi dav_svn.conf 
```
密码： 123456（即账户test的登录密码，下同）

<!--more-->

c、 添加新项目（testsvn为例），在文件末尾添加以下代码 
```

<Location /testsvn>   
 DAV svn   
 SVNPath /home/fruits/svn/projects/code/testsvn  
 AuthType Basic   
 AuthName "Subversion repository"   
 AuthUserFile /etc/svn-auth-file   
 Require valid-user   
</Location>  
```
* 3、新建项目（testsvn）资源库
```
sudo svnadmin create /home/fruits/svn/projects/code/testsvn
```
注：现在可以通过TortoiseSVN下载svn资源了，但还不能上传（因为用户没有写权限） 

* 4、修改项目（testsvn）访问权限
```
sudo  chmod  777  -R   /home/fruits/svn/projects/code/testsvn 
```
注： -R 递归设置testsvn文件夹下的所有权限为读+写+执行 

* 5、验证创建svn项目（testsvn）是否成功
a.从svn上下载testsvn到本地
利用 TortoiseSVN 工具，下载文件svn资源到本地（右键checkout），刚才新建testsvn项目的svn网址： 
```
http://192.168.1.100/testsvn/ 
```
或  命令下载svn资源如下 
```
svn   co   http://192.168.1.100/testsvn/    testsvn
```
b. 上传本地文件到svn上
```
> a、 新建文件 aaa.txt， 输入：doodlemobile 
> b、 右键——》TortoiseSVN ——》Add... 
> c、 右键——》 SVN Commit... ——》 输入更改记录，如：add aaa.txt 
> d、 打开浏览器，输入：http://192.168.1.100/testsvn/，查看是否上传成功！ 
```
* 6、常见问题
1） Could not open the requested SVN filesystem 错误
```
解决： 这是因为还没有创建项目资源库（testsvn），因此无法访问到此文件，解决方法请见上述步骤3
```
2） Permission denied 错误
```
解决： 这是用户没有写权限（无法上传文件），解决方法请见上述步骤4
```


 

 






 

 



