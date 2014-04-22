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