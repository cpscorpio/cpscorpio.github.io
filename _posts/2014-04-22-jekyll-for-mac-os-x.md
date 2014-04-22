---
layout: post
keywords: blog
description: blog
title: "本地搭建Jekyll环境 for Mac OS X"
categories: [Jekyll]
tags: [Jekyll]
group: Jekyll
icon: file-o
---
如果本地已经安装了gem，直接在终端输入: 
{% highlight bash %}
$ gem install jekyll
{% endhighlight %}

如果安装失败，可能是ruby版本或者是gem有问题。

首先 [安装ruby](../../../../../rvm/2014/04/22/rubyrailsrvm/)

升级 gem : 
{% highlight bash %}
$ sudo gem update --system
{% endhighlight %}
