---
layout: post
keywords: blog
description: blog
title: "CentOS上搭建SSH协议Git服务器"
categories: [git]
tags: [git]
group: git
icon: fa fa-github
---

####安装 git
{% highlight bash %}
$ yum install git
{% endhighlight %}

####安装 setuptools
需要使用 [setuptools](https://pypi.python.org/packages/source/s/setuptools/) 安装 gitosis 来管理 git 用户权限
{% highlight bash %}
$ wget https://pypi.python.org/packages/source/s/setuptools/setuptools-3.4.4.tar.gz
$ tar zxvf setuptools-3.4.4.tar.gz
$ cd setuptools-3.4.4
$ python setup.py build
$ python setup.py install
{% endhighlight %}

####安装 gitosis

使用 [gitosis](https://github.com/res0nat0r/gitosis) 控管 User / Project 权限

{% highlight bash %}
$ git clone git://github.com/res0nat0r/gitosis.git
$ cd gitosis
$ python setup.py install
{% endhighlight %}

####添加用户 git

{% highlight bash %}
$ sudo useradd -r -s /bin/sh -c 'git version control' -d /home/git git
$ mkdir -p /home/git
$ chown git:git /home/git
{% endhighlight %}

####上传本地公钥

{% highlight bash %}
$ ssh-keygen -t rsa                             #在本地生成rsa密钥和公钥
$ scp ~/.ssh/id_rsa.pub USER@YOUR_SERVER:/tmp   #上传公钥id_rsa.pub到服务器
{% endhighlight %}

####在服务端生成管理库

{% highlight bash %}
$ sudo -H -u git gitosis-init < /tmp/id_rsa.pub
Initialized empty Git repository in /home/git/repositories/gitosis-admin.git/
Reinitialized existing Git repository in /home/git/repositories/gitosis-admin.git/
{% endhighlight %}

####同步配置文件到本地

{% highlight bash %}
$ git clone git+ssh://git@YOUR_SERVER/gitosis-admin.git
{% endhighlight %}

####创建新的 repositories

{% highlight bash %}
$ cd gitosis-admin
$ vi gitosis.conf       #打开gitosis.conf文件,添加一个新的group
 [gitosis]

 [group gitosis-admin]
 writable = gitosis-admin
 members = cpscorpio@localhost

 [group workspace]
 writable = mytestApp
 members = cpscorpio@localhost

{% endhighlight %}

这里定义了一个叫 **workspace** 的组，授予 **cpscorpio@localhost** 这个用户读写  **mytestApp** repositories 的权限

{% highlight bash %}
$ git commit -a -m "Allow cpscorpio write access to mytestApp"  #同步到服务器
$ git push
{% endhighlight %}

创建 **mytestApp** repositories

{% highlight bash %}
$ mkdir ~/mytestApp
$ cd ~/mytestApp
$ git init
$ echo "#mytestApp" >> README.MD    #随便添加一个文件
$ git remote add origin git@YOUR_SERVER:mytestApp.git
$ git add .
$ git commit -a -m "initial import"
$ git push origin master:refs/heads/master
{% endhighlight %}

####增加项目成员

{% highlight bash %}
$ cd gitosis-admin
$ cp ~/xiaoli@localhost.pub keydir/     #添加成员公钥
$ git add keydir/xiaoli@localhost.pub
$ vi gitosis.conf                       #修改 gitosis.conf, 添加 xiaoli@localhost 用户读写 mytestApp
 [gitosis]

 [group gitosis-admin]
 writable = gitosis-admin
 members = cpscorpio@localhost

 [group workspace]
 writable = mytestApp
 members = cpscorpio@localhost xiaoli@localhost

$ git commit -a -m "Granted xiaoli commit rights to mytestApp" #提交修改
$ git push
{% endhighlight %}