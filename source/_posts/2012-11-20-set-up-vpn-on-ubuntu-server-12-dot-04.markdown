---
layout: post
title: "在Ubuntu Server 12.04上搭建 VPN"
date: 2012-11-20 18:06
comments: true
categories: [ubuntu] 
---

####题外话
为什么要搭建VPN?这个,全世界哦不1/3个世界都知道为什么我就不解释了,为什么要写这个呢,一是前段时间折腾了很久没发现能一步到位的教程,再之记忆力不好怕下次出问题再忘了又得折腾.

####过程

##### 准备工作

1.  购买并给VPS安装系统
1.  用SSH连接到VPS
1.  安装顺手的文本编辑器(Emacs,Vim,blabla其他的我不认识)
1.  安装pptpd `apt-get install pptpd`

##### 开始配置

###### 配置 pptpd 

		sudo emacs /etc/pptpd.conf 

找到下面的文本

		#localip 192.168.0.1 
		#remoteip 192.168.0.234-238,192.168.0.245
        
首先把前面的#号去掉，后面的IP可以适当修改，比如我就改成了

		localip 192.168.13.1
        remoteip 192.168.13.234-238,192.168.13.245
        
原因是我的路由器IP段也是192.168.0.*所以担心会有冲突（其实我也不知道会不会= =)

###### 配置DNS

        sudo emacs /etc/ppp/pptpd-options
    
找到下面的文本

        #ms-dns 10.0.0.1
        #ms-dns 10.0.0.2
    
替换为

        ms-dns 8.8.8.8
        ms-dns 8.8.4.4
    
Google的DNS，如果VPS在国外的话，这个用起来是没问题的，如果在国内的话，你搭VPN做什么= =

###### 添加VPN账号 ######

        sudo emacs /etc/ppp/chap-secrets

在文件后面添加

        username pptpd password *
        
分别表示用户名 类型 密码 允许的ip地址

###### IPv4重定向 ######

        emacs /tec/sysctl.conf

找到

       #net.ipv4.ip_forward=1
       
去掉前面的#
保存退出后在终端中执行

       sysctl -p
       
如果看到输出结果就对了

###### **iptables 配置** ######

这一步是最重要也最容易出错的地方，我就是在这一步折腾了好几次，如果前面都配好了VPN是能连上的，但是连上之后是上不了网地。这些个命令在我看那个教程里面有点不一样，所以特别记录一下

终端中执行

        sudo iptables -t nat -A POSTROUTING -o eth0 -s 192.168.13.0/24 -j MASQUERADE
        sudo iptables -I FORWARD -p tcp --syn -i ppp+ -j TCPMSS --set-mss 1356
        
注意这里用到的ip一定要跟前面配置pptpd时的ip一致，如果现在重启pptpd然后连上VPN的话，已经可以科学上网了，但是最好还是保存一下这个让每次重启的时候自动执行，不然每次都需要手动执行一次这两条命令

        sudo iptables-save > /etc/iptables-rules
        
让pptpd随系统启动

        sudo emacs /etc/rc.local
        
在后面添加命令

        /etc/init.d/pptpd start
        
保存退出，配置完成，现在在终端中执行

        /etc/init.d/pptpd restart
        
或者重启VPS测试看能不能自动启动，连上VPS，试试能不能科学上网就可以了。
        

##### 引用 #####
[How to Set Up A VPN In A VPS](http://jblevins.org/projects/markdown-mode/)
