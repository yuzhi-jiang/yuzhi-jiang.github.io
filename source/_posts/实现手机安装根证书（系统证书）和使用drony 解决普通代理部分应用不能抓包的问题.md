---
title: 实现手机安装根证书（系统证书）和使用drony 解决普通代理部分应用不能抓包的问题
tags: 抓包 drony 系统证书
categories: []
date: 2021-10-17 22:21:06
---

<!--more-->

## 说明

```
Android证书分为“用户证书”和“系统证书”两种，在设置->安全->"查看安全证书"列表中，可以看到“系统”和“用户”两个列表。用户通过浏览器下载安装或者通过WLAN高级设置安装的证书均为用户证书。

关于证书的两个注意事项

（1）安装用户证书必须要设置开机密码，而且设置后就不能取消，除非先删掉所有的用户证书。如果安装为系统证书就不需要设置开机密码，自动化操作时更方便。
（2）Android 7以上版本APP默认不信任用户证书，只信任系统证书，安装为用户证书，对APP的HTTPS抓包会失败。安装为全局证书才能被所有APP信任，方可进行HTTPS抓包。

默认情况下，针对 Android 7.0+ (API level 24+) 的应用不再信任用户或管理员添加的CA证书来进行安全连接。（之前我们其实是将安全证书安装到安卓手机上作为用户信任安全证书，新版本如果APP开启了设置我们的代理请求会被认为是不安全的。）
Android的系统证书的存储位置是/system/etc/security/cacerts，证书文件必须是PEM格式，而且文件命名必须符合系统证书规范

```
通过说明，我们明白了现在的手机基本安装普通证书（用户证书）很多应用都将无法抓包，因为很多应用都有ssl或者检查代理的反抓包措施，那么这时候就无法抓包了吗，答案当然不是。
网上还是有这些的，但是遇到的一下问题总要查好多篇文章才找得到解决方案，所以针对这情况，我以自身情况为列写这篇文章，以免以后要用。

本文以夜神模拟器为列（因为手上没有root的真机）
抓包工具以fiddler4为例
电脑设备以windows10为例
# 一 .准备工作
1、准备一台已经 root的手机（本文以夜神模拟器为例）
2.电脑安装好adb,openssl（如果阿里云支持分享这类的话我倒是不介意留链接，又因为这个环境安装还是比较简单所以就不留教程和链接了，自行百度即可）
3.电脑安装好fiddler4


#  二.安装根证书流程
1. fiddler导出cer证书

	打开fiddler，导航条里选择Tools,选择Options,然后选择HTTPS选卡,在右侧点击Actions, 选择第二个到处证书到桌面

![在这里插入图片描述](https://img-blog.csdnimg.cn/dbad3b2b866043c3952795a6ef3aaded.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)

~~注意这里有两打勾的勾选上才能抓包HTTPS流量~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/0204be5ad7d84457a2b77e4123f965fb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)



