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
|$ git branch dev|创建新的分支dev|
|$ git checkout dev|切换到dev分支|
|$ git checkout -b dev |创建新的分支dev，并切换到该分支|
|$ git branch -d dev |merge之后删除分支dev|
|$ git branch -D dev |放弃修改直接删除分支dev|

# vi/vim  
[Vim速查表-帮你提高N倍效率 - 简书](https://www.jianshu.com/p/6aa2e0e39f99)  
[vi的复制粘贴命令 - lansh - 博客园](http://www.cnblogs.com/lansh/archive/2010/08/19/1803378.html)
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
[PIXHAWK 飞行模式简易说明 - 多旋翼](http://www.moz8.com/thread-36543-1-105.html)  

|PIXHAWK 飞行模式简易说明||||
| :------------ |:---------------:| -----:| -----:|
|飞行模式|||是否需GPS定位|
|中文名|英文名|MP简称|
|自稳模式|Stabilize Mode|Stabilize|否|
||模式说明：自稳模式会自动保持多轴直升机的水平并且维持目前的朝向<br>[相关资料](http://copter.ardupilot.cn/wiki/stabilize-mode/)|


# websites  
# others  
