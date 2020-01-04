# 配置属于自己的Git远程仓库

全球最大的同性交友平台(GitHub)是码农们的乐园,今天就分享一下如何搭建一个私人Git远程仓库;  

抛开需求谈问题就是耍流氓,我们为什么需要自己搭建私人的Git仓库呢~有GitHub不就够了嘛,还要这些东西干啥; 很多时候我们做的事情并不是代表这个事情有确切的用处,而是为了我们自己更好的学(zhuang)习(bi),我做这些事情,不代表有什么用,而代表 **我能做** ~~  

其实实际上还有有作用的  
  
例如:在某公司或者需要保密代码但是有需要多人协作的时候,拥有一个私人的Git代码仓库就会十分有用处. 好啦,接下来我们就一起进入学(zhuang)习(bi)之旅吧;

文章一共有3个阶段  

### **1. 在Linux安装Git**
### **2. 让Linux的Git能被其他电脑访问**
### **3. 让这种感觉变得更爽**

*****  
 
## 1. 在Linux安装Git
 第一步当然是需要有一个Linux系统咯~~,这里怎么装linux系统和装什么linux系统我就不说啦.这个网上教程一搜一大把,简单介绍下我用的linux,是window10的WindowsLinuxSubsystem - **[ubuntu 18.04LTS](https://www.microsoft.com/zh-cn/p/ubuntu-1804-lts/9n9tngvndl3q?activetab=pivot:overviewtab)** ;  
  [![l0R2MF.md.jpg](https://s2.ax1x.com/2020/01/04/l0R2MF.md.jpg)](https://imgchr.com/i/l0R2MF)
 安装好linux后,第一件事当然是更换源咯.因为ubuntu官方自带的源服务器在国外,然后你懂的~  

 我们使用
    ```bash
    $ cp /etc/apt/source.list ./source.list.back
    $ sudo vim /etc/apt/source.list
    ```
 将`网易云`的镜像源替换原本的源;
    ```bash
    deb http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ bionic main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ bionic-security main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ bionic-updates main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ bionic-proposed main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ bionic-backports main restricted universe multiverse
    ```
 然后刷新软件列表
    ```bash
    $ sudo apt update
    ```
   现在就要尝试一下你的linux中有没有git
    ```bash
    $ git --version
    $ git version 2.17.1 
    ```
   如果没有安装git的话
    ```bash
    $ sudo apt install git #安装git
    ```
  #### ***安装好之后第一步就完成啦***
   是不是很简单;  
   是的在linux上我们已经完成了 1/3 的工作;

*************
## 2. 让Linux的Git能被其他电脑访问

众所周知,git是一个分布式版本管理软件,也就是说,任何一个安装的git的电脑既可以是一个客户端也是一个服务端,既可以是我们日常搬砖的工作区间,也可以是别人来搬砖的远程仓库;  
好的现在有一个问题,那就是如何让别人访问到你的git仓库(也就是访问你的文件)  
**答案当然是`ssh` 啦**,我们只需要开启ssh 服务器服务,就可以远程访问你的linux系统.
这里WLS(WindowsLinuxSubsystem)使用的是本机的端口,我们默认是打开ssh port 22端口;
至于这个ssh怎么打开,我也不赘述了,网上一搜也一大把;  
打开ssh后我们就可以吧linux的窗口最小化,然后使用自己电脑的cmd或者powerShell对linux进行控制;**(局域网)**
这个时候我们就可以 进行我们的git操作,因为我们是希望我们要有一个git的远程仓库,因此为了不污染我们这拥有sudo 权限的账号用户,我们就新建一个用户 新建用户很简单;
然后根据步骤一步一步搞就行,我姑且新建了一个用户叫做git;

- 新建用户
    ```bash
    sudo adduser git #这里的git是你的用户名
    $ sudo adduser git
        Adding user `git'
        Adding new group `git' (1002) 
        Adding new user `git' (1002) with group `git'
        Creating home directory `/home/git' 
        Copying files from `/etc/skel' 
        Enter new UNIX password:
        Retype new UNIX password:
        passwd: password updated successfully
        Changing the user information for git
        Enter the new value, or press ENTER for the default
        Full Name []:
        Room Number []:
        Work Phone []:
        Home Phone []:
        Other []:
        Is the information correct? [Y/n] y
    $
    ```
- 尝试初始化仓库
  ```bash
    $ sudo su git
    $ cd ~
    $ git init --bare test.git
      Initialized empty Git repository in /home/git/test.git/
  ```
    为什么我们要初始化.git后缀的仓库呢?因为我们只想让他做仓库而不是一个可以操作的工作区间,实际上建文件夹也是可以的~  
- 本地git仓库添加 remote
    ```bash
    git remote add kimlocal git@127.0.0.1:/home/git/test.git
    ```
    然后接下来的操作,什么```pull``` / ```push``` 就都和使用github上的远程仓库一样啦,但是这样的话也都是需要先在你的git账户上新建一个仓库,就是:
    ```bash
    # linux服务器(远程git仓库端)
    git init --bare [repositories_name].git
    # 本地git bash
    git remote add [remote_name] git@<url>:/home/git/[repositories_name].git
    git push [remote_name]
    ```
***以上的设置都是在局域网内实现的;***  
那我们如何才可以全世界任何一个有网络的角落都可以连接上你的仓库呢~  
- 有两种方案  
    1. 购买一个云服务器,在云服务器上作上述所有操作
    2. 使用网络穿透软件穿透
    3. 如果你的设备连接在一个有公网IP的路由器下且你可以设置路由器,绑定端口映射即可;

三种方式经济上需求依次递减,第三种时免费的,但是在城市的我们基本可遇不可求,各位按需取用;只要付钱,那么就都会有详细的资料的帮助,nat123最适合屌丝使用~可以尝试一哈~;8块钱用终生;sunny-Ngrok 免费用终身,但是个人开发者,有时候会有点不稳定;

- 安全性  
  一旦将你的个人电脑暴露到公网当中那么就不得不考虑 `安全的问题` ,而且如果你真正在使用你自己搭建的远程仓库的时候会发现你每次操作都需要输入密码,不仅很繁琐,而且在某些特定的时候还会面临选择问题;

至此,我们已经可以在世界的任何一个角落访问你的远程仓库了,但是还有很多繁琐和不安全的隐患在;那么这个时候请参考 git的进阶配置

*****
