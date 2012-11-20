---
layout: post
title: "解决M9ICS固件无法安装Twicca和Foursquare等软件的问题"
date: 2012-03-04 14:34
comments: true
categories: [Android]
---
其实我之前稍微有点后悔买了M9，买成2K5，当时2儿子也不过2K左右。买来之后没有Root，不能换字体不能爬墙，郁闷了我相当长一段时间。等这个4.0的固件也是等了相当久，不过还好，至少没有食言，像我这种标准小白鼠，有4.0固件当然要刷之，刷了之后BUG还是很多的。不过我都还能接受。主要是看过ICS的UI之后再也不想看到2.3的UI了…差距还是很大的。
*****
跑题了跑题了，其他BUG暂且不说，居然无法安装Twicca，这可是我最喜欢的软件，没有之一，很是纠结了一番，但是前段时间一直没时间折腾，今天总算花了点时间来折腾。到网上找来Twicca的安装文件，用`adb install Twicca.apk`一试，显示错误大概是

    [MISS SHARED LIBRARY…]
之类的,记忆力不好记不清了= =,copy到Google搜寻一番后,在某个Google Groups的帖子里找到原因是在Androidmanifest.xml文件中声明了uses-library但是ROM里面没有。但是我又不晓得Twicca需要什么library，于是反编译出来一看，原来是`com.google.android.maps`.Meizu也太坑爹了吧，这个包都给删掉！然后从同学的华为Honor固件里面找到了`com.google.android.maps.jar`包放到了对应的/system/frameworks/下，重启，提示正在更新Android系统中…我正想这应该靠谱了，再次尝试安装，再次失败，差点恼了，于是再到Honor的固件里面搜索maps,就发现了这个东西`com.google.android.maps.xml`在/etc/permissons/里面，照样放进去，重启，搞定。Twicca成功安装，Foursquare也成功安装，然后我发现我过年回家抢来的几个Mayor不见了= =，郁闷。 

总结一下：

* 找到`com.google.android.maps.jar`和`com.google.android.maps.xml`
* 分别用RE管理器放在/system/frameworks和/etc/permissons/下面
* 重启搞定

不过我Twicca认证通过了一直没能刷新，Gaeproxy最新版，全局也不行，也就是说折腾半天我屁都没捞到。