---
title: hexo博客如何配置到服务器上
date: 2019-03-15 20:31:27
tags: [服务器，博客]
---

## 把博客放到自己得服务器上

> 最近搞了个服务器，其实在寒假就搞了一个腾讯云，但是啊，这个首付确实还挺便宜的，但是续费的时候要我1000块，这就很难受了。然后听别人说的就去搞了个阿里云。之前自己博客是买了个域名放在了github送的那个小服务器上，能用，但是感觉就有点奇怪。

### 首先啊
> 我是从hexo+github起步的。首先你得知道hexo如何在本地搭建那个博客得。网上其实有很多得教程，但是啊，感觉还是挺奇怪得，很多说得不是很详细，很多到底是本地操作还是服务器端得呢。其实搞清楚后，东西也不是很多。总结来说，一个是服务器上，你要做个git仓库，然后安装下web服务器，然后就没了，在本地，你要做得就是改下你的那个hexo得部署，之前你是放在github得那个地方，现在就是放在本地就完事了。

### 服务器上的操作

#### hexo有关
> 我们之前把hexo的博客放在那个github的服务器上，我们其实什么插件都不用装，把文件直接push到服务器上就可以了，再加一个CANME就好了，这说明啥，这说明我们的那个网站，其实就是个静态的网页，打开的那种。**唉，说到这里其实我就有一个问题了，我们的博客在hexo g静态文件生成的时候，是有静态文件的，里面有一个index.html你要是直接打开，你会发现gg 思密达，但是你要放在web服务器上就能渲染的很好。倒是打破了我对web服务器的理解**然后任务就很明显，把hexo的静态文件放到服务器上的web服务器上就完事了。

#### git用户
> 这个用户，我在一开始是觉得奇怪的。没啥用的啊，搞个git的插件就行了。实际上，确实是这样，没啥用，唯一的用处在于那个，我们把博客推送到github服务器上，不是不用密码吗，是因为我们直接给了ssh的权限，这个的用处好像也就在这里了，对这个用户，我们可以放大点权限，让我们博客直接送过去，不然就会有权限限制。很多博客都是这样说的，那我就这样做了。具体的做法就是
+ 创建用户
+ 给权限[这个挺重要的，注意下]
+ 建立ssh的信任问题
+ 创建一个仓库

#### web服务器
> 这个干啥的呢，自己百度把。我用的是apache服务器，然后这个服务器需要做啥呢
+ 改默认访问网页
+ 建立一个/var/www/hexo的文件，把默认网页访问改成这个。记得把这个文件的权限改成git可以动的。
+ 在那个git用户下面，对那个空仓库，做一个钩子，这个你要注意把那个钩子做成**可执行文件**，干啥呢，就是把你的那个仓库放到/var/www/hexo里面去

### 本地操作

> 改下那个_config.yml里面的那个部署的地址就好了

### 阿里云操作

> 你要去开下那个80端口。
> 再改下那个你的域名映射

### 备案

> 为啥不知道，但是好像确实要去搞下。不然可能会被查水表。反正旁边的人都搞了
