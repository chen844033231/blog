title: 在windows环境下搭建react-native(超详细教程)
date: 2016-1-14 15:39:25
categories:
- WEB
- 框架
tags:
- react-native
- 安卓
---
![](/blog/css/images/reactNative.png)

## 前言
facebook开源的react-native可以采用javascript技术来开发安卓和ios原生应用,但是在开发之前,许多新同学都被搭建环境,运行起第一个hello world程序给拦住了,导致后面的步骤没法进行,笔者看了下目前react-natvie在windows上搭建安卓开发环境的文章,没有写的十分详细,所以打算写一篇详细的文章,来带领初学者在windows上搭建一个react-native的教程.

## 准备工作
(重要提示:推荐全程翻墙,或者采用vpn,不然可能会有未知问题).如果没有VPN,推荐一个免费的[1小时免费vpn](http://free.vpn.wwdhz.com/).

下面的教程基于目前最新版react-native(0.18.0-rc)来搭建.react-native是对安卓的原生代码进行了封装,所以,如果要想在windows上运行安卓程序,需要安装java环境.在安装了java环境后,需要安装android sdk.当这些都安装好后,就可以安装react-native相关东西了.下面一步步进行安装讲解:
### 1. 下载java环境(JDK).
进入[JDK下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
![](/blog/css/images/reactNative/jdk.png)

下载相应系统的jdk.下面演示采用64位的windows. 下载后安装,记住时安装目录.默认安装到(C:\Program Files\java),后面需要加入环境变量.

### 2. 添加java路径到环境变量.
在桌面上,找到的计算机图标,右击-属性-高级系统设置-环境变量-系统变量里面找到Path,然后把java路径加入到path路径中.确定后退出.
例如我下载后java安装到了`C:\Program Files\Java`,然后把`C:\Program Files\Java\jdk1.8.0_65\bin`加入到path路径.
进入命令提示符.输入 `javac`.如果输出一大串用法提示. 就代表jdk安装成功.可以继续后面的步骤了.如果没有,那么需要重新安装JDK.检测下是否忘记把jdk加入环境变量.
![](/blog/css/images/reactNative/javac.png)

### 3. 下载安装android SDK.
如果你以前没有安装过android SDK,最简单的方法是到android官方下载[android studio](https://developer.android.com/sdk/index.html).这个是android官方推荐的编辑器,里面包含了开发安卓的工具和sdk包.到这里下载[android studio](https://developer.android.com/sdk/index.html). (需要翻墙). 如果没有翻墙工具,国内用户可以到[android dev tools](http://androiddevtools.cn/)下载android studio. 作为演示用android studio1.5.1正式版.(这个只是编辑器,不包含 sdk).推荐直接下载全部的android studio.

### 4. 设置SDK
打开Android SDK Manager.(在安装的sdk目录下有个SDK Manager.exe,双击运行).
选中以下项目：
Android SDK Build-tools version 23.0.1
Android 6.0 (API 23)
Android Support Repository
点击"Install Packages" (国内用户推荐使用腾讯Bugly的镜像来加速下载)
![](/blog/css/images/reactNative/sdk.png)

### 5. 模拟器安装
安装Genymotion,Genymotion是一个第三方模拟器，它比Google官方的模拟器更易设置且性能更好。但是，它只针对个人用户免费。如果你想使用Google模拟器，请往下看。

- 下载并安装[Genymotion](https://www.genymotion.com/),进入官方下载页面.(注意看下面有个If you want Genymotion for personal use only, please download it here,个人免费版本).
- 打开Genymotion。如果你尚未安装VirtualBox，它有可能会提示你安装。
- 创建一个模拟器并启动。(下载模拟器需要翻墙)
- 按下⌘+M可以打开开发者菜单（在安装并启动了React Native应用之后）。

###### 备选方案：使用Google官方模拟器 (不推荐,笔者没成功)
打开Android SDK Manager(参见"设置SDK"一步)

选中以下项目：
- Intel x86 Atom System Image (for Android 5.1.1 - API 22)
- Intel x86 Emulator Accelerator (HAXM installer)

点击"Install Packages"
配置硬件加速(HAXM)，否则模拟器会运行的相当缓慢。
创建Android虚拟设备(AVD):
运行android avd并且点击Create... （译注：在Windows系统下，android.bat在Android SDK的tools文件夹下，请注意设置PATH环境变量以便于使用） 创建虚拟设备对话框
选中新创建的虚拟设备，并点击Start...

注意:对于Windows用户而言，Intel x86 Emulator Accelerator和HyperV（系统内置的虚拟机功能）不能同时启用。所以要么选择关闭HyperV（控制面板-程序-启动和关闭Windows功能，取消选择HyperV并点确定），要么选择Genymotion或Bluestacks作为模拟器

### 6.安装react-native
当在把`jdk`和`android sdk`安装好后,进行react-native的安装.在终端输入(需要用管理员权限运行命令提示符):
```shell
npm install -g react-native-cli
```
react-native-cli是一个终端命令，它可以完成其余的设置工作。它可以通过npm安装。刚才这条命令会往你的终端安装一个叫做`react-native`的命令。这个安装过程你只需要进行一次。

然后找到需要建立项目的文件夹,在这个路径下运行:
```shell
react-native init MyFirstProject
```
这个命令会初始化一个工程、下载React Native的所有源代码和依赖包，最后在MyFirstProject/iOS/MyFirstProject.xcodeproj和MyFirstProject/android/app下分别创建一个新的XCode工程和一个gradle工程.

注意: 上面的步骤将会从npm下载依赖包,如果npm源下载比较慢,可以换成淘宝的源,然后重新运行命令:
```shell
npm config set registry https://registry.npm.taobao.org
npm config set disturl https://npm.taobao.org/dist
```

### 6.运行项目
首先确认是否有可以使用的设备,可以通过在命令行输入`adb devices`,查看可用的设备.(adb命令需要把android的sdk/platform-tools加入环境变量.)
```shell
$ adb devices
List of devices attached
emulator-5554 offline   # Google模拟器
14ed2fcc device         # 真实设备
```
在右边那列看到device说明你的设备已经被正确连接了。注意，你只应当连接仅仅一个设备。

注意:如果你连接了多个设备（包含模拟器在内），后续的一些操作可能会失败。拔掉不需要的设备，或者关掉模拟器，确保adb devices的输出只有一个是连接状态。

现在你可以运行`react-native start`和`react-native run-android`来在设备上安装并启动应用了。

注意:在真机上运行时可能会遇到白屏的情况，请找到并开启悬浮窗权限。比如miui系统的设置[在此处](http://jingyan.baidu.com/article/f25ef25466c0fc482d1b824d.html)。

### 7.运行hello world项目
在项目文件路径下输入命令:
```shell
react-natvie start
```
这个命令会运行一个packager进程,来打包react-native程序.然后在打开一个新的命令提示符窗口.在项目文件路径下输入:
```shell
react-natvie run-android
```

等待模拟设备上弹出hello-world.

### 8.通过wifi在真机上运行程序.
通过usb在真机上运行react-native,查看这篇文章[设备上运行](http://reactnative.cn/docs/running-on-device-android.html#content).

下面讲解通过wifi调试安卓程序.
开始之前，确保你的安卓手机已经ROOT.
1. 首先让android手机监听指定的端口,这一步需要在手机上使用shell，因此手机上要有终端模拟器，可以安装一个Android Terminal Emulator，打开这个终端，依次敲入下列几行:
```shell
1. su           //获取root权限
2. setprop service.adb.tcp.port 5555  //设置监听的端口，端口可以自定义，如7890，5555是默认的
3. stop adbd    //关闭adbd
4. start adbd   //重新启动adbd
```
2. 手机和电脑连接上同一网段,记住手机的ip地址,比如:192.168.20.30(可以在手机的无线网络里面找到链接的wifi,点击后可以看到ip地址)
3. 然后在电脑上通过命令提示符输入:
```
adb connect 192.168.20.30:5555
```
192.168.20.30是你手机的ip地址,端口是上面命令的端口号.
4. 确认手机连接上电脑.命令提示符输入:
```shell
adb devices
```
如果出现你设备的id.说明连接成功.

###### 提示:如果闲上面的步骤麻烦,可以在手机上下载一个app叫`WiFi ADB`,安装后一键开启.

如果在真机上运行时遇到白屏的情况，请找到并开启悬浮窗权限。比如miui系统的设置[在此处](http://jingyan.baidu.com/article/f25ef25466c0fc482d1b824d.html)。

如果在运行`react-native run-android`出现`unable to upload some APIs`. 需要在项目文件/android/build.gradle文件里面把
```
dependencies {
        classpath 'com.android.tools.build:gradle:1.3.1'
        ...
    }
```
改成:
```
dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
        ...
    }
```

如果还是报错.可以采用如下步骤手动打包.
进入/android路径,命令行输入:
```
gradlew.bat assembleDebug
```
然后继续输入:
```
adb install app/build/outputs/apk/app-debug.apk
```
如果,进入apk后,报红屏错误.在手机端进行如下步骤:
- 1.摇晃设备，或者运行adb shell input keyevent 82，可以打开开发者菜单。
- 2.点击进入Dev Settings。
- 3.点击Debug server host for device。
- 4.输入你电脑的IP地址和端口号（譬如10.0.1.1:8081）。在Mac上，你可以在系统设置/网络里找查询你的IP地址。在Windows上，打开命令提示符并输入ipconfig来查询你的IP地址。在Linux上你可以在终端中输入ifconfig来查询你的IP地址。
- 5.回到开发者菜单然后选择Reload JS。

至此,react-native安装成功.有任何问题,可以在issue上反馈-_-