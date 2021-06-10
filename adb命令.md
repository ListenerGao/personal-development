# adb 常用命令

## 设备连接
**1. USB连接**
通过 USB 连接来正常使用 adb 需要以下步骤：
1. 确认硬件状态正常(包括 Android 设备处于正常开机状态，USB 连接线和各种接口完好)。
2. Android 设备的开发者选项和 USB 调试模式已开启(可以在「设置」-「开发者选项」-「USB调试」打开USB调试)。
3. 确认设备驱动状态正常(安装ADB驱动程序)。
4. 通过 USB 线连接好电脑和设备后确认状态。
5. 通过 adb devices 命令查看设备连接情况。

**2. WLAN连接（需要 USB 线）**
借助 USB 通过 WiFi 连接来正常使用 adb 需要以下步骤：
1. 将 Android 设备与要运行 adb 的电脑连接到同一个 WiFi。
2. 将设备与电脑通过 USB 线连接(可通过 adb devices 命令查看设备连接情况)。
3. 通过 adb tcpip 5555 命令让设备在 5555 端口监听 TCP/IP 连接。
4. 断开 USB 连接。
5. 找到设备的 IP 地址(可以在「设置」-「关于手机」-「状态信息」-「IP地址」查看 IP 地址)。
6. 通过 adb connect <device-ip-address> 命令使用 IP 地址将 Android 设备与电脑连接。
7. 通过 adb devices 命令查看设备连接情况。
8. 使用完毕后可通过 adb disconnect <device-ip-address> 命令断开无线连接。

**3. WLAN连接（不需要 USB 线）**
**需要 root 权限。** 不借助 USB 通过 WiFi 连接来正常使用 adb 需要以下步骤：
1. 在 Android 设备上安装一个终端模拟器(可通过[Terminal Emulator for Android Downloads](https://jackpal.github.io/Android-Terminal-Emulator/)下载)。
2. 将 Android 设备与要运行 adb 的电脑连接到同一个 WiFi。
3. 打开 Android 设备上的终端模拟器，在里面依次运行命令：
```
su
setprop service.adb.tcp.port 5555
```
4. 找到设备的 IP 地址(可以在「设置」-「关于手机」-「状态信息」-「IP地址」查看 IP 地址)。
5. 通过 adb connect <device-ip-address> 命令使用 IP 地址将 Android 设备与电脑连接。
6. 通过 adb devices 命令查看设备连接情况。


## 常用命令
* adb version //查看adb的版本信息
* adb start-server //启动adb
* adb kill-server //停止adb
* adb root //以root权限执行adb命令
* adb -p <port> start-server //制定 adb server 的网络端口；ADB默认端口为 5037。
* adb devices  //查看连接设备
* adb -s emulator-5554 shell //连接指定设备 emulator-5554 为设备名称
* adb install demo.apk //安装应用
* adb install -r demo.apk //（覆盖）安装应用
* adb uninstall \<package_name> //卸载应用，需要指定包名
* adb uninstall -k \<package_name> //卸载应用，保留数据和缓存文件
* adb shell pm clear \<package_name> //清除应用数据与缓存
* adb shell am force-stop \<package_name> //强制停止应用
* adb shell pm list packages //列出设备安装所有应用的包名
* adb shell pm list packages -3 //列出除了系统应用的第三方应用包名
* adb shell dumpsys package \<package_name> //根据包名查看应用信息
* adb shell dumpsys meminfo //查看内存使用情况Memory Usage
* adb shell top -m 10 //查看占用内存前10的应用
* adb shell am start \<package_name>/.Activity(Launcher Activity) //启动应用
* adb push <local> <remote> //从本地复制文件到设备
* adb pull <remote> <local> //从设备复制文件到本地

## 查看设备信息
* adb shell getprop ro.build.version.release //获取设备系统版本
* adb shell getprop ro.build.version.sdk //获取设备系统api版本
* adb shell getprop ro.product.model //获取设备型号
* adb shell getprop ro.product.brand //获取设备厂商名称
* adb shell cat /sys/class/net/wlan0/address //获取设备MAC地址
* adb shell df //获取设备存储信息
* adb shell wm density //获取设备物理密度
* adb shell wm size //获取设备分辨率
* adb shell cat /proc/cpuinfo //查看设备CPU信息
* adb shell cat /proc/meminfo //查看设备内存信息
* adb shell dumpsys window displays //查看设备显示屏参数
* adb shell settings get secure android_id //获取设备android_id
* adb shell cat /system/build.prop //查看更多硬件及系统属性
* adb shell getprop <属性名> //查看单独属性含义


    | 属性                              | 含义               |
    |:-------------------------------:|:----------------:|
    | ro.build.version.sdk            | SDK 版本           |
    | ro.build.version.release        | Android 系统版本     |
    | ro.build.version.security_patch | Android 安全补丁程序级别 |
    | ro.product.model                | 型号               |
    | ro.product.brand                | 品牌               |
    | ro.product.name                 | 设备名              |
    | ro.product.board                | 处理器型号            |
    | ro.product.cpu.abilist          | CPU 支持的 abi 列表   |
    | dalvik.vm.heapsize              | 每个应用程序的内存限制      
    
    

## [adb logcat](https://developer.android.com/studio/command-line/logcat?hl=zh-cn) 
* adb logcat //查看设备所有日志
* adb logcat -s TagName //仅查看该标签（TagName）日志
* adb logcat > /Users/listenergao/Desktop/log.txt  //将日志信息输出到指定文件中
* adb wait-for-device logcat -b all -v time > /Users/listenergao/Desktop/log.txt
    说明：wait-for-device可以去掉，主要目的是当设备连接上后自动启动抓取，没有设备也不会自动断开。-b中的b=buffer，-b all代表抓取所有buffer log，-v time是指在log中追加时间戳信息。

## adb 命令模拟按键/输入
* adb shell input keyevent \<keycode>
    如：adb shell input keyevent 4 //相当于点击了返回键
    
    | KeyCode | 含义           |
    |:-------:|:------------:|
    | 3       | HOME 键       |
    | 4       | 返回键          |
    | 5       | 打开拨号应用       |
    | 6       | 挂断电话         |
    | 24      | 增加音量         |
    | 25      | 降低音量         |
    | 26      | 电源键          |
    | 27      | 拍照           |
    | 64      | 打开浏览器        |
    | 82      | 菜单键          |
    | 85      | 播放/暂停        |
    | 86      | 停止播放         |
    | 87      | 播放下一首        |
    | 88      | 播放上一首        |
    | 122     | 移动光标到行首或列表顶部 |
    | 123     | 移动光标到行末或列表底部 |
    | 126     | 恢复播放         |
    | 127     | 暂停播放         |
    | 164     | 静音           |
    | 176     | 打开系统设置       |
    | 187     | 切换应用         |
    | 207     | 打开联系人        |
    | 208     | 打开日历         |
    | 209     | 打开音乐         |
    | 210     | 打开计算器        |
    | 220     | 降低屏幕亮度       |
    
## 系统页面跳转
* adb shell am start com.android.settings/com.android.settings.Settings //跳转设置页面


## adb 卸载预置应用
* adb shell pm uninstall [-k] [--user USER_ID] 包名

    * 卸载预置应用，无需root权限。
    * -k 卸载应用且保留数据与缓存，如果不加 -k 则全部删除。
    * --user 指定用户 id，Android 系统支持多个用户，默认用户只有一个，id=0。
    * 例如，卸载360浏览器：adb shell pm uninstall -k --user 0 com.qihoo.browser