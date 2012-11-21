---
layout: post
title: "在Ubuntu Server 12.04上搭建 VPN"
date: 2012-11-20 18:06
comments: true
categories:[ubuntu] 
---

####题外话
为什么要搭建VPN?这个,全世界哦不1/3个世界都知道为什么我就不解释了,为什么要写这个呢,一是前段时间折腾了很久没发现能一步到位的教程,再之记忆力不好怕下次出问题再忘了又得折腾.

####过程

#####准备工作

1.  购买并给VPS安装系统
1.  用SSH连接到VPS
1.  安装顺手的文本编辑器(Emacs,Vim,blabla其他的我不认识)
1.  安装pptpd `apt-get install pptpd`

#####开始配置

配置 pptpd 

		sudo emacs /etc/pptpd.conf 

找到下面的文本

		#localip 192.168.0.1 
		#remoteip 192.168.0.234-238,192.168.0.245
首先把前面的#号去掉，后面的IP可以适当修改，比如我就改成了

		localip 192.168.13.1
		remoteip 192.168.13.234-238,192.168.13.245
原因是我的路由器IP段也是192.168.0.*所以担心会有冲突（其实我也不知道= =)

