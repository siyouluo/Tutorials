各类软硬件操作教程备忘
---
# 树莓派  
## [为树莓派WiFi配置静态IP](https://blog.csdn.net/u013178472/article/details/78574878)  
#使用 vi 编辑文件，增加下列配置项  
    vi /etc/dhcpcd.conf 

    #指定接口 wlan0  
    interface wlan0  
    #指定静态IP，/24表示子网掩码为 255.255.255.0  
    static ip_address=192.168.1.20/24  
    #路由器/网关IP地址  
    static routers=192.168.1.1  
    #手动自定义DNS服务器  
    static domain_name_servers=114.114.114.114  

实际上，只要把/etc/dhcpcd.conf文件中eth0相关的那一段（6行）复制到文件底部，将eth0接口改为wlan0，指定想要的IP即可(eth为有线网口，wlan为无线网口)但要求所指定的IP与路由器在同一网段下，且该IP不能被其他设备占用，因此往往先从桌面将树莓派连接到路由器，ifconfig命令查看当前IP，然后就指定该IP为静态IP
## 设置开机自动运行程序  
$sudo vi /etc/rc.local  

例1.在第一段注释代码之下添加：  

    /bin/bash	teamviewer &  

即在开机时自动使用bash运行teamviewer程序  
例2.在第一段注释代码之下添加：  

    /usr/bin/python	/home/pi/my_program.py &  
 
即在开机时自动使用python解释器运行*.py脚本  

命令末尾的&表示在后台运行程序，否则树莓派可能无法引导  

# Git/Github  
整理自[Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)  
## 安装
Debian系列安装git  

    $ sudo apt-get install git  

[Windows安装](https://git-scm.com/downloads)  
## 设置  

    $ git config --global user.name "Your Name"  
    $ git config --global user.email "email@example.com"  

注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置。当然也可以对某个仓库指定不同的用户名和Email地址。  
## 基本命令  

    $ mkdir learngit  
    $ cd learngit  
    $ pwd  
    /Users/michael/learngit  

    $ git init  
    Initialized empty Git repository in /Users/michael/learngit/.git/  

通过git init命令把这个目录变成Git可以管理的仓库  

|命令|作用|
|----|----|
|$ git add readme.txt|将文件添加到git仓库暂存区|
|$ git commit -m "wrote a readme file" |将文件提交到git仓库|
|$ git status |查看git仓库当前状态，并提示下一步操作|
|$ git diff readme.txt |查看本地文件和git仓库的区别|
|$ git log |查看提交记录|
|$ git log --pretty=oneline |查看提交记录|
|$ git branch dev
$ git checkout dev 
Switched to branch 'dev' |创建新的分支dev并切换到该分支|
|$ git checkout -b dev |Switched to a new branch 'dev' |
|$ git branch -d dev |Deleted branch dev (was b17d20e).merge之后删除分支dev|
|$ git branch -D dev |Deleted branch dev (was b17d20e). 放弃修改直接删除分支dev|

# vi/vim  
# pixhawk  
# websites  
# others  
