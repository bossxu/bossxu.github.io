---
title: domjudge配置流程 
tags: [ACM,oj,服务器]
date: 2018-12-27 13:01:05
---


# domjudge配置流程

对于很多的acmer来说，domjuege不是一个陌生的东西，很多的比赛里面，用到的系统不是domjudge就是pc2，就我个人来说，感觉domjudge用的好像更多点，受老师的嘱咐，最近把domjudge的基础流程走了一遍。
> 因为博主的实力不太够，我是刚退役，就会点acm的题，很多知识都是小白，所以这个可以说是从零开始的，很多都是建立在我的认知上的，如有不对，还请谅解

####  domjudge是什么呢？
 > 一些没怎么打过现场赛的可能对这个东西还是比较陌生。这个简单的说就是一个开源的在线判题系统，他很多东西都帮你集成好了，你利用这个平台，可以搭建一些小的比赛，网络赛很多是放在别的学校的oj上的，但是现场赛，不能上网，只能局域网访问，这个时候来一个domjudge这种类型的就很好。基本所有的功能它都有。

## 怎么配
domjudge的配置分为两步，这两步理论上是可以独立开来的。  
第一步是配domjudge的服务端，第二步是配置donjudge的判题端。

### 准备工作
我这里用的服务端和判题端都是在 `ubuntu 18.04`下面进行操作的。而且我看网上的很多文档也是这样写的。

#### 1. 安装依赖包和环境：
``` shell
sudo apt-get upgrade && sudo apt-get update 
```

```shell
sudo apt install gcc g++ make zip unzip mariadb-server \
        apache2 php php-cli libapache2-mod-php php-zip \
        php-gd php-curl php-mysql php-json php-xml php-mbstring \
        acl bsdmainutils ntp phpmyadmin python-pygments \
        libcgroup-dev linuxdoc-tools linuxdoc-tools-text \
        groff texlive-latex-recommended texlive-latex-extra \
        texlive-fonts-recommended texlive-lang-european 
```  
安装的时候记得选择 `apache2`

```shell
sudo apt install libcurl4-gnutls-dev libjsoncpp-dev libmagic-dev   
sudo phpenmod json`
```

#### 2. 编译domjudge
```shell
cd Downloads   
wget https://www.domjudge.org/releases/domjudge-6.0.3.tar.gz      
tar -zxvf domjudge-6.0.2.tar.gz   
```
这里也可以通过上[官网](https://www.domjudge.org)进行下载，下载和解压的地址，要记住。

```shell
cd domjudge-6.0.2
./configure --prefix=$HOME/domjudge --with-baseurl=127.0.0.1
make domserver && sudo make install-domserver
make judgehost && sudo make install-judgehost
make docs && sudo make install-docs
```
那个127.0.0.1表示的是你的默认端口，后面两句分别是安装服务端和判题端。

----

### domjudge服务端

#### 1. 配置数据库
```shell
cd ~/domjudge/domserver
sudo bin/dj_setup_database -u root install
```
这里有个点在于 ~ 对应的是主目录

#### 2. 配置web服务器
这个提示写在前面:
注意先不要直接复制跑代码，要把`ln`命令中的`username`换成你的实际用户名.
不然你就要去把那个文件删除，从新去添加。
```shell
cd ~/domjudge/domserver
sudo ln -s /home/username/domjudge/domserver/etc/apache.conf /etc/apache2/conf-available/domjudge.conf
sudo a2enmod rewrite
sudo a2enconf domjudge
sudo systemctl reload apache2
```
这个时候你可以通过连接上`http://127.0.0.1/domjudge`.通过默认用户名`admin`和密码`admin`登录后台。

