---
title: 使用Hexo + Github搭建自己的私人博客
---

##背景介绍
目前有很多博客平台，简书、CSDN等等，但是作为程序猿，就应该装逼搞个自己的私人博客平台，能够自己DIY。网上有很多这种博客，写这篇文章的目的，主要是记录自己创建博客的一个历程，如果你还没有创建过，不妨跟着这篇博客做起来。这篇文章是基于Windows平台的，mac和Linux 的请绕道。

##准备阶段
+ [git环境搭建](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/00137396287703354d8c6c01c904c7d9ff056ae23da865a000)
+ [windows 下安装nodejs及其配置环境](http://www.cnblogs.com/pigtail/archive/2013/01/08/2850486.html)
+ github账户的注册和配置

如果搭建node.js 之后出现express不是内部或外部命令,请移步 [nodejs小问题：express不是内部或外部命令](http://jingyan.baidu.com/article/922554468a3466851648f419.html)，先不要急着操作，因为前面的是入坑，真正的解决方法在后面。
安装成功后，测试创建node.js项目的时候，怎么就默认创建到了个人文件夹？？我想创建到指定目录，比如F:\nodejs\workspcace，怎么去实现呢？？网上也没找到教程，通过  **[nodejs的express使用介绍](http://www.cnblogs.com/mq0036/p/5243312.html#toc11)**  项目创建都依赖于 **package.json** 。
打开express的安装目录F:\nodejs\node_modules\express  这个是windows 下安装nodejs及其配置环境npm config set prefix "F:\nodejs" 的全局路径，打开express文件夹底下，打开package.json 修改以下代码中的"C:\\XXX\\XXX"到指定的目录，进入cmd 运行express helloworld 但是没有效果，不知道是什么原因，如果有知道的请告知，谢谢。

    "_args": [
    [
      {
        "raw": "express",
        "scope": null,
        "escapedName": "express",
        "name": "express",
        "rawSpec": "",
        "spec": "latest",
        "type": "tag"
      },
      "C:\\XXX\\XXX"
    ]   
    ]
    还有一个地方
    "_where": "C:\\XXX\\XXX",
 
 这样就只能cd 命令先进入我们想要的指定文件夹 XXX/XXXX/workspace，然后创建项目了。

#### github账户的注册和配置
如果已经拥有账号，请跳过此步
打开https://github.com/，在下图的框中，分别输入自己的用户名，邮箱，密码。

![2017-05-14_133901.png](http://upload-images.jianshu.io/upload_images/1996162-a574dfd880bdddbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后打开邮箱确认注册，再进行登陆，就可以使用github pages了。登陆之后，点击页面右上角的加号，选择New repository：

![2017-05-14_134352.png](http://upload-images.jianshu.io/upload_images/1996162-10c8ae629b95399c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Repository name下填写yourname.github.io，Description (optional)下填写一些简单的描述（不写也没有关系），如图所示：

![2017-05-14_140311.png](http://upload-images.jianshu.io/upload_images/1996162-b2536d4d7afe7ee7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意：比如我的github名称是philos ,这里你就填 philos.github.io

接下来开启gh-pages功能，点击界面右侧的Settings，你将会打开这个库的setting页面，向下拖动，直到看见GitHub Pages，如图：

点击Automatic page generator，Github将会自动替你创建出一个gh-pages的页面。
 如果你的配置没有问题，那么大约15分钟之后，yourname.github.io这个网址就可以正常访问了~ 如果yourname.github.io已经可以正常访问了，那么Github一侧的配置已经全部结束了。

到此搭建hexo博客的相关环境配置已经完成，下面开始讲解Hexo的相关配置

##Hexo的相关配置
### hexo简介
> Hexo是一个简单、快速、强大的基于 Github Pages 的博客发布工具，支持Markdown格式，有众多优秀插件和主题。

官网： [http://hexo.io](http://hexo.io)
github: [https://github.com/hexojs/hexo](https://github.com/hexojs/hexo)

###  hexo原理
> 由于github pages存放的都是静态文件，博客存放的不只是文章内容，还有文章列表、分类、标签、翻页等动态内容，假如每次写完一篇文章都要手动更新博文目录和相关链接信息，相信谁都会疯掉，所以hexo所做的就是将这些md文件都放在本地，每次写完文章后调用写好的命令来批量完成相关页面的生成，然后再将有改动的页面提交到github。

**注意：**hexo有2种_config.yml文件，一个是根目录下的全局的_config.yml，一个是各个theme下的

### hexo安装

    $ npm install -g hexo

### 初始化博客工作空间
新建一个文件夹（比如名字为blog），你的博客项目的代码和文件都会保存到这里。

```
$ cd /f/blog/
$ hexo init
```
hexo 会自动下载一些文件到这个目录，包括node_moudules，目录结构如下：

![2017-05-16_015259.png](http://upload-images.jianshu.io/upload_images/1996162-02c7bf3c5442a938.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

hexo s是开启本地预览服务，打开浏览器访问 http://localhost:4000 即可看到内容，很多人会碰到浏览器一直在转圈但是就是加载不出来的问题，一般情况下是因为端口占用的缘故，因为4000这个端口太常见了，解决端口冲突问题请参考这篇文章：

[http://blog.liuxianan.com/windows-port-bind.html](http://blog.liuxianan.com/windows-port-bind.html)

第一次初始化的时候hexo已经帮我们写了一篇名为 Hello World 的文章，默认的主题比较丑，打开时就是这个样子：
![20160818_132443_202_6848.png](http://upload-images.jianshu.io/upload_images/1996162-7289e01b272f50a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 修改主题
个人比较喜欢的2个主题：[hexo-theme-jekyll](https://github.com/pinggod/hexo-theme-jekyll) 和 [hexo-theme-yilia](https://github.com/litten/hexo-theme-yilia)。

首先下载这个主题：

```
$ cd /f/blog
$ git clone https://github.com/litten/hexo-theme-yilia.git themes/yilia
```
下载后的主题都在这里：

![2017-05-16_023839.png](http://upload-images.jianshu.io/upload_images/1996162-ddf9ec7e159b842c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改/f/blog 里面的_config.yml中的theme: landscape改为theme: yilia，然后重新执行hexo g来重新生成。打开[http://localhost:4000](http://localhost:4000)看到具体的效果如下：
![2017-05-16_024643.png](http://upload-images.jianshu.io/upload_images/1996162-2fe229ebc956ad55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果出现一些莫名其妙的问题，可以先执行hexo clean来清理一下public的内容，然后再来重新生成和发布。

### 上传之前
在上传代码到github之前，一定要记得先把你以前所有代码下载下来（虽然github有版本管理，但备份一下总是好的），因为从hexo提交代码时会把你以前的所有代码都删掉。

### 配置 SSH key
在上传之前需要先配置一下 SSH key

    $ cd ~/. ssh #检查本机已存在的ssh密钥

如果提示：No such file or directory 说明你是第一次使用git。

    ssh-keygen -t rsa -C "邮件地址"

然后连续3次回车，最终会生成一个文件在用户目录下，打开用户目录，找到.ssh\id_rsa.pub文件，记事本打开并复制里面的内容，打开你的github主页，进入个人设置  -> SSH and GPG keys -> New SSH key：

![2017-05-16_025817.png](http://upload-images.jianshu.io/upload_images/1996162-6e6b455976947edb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**测试是否成功**

```
$ ssh -T git@github.com # 注意邮箱地址不用改
```
此时你还需要配置(全局配置)：

```
$ git config --global user.name "philos"// 你的github用户名，非昵称
$ git config --global user.email  "xxx@qq.com"// 
```

### 配置_config.yml
这里面都是一些全局配置，需要特别注意的地方是，冒号后面必须有一个空格，否则可能会出问题。

配置_config.yml中有关deploy的部分：
**正确写法：**
```
deploy:
  type: git #注意这里不是github
  repository: git@github.com:philos3/philos.github.io.git
  branch: master
```

填写你的github注册邮箱
### 上传到github

由于hexo默认会把所有md文件都转换成html，包括README.md，所有需要每次生成之后、上传之前，手动将README.md复制到public目录，并删除README.html。

打开你的git bash，输入**hexo d**就会将本次有改动的代码全部提交

### 常用hexo命令
**常见命令**

```
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
```

**缩写：**

```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

**组合命令：**

```
hexo s -g #生成并本地预览
hexo d -g #生成并上传
```

## 写博客
千呼万唤始出来，这个才是以后我们的日常目的。在/f/blog/ 文件夹下 执行命令

    hexo new 'my-first-blog 我的博客名字'

hexo会帮我们在_posts下生成相关md文件。
我们只需要打开这个文件就可以开始写博客了，默认生成如下内容：
![](./2017-05-16_032041.png)

当然你也可以直接自己新建md文件，用这个命令的好处是帮我们自动生成了时间。

一般完整格式如下：

```
---
title: postName #文章页面上的显示名称，一般是中文
date: 2013-12-02 15:30:16 #文章生成时间，一般不改，当然也可以任意修改
categories: 默认分类 #分类
tags: [tag1,tag2,tag3] #文章标签，可空，多标签请用格式，注意:后面有个空格
description: 附加一段文章摘要，字数最好在140字以内，会出现在meta的description里面
---

以下是正文
```

**如何让博文列表不显示全部内容**

默认情况下，生成的博文目录会显示全部的文章内容，如何设置文章摘要的长度呢？

答案是在合适的位置加上<!--more-->即可。