![在这里插入图片描述](https://img-blog.csdnimg.cn/9b062f549d2b4554b231b9c33daf61ad.png#pic_center)

2. cer证书转pem证书
==cmd需要进入cer证书所在目录==

```
openssl x509 -inform der -in FiddlerRoot.cer -out FiddlerRoot.pem
```

==或者输入时带上证书路径,此处youname，指的是用户名称如zhangsan==

```
openssl x509 -inform der -in C:\Users\youname\Desktop\FiddlerRoot.cer -out FiddlerRoot.pem
```

3. 为证书计算hash值改名然后pem证书

```
openssl x509 -inform PEM -subject_hash -in FiddlerRoot.pem -noout
```
会输出一个hash值
![在这里插入图片描述](https://img-blog.csdnimg.cn/2add5fd65ecf45698534b60491fbefc3.png)


~~这里放一个其他博客提到的格式~~
[博客链接](https://blog.csdn.net/u010132177/article/details/117199579/).

```
#PEM或者DER格式均可

#如果是PEM格式：
In: openssl x509 -inform PEM -subject_hash_old -in mitmproxy-ca-cert.pem -noout
out: c8750f0d

# 如果是DER格式：
In: openssl x509 -inform PEM -subject_hash_old -in mitmproxy-ca-cert.cer -noout
out: c8750f0d


```


4. 将hash值赋值，并将xxx.pem证书改名为hash值后缀改为0（如c8750f0d.0）



5. adb连接手机
	

	 **查看电脑是否成功连接到手机**

	```
	adb devices -l #显示所有已连接的设备详细信息：127.0.0.1：62001
	
	未连接则运行如下命令连接
	adb connect 127.0.0.1:62001 #默认端口 
	```
	**连接成功状态**
![在这里插入图片描述](https://img-blog.csdnimg.cn/5def9a1e78544fb29a96f2e08aba09d6.png)

6. adb 推送证书到sdcard目录
	

	```
	#传入手机，提醒小白需注意源文件（即xxxx/xxx/c8750f0d.0）目录问题
	adb push c8750f0d.0 /sdcard
	```

7. adb shell 修改system目录权限（避免出现adb push时 Read-only file system的错误）
	-   进入cmd
	-  adb remount（此处若提示`Not running as root. Try "adb root" first.`则运行  `adb shell su -c "mount -o rw,remount,rw /system"`）
	- adb shell
	- su或者#（获取手机的root权限）
	- #挂载系统目录为可写（如此方法不可用，用下面那条命令，直接修改system权限）
mount -o rw,remount /

	-  chmod 777 system
	
	
	
8. 将证书移动到目录（/system/etc/security/cacerts/）
	==紧接这上面修改完系统权限后，将证书移到目标目录==
	

	```
	mv /sdcard/c8750f0d.0 /system/etc/security/cacerts
	```
9.#修改证书权限

```
chmod 644 /system/etc/security/cacerts/c8750f0d.0
```
如果没意外，至此已经安装系统证书到手机


**查看是否成功：**
手机的设置——安全——凭据存储——信任的凭据（信任的CA证书）
==注意，其他证书，名字不一样==
![在这里插入图片描述](https://img-blog.csdnimg.cn/cb79764b4b46427fafff72868b7cb436.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_18,color_FFFFFF,t_70,g_se,x_16)



# 三.安装drony

分享网友提供的链接

英文版https://ustb.lanzoux.com/iXrGTjtw2re            密码：68e1
中文版https://ustb.lanzoux.com/iVML1jtw9ed          密码:2b71


	使用说明：手机wifi处无需设置代{过}{滤}理，抓包工具代{过}{滤}理为本机127.0.0.1:port，测试时使用模拟器，真机需要将电脑防火墙全部关闭才可识别到连接WiFi，仅为部分app可用，如果app做了双向代{过}{滤}理可能就不行了
[吾爱文章链接](https://www.52pojie.cn/forum.php?mod=viewthread&tid=1340770&highlight=drony)


Droni_1.3.155谷歌商店版：https://pan.xunlei.com/s/VMfYAdY7fai4ddg13xRs-SAoA1   提取码：45fi
 [吾爱文章链接](https://www.52pojie.cn/forum.php?mod=viewthread&tid=1483104&highlight=drony)



因为使用软件方法都一样，就吾爱那里拔了一些图（侵删）


![在这里插入图片描述](https://img-blog.csdnimg.cn/9b767888f8e047b6938aa5ff0b4c3e70.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2a9c58f63bc543f78e209d4d9e1219cc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/d89e74b59a9e4b619da22bd973e3c619.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/1900047259e349ffb0a6f5afed59191a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e4d4f3a294aa4c6885a3116d8662dd9b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_16,color_FFFFFF,t_70,g_se,x_16#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/b3090395de424f5ca32098ca6a87aefe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5aSc5p6r5LmL5a62,size_18,color_FFFFFF,t_70,g_se,x_16#pic_center)


# 到此就可以愉快的抓包了
因为本文使用的是夜神模拟器，所以此次从模拟器上，配置drony后就无法上网（在未root的真机可以上网），以前在root上可以正常抓包，不知道是不是模拟器的问题还是什么，暂时未解决，
如有大佬知晓，望不吝指点

# 最后留下参考链接

[部分APP无法代理抓包的原因及解决方法（flutter 抓包）](https://www.cnblogs.com/lulianqi/p/11380794.html#_label2)
[adb push时 Read-only file system的错误](https://blog.csdn.net/hhh901119/article/details/78984648?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_title~default-0.no_search_link&spm=1001.2101.3001.4242)
[命令“adb root”有效，但“adb remount”导致“不允许操作”消息](https://android.stackexchange.com/questions/52443/the-command-adb-root-works-but-adb-remount-results-in-operation-not-permit)
[为安卓系统(夜神模拟器)添加Mitmproxy证书](https://blog.csdn.net/u010132177/article/details/117199579)
