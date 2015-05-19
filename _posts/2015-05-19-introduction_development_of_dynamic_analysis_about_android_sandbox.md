---

layout: post
category: Notes
title: Android沙盒开发之动态分析介绍
tagline: by GodJobs
tags: [Android, Sercurity]

---

随着Andriod用户的增多，针对Android系统的恶意软件越来越多。
因此，分析Android应用变得越来越重要。目前，分析Android apk的工具有静态分析工具和动态分析工具。
静态分析主要就是反编译apk，分析反编译之后的代码。
动态分析主要就是让程序运行起来，获取程序运行过程中产生的api调用，从而获取其行为信息。
本文主要研究动态分析，因此以下主要从动态分析入手。

动态分析可以分析程序的很多行为，包括：

   1. 程序启动的Activity、Service、BroadcastReceiver组件。
   2. 程序root权限获取。
   3. 程序文件操作，打开、读写、关闭文件。
   4. 程序数据库操作。
   5. 程序短信发送、录音、定位、获取手机IMEI/IMSI/电话号码、拨打电话、电话状态监听、终止短信接收、改变电话状态(挂断、接听)。
   6. 程序通讯录、通话记录、短信数据库获取。
   7. 程序访问网络URL、IP地址和端口号、网络抓包。
   8. 程序开机自启动、接收短信行为、电话。
   9. 动态权限获取。
   10. 程序ptrace注入、dexclassloader加载、Java反射调用。
   11. 程序开机自启动

目前，动态分析工具有APIMonitor和DroidBox。DroidBox是基于TaintDroid构建的分析工具。
分析了一下TaintBox源码，主要工作原理是在dalvik目录中增加了对对象的操作类Taint.cpp，并在对象中增加u8 tag变量，存储标识，完成对关键数据的污点标记。
当程序通过API获取敏感信息，利用封装的Taint类讲数据进行污点标识。TaintBox的污点追踪值得学习借鉴。

实现方式：

   1. 内核空间api拦截，通过printk将格式信息写入。在PC端通过adb shell  cat  /proc/kmsg读取信息解析。
   2. 用户空间api拦截。
