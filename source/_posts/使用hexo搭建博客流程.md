---
title: 使用hexo搭建博客流程
date: 2017-07-21 13:51:34
categories: 博客 #文章分类
tags: [搭建,博客,hexo] #文章标签
description: 跟风随大流的搭建博客
comments: true
---

- 首先声明，本教程所针对的是windows用户。

### 安装Git

下载并安装git，如果你想了解点Git的基础命令，我推荐以下博文：[Git常用的基础命令](http://opiece.me/2015/04/06/git-basic-commands/)，史上最全github使用方法：[github入门到精通](http://blog.csdn.net/v123411739/article/details/44071059/),当然即使你不懂Git的命令，跟着本博文走，也完全没问题。

### 安装Node.js

下载并安装Node.js，此处建议不要下载最新的v6开头的版本，否则每次执行hexo命令都会有提示，建议使用v5开头的版本，Node.js主要用于安装hexo，npm开头的命令都依赖于Node.js，此处我建议安装完毕后重启电脑，因为我当初安装完没重启，结果后面使用命令安装hexo的时候，提示无效的命令，因此推荐重启。当然，你也可以选择等到后面遇到问题，再选择重启。如果出现以下提示则代表你的Node.js没有安装或者还未生效，如果已经安装了则重启电脑，如果没安装则安装再重启电脑。

### 安装hexo

安装前先介绍几个hexo常用的命令,#后面为注释。
> $ hexo g #完整命令为hexo generate，用于生成静态文件

> $ hexo s #完整命令为hexo server，用于启动服务器，主要用来本地预览

> $ hexo d #完整命令为hexo deploy，用于将本地文件发布到github上

> $ hexo n #完整命令为hexo new，用于新建一篇文章

鼠标右键任意地方，选择Git Bash，使用以下命令安装hexo（ps：以下命令中的$符号只为了让教程和实际看起来一致，实际输入命令只需输入$ 后面的命令即可）

> $ npm install hexo-cli -g

如果之后在使用的过程中，遇到以下的错误
> ERROR Deployer not found : github

则运行以下命令,或者你直接先运行这个命令更好。
> $ npm install hexo-deployer-git --save

接下来创建放置博客文件的文件夹：hexo文件夹。在自己想要的位置创建文件夹，如我hexo文件夹的位置为E:\hexo，名字和地方可以自由选择，当然最好不要放在中文路径下，至于原因，我想很多人懂得。之后进入文件夹，即E:\hexo内，点击鼠标右键，选择Git Bash，执行以下命令，Hexo会自动在该文件夹下下载搭建网站所需的所有文件。
> $ hexo init

安装依赖包
> $ npm install

让我们看看刚刚下载的hexo文件带来了什么，在E:\hexo内执行以下命令，
> $ hexo g

> $ hexo s

然后用浏览器访问http://localhost:4000/，此时，你应该看到了一个漂亮的博客了，当然这个博客只是在本地的，别人是看不到的，hexo3.0使用的默认主题是landscape。轻轻松松就看到了一点成果，是不是很激动，这就是hexo的强大之处，这个本地预览的功能，我真是爱不释手。

### hexo的配置文件

[详细参照](http://theme-next.iissnan.com/getting-started.html)

hexo里面有两个常用到的配置文件，分别是整个博客的配置文件E:\hexo\_config.yml和主题的配置文件E:\hexo\themes\light\_config.yml，此地址是对于我来说，hexo3.0使用的默认主题是landscape，因此你们的地址应该是E:\hexo\themes\landscape\_config.yml，下文所有讲到light的地方，你们将之换为自己的主题名即可。本博客使用的主题是基于light改善的主题，目前还在完善中，如果完成的比较好，以后可能发布在github上。如果你想自己挑选喜欢的主题，hexo官方提供了12个主题供你自己选择，使用方法很简单，点击自己想要的主题，进入该主题的Repository，使用Git将主题clone到本地，然后将整个文件夹复制到E:\hexo\themes文件夹下，将E"\hexo\_config.yml里的theme名字改为自己下载的主题的文件夹名即可。


```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site	这下面的几项配置都很简单，你看我的博客就知道分别是什么意思
title: Chillax blog	#博客名
subtitle: Goals determine what you are going to be	#副标题
description: Goals determine what you are going to be #用于搜索，没有直观表现
author: huangjunhui	#作者
language: zh-CN	#语言
timezone: 	#时区，此处不填写，hexo会以你目前电脑的时区为默认值

# URL	暂不配置，使用默认值
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://opiece.me  #域名
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory		暂不配置，使用默认值
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing	文章布局等，使用默认值
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  tab_replace:

# Category & Tag	暂不配置，使用默认值
default_category: uncategorized
category_map:
tag_map:

# Date / Time format	时间格式，使用默认值
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination	
## Set per_page to 0 to disable pagination
per_page: 10	#每页显示的文章数，0表示不分页
pagination_dir: page

# Extensions	插件配置，暂时不配置
## Plugins: http://hexo.io/plugins/
## Themes: http://hexo.io/themes/
plugins:
- hexo-generator-feed
theme: light	#使用的主题，即：E:\myblog\themes文件夹下的主题文件夹名

feed:	#之后配置rss会用，使用如下配置即可
  type: atom
  path: atom.xml
  limit: 20  

# Deployment	用于部署到github，之前已经配置过
## Docs: http://hexo.io/docs/deployment.html

deploy: 
  type: git
  repository: http://github.com/huangjunhui/huangjunhui.github.io.git
  branch: master
```

按照自己的意愿修改完后，执行hexo g，hexo s，打开localhost:4000看看效果。
E:\hexo\themes\light\_config.yml，此处针对Concise主题，如果使用其他主题，请查看自己主题的帮助文档，NexT主题帮助文档


```
menu:	#博客右上角的菜单栏，暂时使用默认值
  首页: /
  归档: /archives
  关于: /about
  
#该类对应于E:\hexo\themes\light\layout\_widget下的文件
widgets:	#站点右边栏，可以参照我的博客看，暂时使用默认值
- intro		#简介
- search		#搜索
- category	#分类
- tagcloud	#标签云
- weibo		#微博
- blogroll	#友情链接



excerpt_link: Read more	#文章下的Read more，可以改为'阅读全文'

plugins:	#插件，暂时使用默认值

twitter:	#twitter
  username:
  show_replies: false
  tweet_count: 5

addthis:	#SNS分享，暂时使用默认
  enable: true
  pubid:
  facebook: true
  twitter: true
  google: true
  pinterest: true

fancybox: true	#图片效果，使用默认值

google_analytics: 
rss: 	#生成RSS，暂时使用默认值
```


### 注册Github帐号

已经有Github帐号跳过此步，首先进入[Github](https://github.com/)进行注册，用户名、邮箱和密码之后都需要用到，自己记好。


### 创建repository

[百度](http://www.baidu.com/)

### 部署本地文件到github

既然Repository已经创建了，当然是先把博客放到Github上去看看效果。编辑E：\hexo下的_config.yml文件，建议使用Notepad++(Sublime 也可以哈哈哈)。

在_config.yml最下方，添加如下配置(命令中的第一个huangjunhui为Github的用户名,第二个huangjunhui为之前New的Repository的名字,记得改成自己的。另外记得一点，hexo的配置文件中任何’:’后面都是带一个空格的),如果配置以下命令出现ERROR Deployer not found : github，则参考上文的解决方法。


```
deploy: 
  type: git
  repository: http://github.com/huangjunhui/huangjunhui.github.io.git
  branch: master
```

配置好_config.yml并保存后，执行以下命令部署到Github上。如果你是第一次使用Github或者是已经使用过，但没有配置过SSH，则可能需要配置一下，另一种发布到github的配置走[原文](http://opiece.me/2015/04/09/hexo-guide/)（我弄的时候没有使用到这一步骤）。


```
$ hexo g
$ hexo d
```

执行上面的第二个命令，可能会要你输入用户名和密码，皆为注册Github时的数据，输入密码是不显示任何东西的，输入完毕回车即可。
此时，我们的博客已经搭建起来，并发布到Github上了，这时可以登陆自己的Github查看代码是否已经推送到对应Repository，在浏览器访问huangjunhui.github.io就能看到自己的博客了。第一次访问地址，可能访问不了，您可以在几分钟后进行访问，一般不超过10分钟。


### 发表一篇文章

1. 在Git Bash执行命令：$ hexo new "my new post"
2. 在E:\hexo\source\_post中打开my-new-post.md，打开方式使用记事本或notepad++。
hexo中写文章使用的是Markdown，没接触过的可以看下[Markdown语法说明](http://wowubuntu.com/markdown/#p),[一分钟学会Markdown](http://lutaf.com/markdown-simple-usage.htms)


```
title: my new post #可以改成中文的，如“新文章”
date: 2015-04-08 22:56:29 #发表日期，一般不改动
categories: blog #文章文类
tags: [博客，文章] #文章标签，多于一项时用这种格式，只有一项时使用tags: blog
---
#这里是正文，用markdown写，你可以选择写一段显示在首页的简介后，加上
<!--more-->#在<!--more-->之前的内容会显示在首页，之后的内容会被隐藏，当游客点击Read more才能看到。
```

写完文章后，你可以使用1.$ hexo g生成静态文件。2.$ hexo s在本地预览效果。3.hexo d同步到github，然后使用http://你的项目名.github.io进行访问。


```
------hexo new page tags  新建标签
------hexo new page categories 新建分类
```

### 清除缓存

清除缓存文件db.json和已生成的静态文件public。
在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。


```
$ hexo clean
```
转载要注明出处：谢谢[JoonWhee](http://opiece.me/2015/04/09/hexo-guide/)






