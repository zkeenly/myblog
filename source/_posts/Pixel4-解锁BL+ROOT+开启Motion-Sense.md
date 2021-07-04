---
title: Pixel4-解锁BL+ROOT+开启Motion-Sense
date: 2021-07-04 22:09:06
categories: Android
tags: 
	- android
	- pixel
	- root
	- motion sense
comments: true
---

​	最近花1200入手了一台pixel4，到手后为了发现网络连接不断提示无法连接互联网，影响到部分程序的正常联网使用。提示无法连接网络的原因是由于Pixel默认使用google域名进行网络验证，而google由于众所周知的原因在国内无法顺利访问。其实这个问题用非ROOT的方法也可以解决。

#### 获取ROOT：

基本上我的操作步骤如第一篇文章所述，部分细节需要注意，不得不说，国外的论坛真的大多简单明了还不收费，国内很多都是一通抄袭,而且步驟非常不完整。本文其實大部分内容都是引用自其他内容,但是做了較爲完整的整合引用,以及細節説明.

1. 获取Boot Image镜像

   如链接[3]，源文中未给出具体链接，这里一定要选择与手机一致的版本号，

Settings > About phone > Build number.

![1625390973678](C:\hexo\myblog\source\images\2021-07-04-1.png)

将文件解压，取出压缩包中的boot.img 文件，将文件拷贝到手机目录中（建议选择Download路径，不然后面找起来比较麻烦）

2. Magisk Manager

   面具的下载地址有很多了，如果能访问官网，我建议是直接从连接[4]中下载最新版本，安装完毕后选择install> select and patch a file > 选择boot.img文件，选择完毕之后，它将会给你一个magisk_patched.img 文件，记得把这个文件再拷贝回去电脑上留着。

3. 打开OEM锁

   去设置里将选择>About phone. >多次點擊Build number > 返回上級頁面 ,選擇System > Developer Options. > 打開OEM unlocking

4. 安裝ADB

   ADB的安裝比較簡單,就是下載一個軟件包,免安裝,如[5].需要注意的是,一定要使用CMD打開命令行,不要使用powershell.

5. Unlock Bootloader

   解鎖BL,需要關閉手機,然後按下電源按鍵以及音量減(注意是音量-).另外在此之前還需要安裝驅動.驱动程序文件如[6],安裝驅動方法如[7].

   安装好驱动后,在手机打开USB调试模式

   输入命令:

   adb devices

   可检查是否正常连接.

   进入BL页面后,可以通过命令

   fastboot devices

   检查是否有正常连接.

   进入BL页面之后,电脑连接手机,打开命令行输入

   fastboot flashing unlock

   即可解锁

   解锁完毕之后,再将我们拷贝出来的magisk_patched.img 文件刷入手机

   fastboot flash boot path/to/magisk_patched.img

   其中path/to 为本地文件所在的路径.

至此ROOT已经完毕，获取到ROOT之后，手机开机会有一个风险提示，忽略即可。

另外面具APP可能无法正常使用，需要重新安装。

#### 解锁Motion Sense：

解锁Motion Sense需要安装EdXposed,如链接[8]，在Modules中搜索enableSoliOnPixel4,如[13],安装即可。

也可以通过命令行方式修改，如链接[11]，但是据说重启会失效？

```shell
adb shell setprop pixel.oslo.allowed_override 1
```



#### 修改本地网络检查方式：

参考链接[10],将网络检查的地址修改为google中国的地址

删除默认地址

adb shell settings delete global captive_portal_https_url
adb shell settings delete global captive_portal_http_url

改用新地址

adb shell settings put global captive_portal_http_url http://captive.v2ex.co/generate_204
adb shell settings put global captive_portal_https_url https://captive.v2ex.co/generate_204

















ref:

[1]https://www.xda-developers.com/google-pixel-4-root-magisk/  获取root

[2]https://blog.csdn.net/asdrt12589wto1/article/details/115965281 网络受限

[3]https://developers.google.com/android/ota 镜像下载

[4]https://magiskmanager.co/apk/ 面具

[5]https://www.xda-developers.com/install-adb-windows-macos-linux/ 安裝ADB

[6]https://developer.android.com/studio/run/win-usb?hl=zh-cn 驱动

[7]https://blog.csdn.net/qz2014728/article/details/101760398 安裝驅動

[8]https://edxposed.com/edxposed-install 安装Edxposed 

[9]https://www.didgeridoohan.com/magisk/ManagerIssues 面具问题

[10]https://blog.csdn.net/asdrt12589wto1/article/details/115965281 网络校验修正

[11]https://forum.xda-developers.com/t/magisk-tasker-release-motion-sense-soli-oslo-mod.3993877/#post-80729593 命令行启用motion sense

[12]https://www.xda-developers.com/enable-pixel-4-motion-sense-gestures/ 启用motion sense

[13]https://forum.xda-developers.com/t/success-enable-soli-on-china.3994917/ enableSoliOnPixel4

