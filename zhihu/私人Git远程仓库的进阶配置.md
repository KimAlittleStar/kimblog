# 私人Git远程仓库的进阶配置
****
  - 阅读本文的必要条件:  
    1. linux命令行的基本操作
    2. git 的基本使用
**** 

 ## ssh免密登录  
  - ### 本地端的准备
    1. 生成密钥  
        使用git bash 执行以下命令
        ```bash
        cd ~
        ssh-keygen -t rsa # 生成的密钥代表你这台电脑
        ```
        打开 用户文件夹下/.ssh/id_rsa.pub,复制当中的那行公钥 备用          

  - ### 远程端(git仓库端的准备)
    1. 如果是你自己的公钥,那么你可以使用
        ```bash
        ssh-copy-id [XX]@<IP>   #在本地的终端操作 这里需要输入密码
        ```
    2. 如果你需要添加别人的密钥到此远程服务器,那么你需要ssh登录服务器后
        ```bash
        vim ~/.ssh/authorized_keys 
        # 编辑 加入其他人的密钥,然后告知其他人登录
        ```
  - *须知*  
    第一次使用密钥免密登陆时会要求你将服务端的公钥保存下来,此时输入yes即可;  

 ## 本地新建远程仓库
  - 在本地新建远程仓库的含义是:在使用git-shell的基础之上在远程服务器上新建一个仓库,避免了管理员每次出现新项目是都需要管理员登录管理员账户去新建一个空项目;从而方便大家;这在服务器搭建的初期阶段十分有用处  
  
  - **为什么需要修改git用户的默认shell 为 git-shell**  
    
    为了保证服务器的安全性,一般我们都会把git用户的默认ssh shell 更改为 git-shell ,这样每次你操作 ssh 后就会立刻退出,而不能做其他的 类似于 ls mkdir 之类的操作,提高了安全性;但是与此同时,你也只能从远程git仓库中clone/push/pull  
    修改默认 ssh shell:
    ```bash
    sudo vim /etc/passwd 
    # 1. 找到git 用户
    # 2. 将git 用户默认的shell 由 '/bin/bash' 修改为 '/usr/bin/git-shell'
    ```  


  - **在本地操作初始化一个远程仓库**

    如果我们什么都不做,仅仅将默认ssh 改为git-shell 当我们登录到ssh当中时:  
    ```bash
    $ ssh git@localhost
      fatal: Interactive git shell is not enabled.
      hint: ~/git-shell-commands should exist and have read and execute access.
      Connection to localhost closed.
    ``` 
    系统提示:交互式 git shell 没有 使能,提示:~/git-shell-commands 应该存在且拥有读和执行的权限;

    - **使能 交互式 git shell**  
      1. 登录 sudo 权限的账户 或者 使用该git账户bash命令行进行操作  
        ```bash
        # clone the project
        git clone https://github.com/KimAlittleStar/git-shell-init
        cd ./git-shell-init
        # config Interactive git-shell ./config.sh [username] [git repository path]
        # 第一个参数填写用户名,默认值为当前操作的用户名
        # 第二个参数填写初始化仓库的目录,你可以在根目录新建一个目录减少路径长度 默认为/home/[username]/
        sudo bash ./configure.sh git /home/git
        ```  