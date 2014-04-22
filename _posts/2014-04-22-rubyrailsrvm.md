---
layout: post
keywords: blog
description: blog
title: "学习ruby/rails，安装rvm"
categories: [rvm]
tags: [ruby,rvm]
group: rvm
icon: file-o
---

[rvm](https://rvm.io/)是一个命令行工具，可以提供多版本ruby环境的管理和切换。  
####安装 rvm

>	`$ curl -L https://get.rvm.io | bash -s stable`

####修改 RVM 的 Ruby 安装源
>	`$ sed -i -e 's/ftp\.ruby-lang\.org\/pub\/ruby/ruby\.taobao\.org\/mirrors\/ruby/g' ~/.rvm/config/db`

####安装 Ruby
查看可以安装的ruby版本
>	`$ rvm list known`

安装一个ruby版本

>	`$ rvm install 1.9.3`

使用一个ruby版本

>	`$ rvm use 1.9.3`

设置版本 1.9.3 为默认使用版本

>	`$ rvm use 1.9.3`

查询已经安装的ruby版本

>	`$ rvm list`

卸载一个已安装的ruby版本

>	`$ rvm remove 1.9.3`
