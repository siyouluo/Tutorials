# 树莓派
---
   * [树莓派](#树莓派)
      * [为树莓派WiFi配置静态IP](#为树莓派wifi配置静态ip)
      * [设置开机自动运行程序](#设置开机自动运行程序)
      * [交换空间](#交换空间)

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

## 交换空间  

树莓派资源有限，经常遇到运行时内存占用过多而卡死的情况，参照如下方式使用swap交换空间，可以缓解此情况  
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