#### 3. 配置mysql
编辑`/etc/mysql/my.cnf`,追加以下内容：
> 这里口头以'/'开始就表示是以根目录开始，这里记得别用vi，没有vim记得更新，vi是真的难用，不接受反驳。啦啦啦。
```
[mysql]
max_connections = 1000
max_allowed_packet = 16MB
innodb_log_file_size = 48MB
```
其中 max_allowed_packet 数值改成两倍于题目测试数据文件的大小，innodb_log_file_size 数值改成十倍于题目测试数据文件的大小。这里我不是很清楚用处，目前还没有碰到这些问题。

```shell
sudo systemctl restart mysql
```

#### 4. 配置php
编辑`~/domjudge/domserver/etc/apache.conf`,取消以下几行内容前的注释:
```
<IfModule mod_php7.c>
php_value max_file_uploads      100
php_value upload_max_filesize   128M
php_value post_max_size         128M
php_value memory_limit          512M
</IfModule>
```
编辑`/etc/php/7.2/apache2/php.ini`,搜索`date.timezone`关键字,取消前面的注释将值设为你们的地址，举例来说`Asia/Shanghai`.这里的是否有格式要求我就不清楚了。

```shell
sudo systemctl restart apache2
```

#### 5. 配置apache
编辑`/etc/apache2/apache2.conf`，搜索`KeepAlive`关键字，并将其值设为`off`,并加一行
`MaxClients 1000`
```shell
sudo systemctl restart apache2
```
不知道干嘛用的。

> 现在怎么说呢，这个服务端算是配好了，至于后面的一些加题，加队，加人的操作，在后面会给出来。后面配置这些文件的方案都是可视化的，在集成的网站上你会通过一个像用户的操作，去进行更改，这个在后面再说。

-------

## judgehost配置
这里的判题系统，怎么说呢，和我一开始的想法是不一样的，一开始我是认为，服务器去找判题机，这样每次配置的时候都是很烦的一件事，后来弄清楚，这里的原理是这样，这里的judgehost的身份相当于是一个用户，judgehost去向服务端发送请求，通过这个就可以让这些东西建立链接。所以我们服务端配好后，就不用去管一些奇奇怪怪的东西了，接着去配判题系统就可以了。

#### 1. 添加用户 
这个就看你一台主机想配几个判题机了。一般来说，现在的电脑都不会只配置一个判题机吧，我这里就默认4个了。
```shell
useradd -d /nonexistent -U -M -s /bin/false domjudge-run-0
useradd -d /nonexistent -U -M -s /bin/false domjudge-run-1
useradd -d /nonexistent -U -M -s /bin/false domjudge-run-2
useradd -d /nonexistent -U -M -s /bin/false domjudge-run-3
groupadd domjudge-run
```
对于多核判题，每次判题记得要运行`~/domjudge/judgehost/bin`里面的`create_cgroups`,记得开权限，怎么执行，就是 `sudo ./create_cgroups`，不然会报错。
  
#### 2.配置 sudoers
这里的作用应该是给domjudge_run权限。
```shell
sudo cp $HOME/domjudge/judgehost/etc/sudoers-domjudge /etc/sudoers.d/
```
#### 3.修改rest密码
这里的用处在于，建立链接的时候，让服务端辨识。  
找到`~/domjudge/judgehost/etc/restapi.secret`,这个文件的一个样例：
```
default	http://localhost/domjudge/api	judgehost	Dgj9COHDKa5bSEmT
```
其中将那个`localhost`换成你服务器的IP地址，不知道的话，用`ifconfig`看下.
后面的`judgehost` 是用户名，`Dgj9COHDKa5bSEmT`是密码。这个也是要修改的。

打开你服务端所安装的电脑的目录`~/domjudge/domserver/etc/restapi.secret`
你会发现这两个格式是相同的，你把判题端的用户名和密码换成和客户端的一样就可以了。

#### 4.构建 chroot 环境
这里的用处是他把你的判题环境也集成好了，你只要选择安装一下就可以，但是它那里面是默认源，可能有点不太行，推荐用阿里源，用了两次没啥问题。


