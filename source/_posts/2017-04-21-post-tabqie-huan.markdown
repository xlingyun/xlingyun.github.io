---
layout: post
title: 安装Octopress步骤
date: 2017-04-21 19:23:53 +0800
comments: true
categories: 
---

#### 安装Octopress步骤

##### Ruby等依赖安装

> 由于Jekyll和octopress都是ruby写的，会有诸多ruby依赖，建议切换ruby源唯国内源。对于git版本没有太大要求。
> 查看ruby版本`ruby -v`
> 安装bundler bundler可以自动解决依赖，安装方法：`gem install bundler`,建议国内用户切换gem源为国内源，方法如下：

```
    gem source -r https://rubygems.org/ # 删除官方源
    gem source -a https://ruby.taobao.org/ #添加淘宝源
```
>  查看当前源：`gem source -l`


##### clone网站repo 
> 设置git
 
 ```
 git config --global user.name "name"
 git config --global user.email "emailaddress"
 ```
> 生成ssh key：

```
ssh-keygen -t rsa -C "emailaddress"
```

>按3个回车，密码为空，复制~/.ssh/id_rsa.pub的内容，添加到GitHub账户中，然后clone网站repo

##### octopress安装
> octopress的安装也比较简单，下载源码后会有Gemfile文件来指示所有依赖，使用`bundle`即可，下载源码:

```
git clone git://github.com/imathis/octopress.git octopress
```

> 进入octopress文件夹，使用bundle自动安装octopress，`bundle install`会自动安装所有octopress及其所有依赖。

> 执行如下命令安装默认主题

```
rake install 
``` 
所谓rake就是ruby make的缩写。
- 执行

```
rake preview
```

在浏览器内输入`http://localhost:4000`，即可看到我们搭建完成的博客
##### 部署到GitHub


tab切换
## 标题一
## 标题二
###### 标题六

- 列表项1
- 列表项2 
- 列表项3
- 列表项4
- 无序列表5

* 无序列表
* 无序列表

1. 有序列表
1. 有序列表

> 引用的一段名言
