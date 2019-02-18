各类软硬件操作教程备忘
---
   * [树莓派](#树莓派)
      * [为树莓派WiFi配置静态IP](#为树莓派wifi配置静态ip)
      * [设置开机自动运行程序](#设置开机自动运行程序)
      * [虚拟内存](#虚拟内存)
   * [Git/Github](#gitgithub)
      * [安装](#安装)
      * [设置](#设置)
      * [基本命令](#基本命令)
      * [将本地库关联到远程](#将本地库关联到远程)
      * [MarkDown](#MarkDown)
      * [TOC](#toc)
   * [LaTex](#LaTex)
   * [vi/vim](#vivim)
      * [进入vim](#进入vim)
      * [vim配置](#vim配置)
      * [移动光标](#移动光标)
      * [屏幕滚动](#屏幕滚动)
      * [插入文本类](#插入文本类)
      * [删除命令](#删除命令)
      * [复制粘贴](#复制粘贴)
      * [撤销](#撤销)
      * [搜索及替换](#搜索及替换)
      * [书签](#书签)
      * [visual模式](#visual模式)
      * [行方式命令](#行方式命令)
      * [宏](#宏)
      * [窗口操作](#窗口操作)
      * [文件及其他](#文件及其他)
   * [pixhawk](#pixhawk)
      * [飞行模式](#飞行模式)
      * [查看飞控日志](#查看飞控日志)
   * [websites](#websites)
   * [others](#others)

# 树莓派  
## 为树莓派WiFi配置静态IP  
[树莓派手动指定静态IP和DNS 终极解决大法](https://blog.csdn.net/u013178472/article/details/78574878)  
#使用 vi 编辑文件  

    vi /etc/dhcpcd.conf 
    
增加下列配置项  

    #指定接口 wlan0  
    interface wlan0  
    #指定静态IP，/24表示子网掩码为 255.255.255.0  
    static ip_address=192.168.1.20/24  
    #路由器/网关IP地址  
    static routers=192.168.1.1  
    #手动自定义DNS服务器  
    static domain_name_servers=114.114.114.114  

实际上，只要把/etc/dhcpcd.conf文件中eth0相关的那一段（6行）复制到文件底部，将eth0接口改为wlan0，指定想要的IP即可<br>(eth为有线网口，wlan为无线网口)<br>但要求所指定的IP与路由器在同一网段下，且该IP不能被其他设备占用，因此往往先从桌面将树莓派连接到路由器，ifconfig命令查看当前IP，然后就指定该IP为静态IP
## 设置开机自动运行程序  

    $sudo vi /etc/rc.local  

例1.在第一段注释代码之下添加：  

    /bin/bash	teamviewer &  
    #在开机时自动使用bash运行teamviewer程序  
例2.在第一段注释代码之下添加：  

    /usr/bin/python	/home/pi/my_program.py &  
    #在开机时自动使用python解释器运行*.py脚本  
命令末尾的&表示在后台运行程序，否则树莓派可能无法引导  

## 虚拟内存  

树莓派资源有限，经常遇到运行时内存占用过多而卡死的情况，参照如下方式使用swap虚拟内存，可以缓解此情况  
[为树莓派配置或扩展swap分区 - 念槐聚 - 博客园](https://www.cnblogs.com/haochuang/p/6836254.html)

    $dd if=/dev/zero of=/tmp/tmp.swap bs=1M count =2000 #建立一个2000M的文件
    $mkswap /tmp/tmp.swap   #标识为SWAP文件
    $swapon /tmp/tmp.swap   #激活SWAP文件
    $sudo vi /etc/fstab  #修改/etc/fstab文件，增加以下内容
    /tmp/tmp.swap swap swap default 0 0
    $swapon -s # 或free 或cat /proc/swaps 查看
删除SWAP分区  

    $swapoff /dev/cciss/c0d0p6
    $sudo vi /etc/fstab

# Git/Github  
整理自[Git教程 - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)  
## 安装
Debian系列安装git  

    $ sudo apt-get install git  

Windows安装参考[Git - Downloads](https://git-scm.com/downloads)  
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
|git reset --hard HEAD^|回退到上一个版本|
|git reset --hard 1094a|回退到commit id的前几位为1094a的版本<br>commit id可以使用git log查看|
|$ git status |查看git仓库当前状态，并提示下一步操作|
|$ git diff readme.txt |查看本地文件和git仓库的区别|
|$ git log |查看提交记录|
|$ git log --pretty=oneline |查看提交记录|
|$ git branch dev|创建新的分支dev|
|$ git checkout dev|切换到dev分支|
|$ git checkout -b dev |创建新的分支dev，并切换到该分支|
|$ git branch -d dev |merge之后删除分支dev|
|$ git branch -D dev |放弃修改直接删除分支dev|

## 将本地库关联到远程  
<table>
<tr>
    <td width="200">ssh-keygen -t rsa –C example@example.com<br>
        #密码可设置为空</td>
    <td rowspan="7"><img src="./images/SSH_keys.png"></td>
</tr>
<tr>
    <td>将本地生成的id_rsa.pub添加到SSH key</td>
</tr>

<tr>
    <td>在GitHub建立新的仓库，按提示操作</td>
</tr>

<tr>
    <td>git remote add origin git@github.com:LiRengithub/Sort.git</td>
</tr>

<tr>
    <td>git push -u origin master<br>
    #若将其他分支例如dev推送到远程则:git push origin dev</td>
</tr>

<tr>
    <td>git checkout [-b] MergeSort<br>
    git pull origin MergeSort<br>
    #将远程分支MergeSort拉取到本地MergeSort</td>
</tr>
</table>

    push.default的默认值在 Git 2.0 已从 'matching'变更为 'simple'
    # 配置push.default的值
    $ git config --global push.default matching   #git 将推送和远程同名的所有本地分支
    # 或
    $ git config --global push.default simple #只推送当前分支到远程关联的同名分支，即 'git push' 推送当前分支。
    # 从 Git 2.0 开始，Git 默认采用更为保守的 'simple' 模式，
    # 参见 'git help config' 并查找 'push.default' 以获取更多信息。
    #（'simple' 模式由 Git 1.7.11 版本引入。如果您有时要使用老版本的 Git，为保持兼容，请用 'current' 代替 'simple'）  

## MarkDown
[编写*.md文件](https://github.com/guodongxiaren/README)
## TOC  
为markdown文档自动添加目录  
[github链接](https://github.com/ekalinin/github-markdown-toc)  

    $ wget https://raw.githubusercontent.com/ekalinin/github-markdown-toc/master/gh-md-toc
    $ chmod a+x gh-md-toc  
    $ cat README.md | ./gh-md-toc -

# LaTex
[参考教程](http://www.voidcn.com/article/p-aoquvmgu-bro.html)  
[TexLive国内镜像ISO](http://mirrors.hust.edu.cn/CTAN/systems/texlive/Images/)  


# vi/vim  
[wsdjeg/vim-galore-zh_cn: Vim 从入门到精通](https://github.com/wsdjeg/vim-galore-zh_cn)  
[wsdjeg/vim-plugin-dev-guide: Vim 插件开发指南](https://github.com/wsdjeg/vim-plugin-dev-guide)  
以下内容转自[Vim速查表-帮你提高N倍效率 - 简书](https://www.jianshu.com/p/6aa2e0e39f99)  
另外参考[vi的复制粘贴命令 - lansh - 博客园](http://www.cnblogs.com/lansh/archive/2010/08/19/1803378.html)
## 进入vim
|命令|描述|
|----|----|
|vim filename|打开或新建文件,并将光标置于第一行首|
|vim +n filename|打开文件，并将光标置于第n行首|
|vim + filename|打开文件，并将光标置于最后一行首|
|vim +/pattern filename|打开文件，并将光标置于第一个与pattern匹配的串处|
|vim -r filename|在上次正用vim编辑时发生系统崩溃，恢复filename|
|vim filename….filename|打开多个文件，依次编辑|
## vim配置  
|命令|描述|
|----|----|
|all|列出所有选项设置情况|
|term|设置终端类型|
|ignorance|在搜索中忽略大小写|
|list|显示制表位(Ctrl+I)和行尾标志（$)|
|number|显示行号|
|report|显示由面向行的命令修改过的数目|
|terse|显示简短的警告信息|
|warn|在转到别的文件时若没保存当前文件则显示NO write信息|
|nomagic|允许在搜索模式中，使用前面不带“\”的特殊字符|
|nowrapscan|禁止vi在搜索到达文件两端时，又从另一端开始|
|mesg|允许vi显示其他用户用write写到自己终端上的信息|
|:set number / set nonumber|显示/不显示行号|
|:set ruler /set noruler|显示/不显示标尺|
|:set hlsearch|高亮显示查找到的单词|
|:set nohlsearch|关闭高亮显示|
|:syntax on|语法高亮|
|:set nu|显示行号|
|:set tabstop=8|设置tab大小,8为最常用最普遍的设置|
|:set softtabstop=8|4:4个空格,8:正常的制表符,12:一个制表符4个空格,16:两个制表符|
|:set autoindent|自动缩进|
|:set cindent|C语言格式里面的自动缩进|
## 移动光标  
|命令|描述|
|----|----|
|k nk|上 向上移动n行|
|j nj|下 向下移动n行
|h nh|左 向左移动n行
|l nl|右 向右移动n行
|Space|光标右移一个字符
|Backspace|光标左移一个字符
|Enter|光标下移一行
|w/W|光标右移一个字至字首
|b/B|光标左移一个字至字首
|e或E|光标右移一个字至字尾
|)|光标移至句尾
|(|光标移至句首
|}|光标移至段落开头
|{|光标移至段落结尾
|n$|光标移至第n行尾
|H|光标移至屏幕顶行
|M|光标移至屏幕中间行
|L|光标移至屏幕最后行
|0|（注意是数字零）光标移至当前行首
|^|移动光标到行首第一个非空字符上去
|$|光标移至当前行尾
|gg|移到第一行
|G|移到最后一行
|f|移动光标到当前行的字符a上
|F|相反
|%|移动到与制匹配的括号上去（），{}，\[]，<>等
|nG|移动到第n行上
## 屏幕滚动  
|命令|描述
|----|----|
|Ctrl+u|向文件首翻半屏
|Ctrl+d|向文件尾翻半屏
|Ctrl+f|向文件尾翻一屏
|Ctrl＋b|向文件首翻一屏
|nz|将第n行滚至屏幕顶部，不指定n时将当前行滚至屏幕顶部
## 插入文本类  
|命令|描述
|----|----|
|i|在光标前
|I|在当前行首
|a|光标后
|A|在当前行尾
|o|在当前行之下新开一行
|O|在当前行之上新开一行
|r|替换当前字符
|R|替换当前字符及其后的字符，直至按ESC键
|s|从当前光标位置处开始，以输入的文本替代指定数目的字符
|S|删除指定数目的行，并以所输入文本代替之
|ncw/nCW|修改指定数目的字
|nCC|修改指定数目的行
## 删除命令
|命令|描述
|----|----|
|x/X|删除一个字符，x删除光标后的，而X删除光标前的
|dw|删除一个单词(删除光标位置到下一个单词开始的位置)
|dnw|删除n个单词
|dne|也可，只是删除到单词尾
|do|删至行首
|d$|删至行尾
|dd|删除一行
|ndd|删除当前行及其后n-1行
|dnl|向右删除n个字母
|dnh|向左删除n个字母
|dnj|向下删除n行,当前行+其上n行
|dnk|向上删除n行,当期行+其下n行
|cnw\[word]|将n个word改变为word
|C$|改变到行尾
|cc|改变整行
|shift+j|删除行尾的换行符，下一行接上来了
## 复制粘贴
|命令|描述
|----|----|
|p|粘贴用x或d删除的文本
|ynw|复制n个单词
|yy|复制一行
|ynl|复制n个字符
|y$|复制当前光标至行尾处
|nyy|拷贝n行
## 撤销
|命令|描述
|----|----|
|u|撤销前一次的操作
|shif+u(U)|撤销对该行的所有操作
## 搜索及替换
|命令|描述
|----|----|
|/pattern|从光标开始处向文件尾搜索pattern
|?pattern|从光标开始处向文件首搜索pattern
|n|在同一方向重复上一次搜索命令
|N|在反方向上重复上一次搜索命令
|cw newword|替换为newword
|n|继续查找
|.|执行替换
|:s/p1/p2/g|将当前行中所有p1均用p2替代,g表示执行 用c表示需要确认
|:n1,n2 s/p1/p2/g|将第n1至n2行中所有p1均用p2替代
|:g/p1/s//p2/g|将文件中所有p1均用p2替换
|:1,$ s/string1/string2/g|在全文中将string1替换为string2
## 书签
|命令|描述|
|----|----|
|m[a-z]|在文中做标记，标记号可为a-z的26个字母|
|\`a|移动到标记a处|
## visual模式
|命令|描述
|----|----|
|v|进入visual 模式
|V|进入行的visual 模式
|ctrl+v|进如块操作模式用o和O改变选择的边的大小
|将光标移到开始插入的位置，按CTRL+V进入VISUAL模式，选择好模块后按I（shift+i)，后插入要插入的文本，按\[ESC]完成|在所有行插入相同的内容如include<|
## 行方式命令
|命令|描述
|----|----|
|:n1,n2 co n3|将n1行到n2行之间的内容拷贝到第n3行下
|:n1,n2 m n3|将n1行到n2行之间的内容移至到第n3行下
|:n1,n2 d|将n1行到n2行之间的内容删除
|:n1,n2 w!command|将文件中n1行至n2行的内容作为command的输入并执行之
|若不指定n1，n2，则表示将整个文件内容作为command的输入||
## 宏
|命令|描述
|----|----|
|q\[a-z]|开始记录但前开始的操作为宏，名称可为【a-z】，然后用q终止录制宏
|reg|显示当前定义的所有的宏，用@\[a-z]来在当前光标处执行宏\[a-z]
## 窗口操作
|命令|描述
|----|----|
|:split|分割一个窗口
|:split file.c|为另一个文件file.c分隔窗口
|:nsplit file.c|为另一个文件file.c分隔窗口，并指定其行数
|ctrl＋w|在窗口中切换
|:close|关闭当前窗口
## 文件及其他
|命令|描述
|----|----|
|:q|退出vi
|:q!|不保存文件并退出vi
|:e filename|打开文件filename进行编辑
|:e!|放弃修改文件内容，重新载入该文件编辑
|:w|保存当前文件
|:wq|存盘退出
|:ZZ|保存当前文档并退出VIM
|:!command|执行shell命令command
|:r!command|将命令command的输出结果放到当前行
|:n1,n2 write temp.c
|:read file.c|将文件file.c的内容插入到当前光标所在的下面

# pixhawk  
## 飞行模式
[ArduPilot Tutorial(PDF版)及ArduPilot飞行模式介绍 - xiaoshuai537的博客 - CSDN博客](https://blog.csdn.net/xiaoshuai537/article/details/60465851)  
以下内容转自[PIXHAWK 飞行模式简易说明 - 多旋翼](http://www.moz8.com/thread-36543-1-105.html)  
<table>
<tr>
    <td colspan="4" align="center">PIXHAWK 飞行模式简易说明</td>
</tr>
<tr>
    <td colspan="3" align="center">飞行模式</td>
    <td rowspan="2">是否需GPS定位</td>
</tr>  
<tr>
    <td>中文名</td>
    <td>英文名</td>
    <td>MP简称</td>
</tr>

<tr>
    <td rowspan="2" width="106">自稳模式</td>
    <td width="150">Stabilize Mode</td>
    <td width="106">Stabilize</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：自稳模式会自动保持多轴直升机的水平并且维持目前的朝向。<br />
&nbsp; &nbsp; 相关资料：<a href="http://copter.ardupilot.cn/wiki/stabilize-mode/" target="_blank">http://copter.ardupilot.cn/wiki/stabilize-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">定高模式</td>
    <td width="150">Altitude Hold</td>
    <td width="106">AltHold</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：此模式不需要GPS支持，pixhawk会根据气压传感器的数据保持当前高度，但不会定点，飞行器依然会漂移，您可以遥控来移动或保持位置。<br />
&nbsp; &nbsp; 定高时飞控通过控制油门来保持高度。<br />
&nbsp; &nbsp; 定高时但仍可用遥控油门来调整高度，但不可以用来降落，因为油门不会降到0。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/altitude-hold/" target="_blank">http://copter.ardupilot.cn/wiki/altitude-hold/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">悬停模式</td>
    <td width="150">Loiter Mode</td>
    <td width="106">Loiter</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：悬停模式就是GPS定点模式。应该在起飞前先让GPS定点，避免在空中突然定位发生问题。其他方面跟定高模式基本相同。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/loiter-mode/" target="_blank">http://copter.ardupilot.cn/wiki/loiter-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">返航模式</td>
    <td width="150">RTL Mode</td>
    <td width="106">RTL</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：返航模式需要GPS定位。当切换到返航模式时，飞行器会返回家的位置。默认情况下，在返航之前，飞行器会首先飞到至少15米的高度（如果当前高度＞15米，就会保持当前高度）。<br />
&nbsp; &nbsp; 家的位置：解锁时后，GPS首次定位的位置。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/rtl-mode/" target="_blank">http://copter.ardupilot.cn/wiki/rtl-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">自动模式</td>
    <td width="150">Auto Mode</td>
    <td width="106">Auto</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：此模式下飞行器会自动执行地面站Mission Planner设定好的任务，例如起飞、按顺序飞向多个航点、旋转、拍照等。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/auto-mode/" target="_blank">http://copter.ardupilot.cn/wiki/auto-mode/</a></td>
</tr>

<tr>
    <td rowspan="2" width="106">特技模式</td>
    <td width="150">Acro Mode</td>
    <td width="106">Acro</td>
    <td width="107"><div align="center">否</div></td></tr><tr><td colspan="3" width="406">&nbsp; &nbsp; 模式说明：特技模式是仅基于速率控制的模式。<br />
特技模式提供了遥控器摇杆到飞行器电机之间的最直接的控制关系。<br />
&nbsp; &nbsp; 在特技模式下飞行，就像是不装飞控的遥控直升机一样，需要持续不断的手工摇杆操作。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/acro-mode/" target="_blank">http://copter.ardupilot.cn/wiki/acro-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">运动模式</td>
    <td width="150">Sport Mode</td>
    <td width="106">Sport</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：运动模式也可以说是“速率控制的自稳”加定高。它的设计目的是用于飞行FPV和拍摄移动镜头或者是飞越。<br />
相关资料:<a href="http://copter.ardupilot.cn/wiki/sport-mode/" target="_blank">http://copter.ardupilot.cn/wiki/sport-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">飘移模式</td>
    <td width="150">Drift Mode</td>
    <td width="106">Drift</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：飘移模式能让用户就像飞行安装有“自动协调转弯”的飞机一样飞行多旋翼飞行器。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/drift-mode/" target="_blank">http://copter.ardupilot.cn/wiki/drift-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">引导模式</td>
    <td width="150">Guided Mode</td>
    <td width="106">Guided</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：此模式需要地面站软件和飞行器之间通信。连接后，在任务规划器Mission Planner软件地图界面上，在地图上任意位置点鼠标右键，选弹出菜单中的“Fly to here”（飞到这里），软件会让你输入一个高度，然后飞行器会飞到指定位置和高度并保持悬停。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/guided-mode" target="_blank">http://copter.ardupilot.cn/wiki/guided-mode</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">绕圈模式</td>
    <td width="150">Circle Mode</td>
    <td width="106">Circle</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：当切入绕圈模式时，飞行器会以当前位置为圆心绕圈飞行。而且此时机头会不受遥控器方向舵的控制，始终指向圆心。如果遥控器给出横滚和俯仰方向上的指令，将会移动圆心。与定高模式相同，可以通过油门来调整飞行器高度，但是不能降落。 圆的半径可以通过高级参数设置调整。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/circle-mode/" target="_blank">http://copter.ardupilot.cn/wiki/circle-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">定点模式</td>
    <td width="150">Position Mode</td>
    <td width="106">OF_Loiter</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：定点模式是依赖于GPS的，其余基本和悬停模式相同，在定点模式下，飞行器会保持位置和头的方向不变，同时允许操作者手动控制油门。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/position-mode/" target="_blank">http://copter.ardupilot.cn/wiki/position-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">降落模式</td>
    <td width="150">Land mode</td>
    <td width="106">Land</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：下降至10m的过程中（或是直到声呐检测到了飞行器下面有东西之前）使用常规定高控制器，通过WPNAV_SPEED_DN参数限制下降速度，可通过Mission Planner修改，在10m内，飞行器会以LAND_SPEED参数规定的速率下降，默认为50cm/s。一到达地面，如果飞手的油门位于最低，飞行器就会自动关闭电机并锁定飞行器。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/land-mode/" target="_blank">http://copter.ardupilot.cn/wiki/land-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">跟着我！模式</td>
    <td width="150">Follow Me! Mode</td>
    <td width="106">地面站模式</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：操作者地面站设备带有GPS，此GPS会将位置信息通过地面站和数传电台随时发给飞行器，飞行器实际执行的是“飞到这里”的指令。其结果就是飞行器跟随操作者移动。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/follow-me-mode/" target="_blank">http://copter.ardupilot.cn/wiki/follow-me-mode/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">简单模式</td>
    <td width="150">Simple Modes</td>
    <td width="106">可用7/8通道切换</td>
    <td width="107"><div align="center">否</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：即无头模式，不用管机头朝向，可以将飞行器看成一个点，如果升降舵给出俯冲指令，飞行器就会飞得远离操作者；反之如果给出拉杆指令，飞行器会飞回操作者；给出向左滚转的指令，飞行器会向左飞，反之亦然。<br />
&nbsp; &nbsp; 简单模式可以让你用起飞时的头的方向控制飞行器，仅需要较好的罗盘指向。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/" target="_blank">http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/</a></td>
</tr>
<tr>
    <td rowspan="2" width="106">超简单模式</td>
    <td width="150">Super Simple Modes</td>
    <td width="106">可用7/8通道切换</td>
    <td width="107"><div align="center">是</div></td>
</tr>
<tr>
    <td colspan="3" width="406">&nbsp; &nbsp; 模式说明：超简单模式可以让你以飞行器朝向家——解锁位置的方向控制飞行器，但需要较好的GPS定位。<br />
&nbsp; &nbsp; 模型在家10m以内时，方向是不会更新的，所以要避免在家附近飞<br />
&nbsp; &nbsp; 在起飞时要确保控制是正确的，和简单模式一样，你应该在解锁时站在模型后面，飞手和模型所朝方向也应是一样的。<br />
相关资料：<a href="http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/" target="_blank">http://copter.ardupilot.cn/wiki/simpleandsuper-simple-modes/</a></td>
</tr>

</table>

## 查看飞控日志  
[APM和PIX飞控日志分析入门贴 - 闲来无事搞搞机 - CSDN博客](https://blog.csdn.net/xazzh/article/details/72814567)

# websites  
[Welcome to DroneKit-Python’s documentation!](http://python.dronekit.io/)  
[使用DroneKit控制无人机 - DarrenChan陈驰 - 博客园](https://www.cnblogs.com/DarrenChan/p/7847199.html)  
[A*算法应用[转] - 前程明亮 - 博客园](https://www.cnblogs.com/0zcl/p/6242790.html)  
[DJI 大疆创新官网 - 未来无所不能](https://www.dji.com/cn)  
[ROS/Tutorials - ROS Wiki](http://wiki.ros.org/ROS/Tutorials)  
[Ubuntu 16.04.5 LTS (Xenial Xerus)](http://releases.ubuntu.com/16.04/)  
[Arduino - Home](https://www.arduino.cc/)  
[Arduino中文社区 - Powered by Discuz!](https://www.arduino.cn/)  
[How-To-Ask-Questions-The-Smart-Way](https://github.com/FredWe/How-To-Ask-Questions-The-Smart-Way/blob/master/README-zh_CN.md)  
[OpenEdv-开源电子网](http://www.openedv.com/forum.php)  
[Linux公社资源站_Linux公社-Linux系统门户网站](https://linux.linuxidc.com/index.php)  
[Linux公社FTP 根位于 ftp1.linuxidc.com](ftp://ftp1.linuxidc.com/)   
用户名：ftp1.linuxidc.com<br>
密码：www.linuxidc.com  
[C++教程™](https://www.yiibai.com/cplusplus/)  
[Linux命令大全(手册)_Linux常用命令行实例详解_Linux命令学习手册](http://man.linuxde.net/)  
[物联网开发中心_智能硬件设备云服务接入平台_机智云](https://dev.gizwits.com/zh-cn/developer/)  
[机智云物联网开发者社区_智能硬件及开源硬件Gokit论坛](http://club.gizwits.com/forum.php)  
[菜鸟教程 - 学的不仅是技术，更是梦想！](http://www.runoob.com/)  
[gcc链接外部函数库，比如数学函数库 - liming0931的专栏 - CSDN博客](https://blog.csdn.net/liming0931/article/details/7003266)  
[神经网络教程 | tensorfly](http://www.tensorfly.cn/home/?cat=4)  
[第一章: 利用神经网络识别手写数字 | tensorfly](http://www.tensorfly.cn/home/?p=80)  
[介绍 | TensorFlow 官方文档中文版](http://www.tensorfly.cn/tfdoc/get_started/introduction.html)  
[The Python Standard Library — Python 2.7.15 documentation](https://docs.python.org/2/library/index.html)  
[Documentation — SciPy.org](https://www.scipy.org/docs.html)  
[NumPy Reference — NumPy v1.15 Manual](https://docs.scipy.org/doc/numpy/reference/)  
[The mplot3d Toolkit — Matplotlib 3.0.2 documentation](https://matplotlib.org/tutorials/toolkits/mplot3d.html#sphx-glr-tutorials-toolkits-mplot3d-py)  

# others  
[EbookFoundation/free-programming-books: Freely available programming books](https://github.com/EbookFoundation/free-programming-books)  
[gzc/CLRS: Solutions to Introduction to Algorithms](https://github.com/gzc/CLRS)  
[代码规范：在Keil5中使用代码格式化工具Astyle（插件） - wishriver - 博客园](http://www.cnblogs.com/jnhs/p/10004334.html)  

|报错信息|解决办法|
|----|----|
|pip .ReadTimeoutError|pip install * --default-timeout=1000|
