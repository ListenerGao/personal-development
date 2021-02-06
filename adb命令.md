# adb 常用命令

## 常用命令
* adb devices  //查看连接设备
* adb -s emulator-5554 shell //连接指定设备 emulator-5554 为设备名称
* adb install demo.apk //安装应用
* adb install -r demo.apk //（覆盖）安装应用
* adb uninstall \<package_name> //卸载应用，需要指定包名
* adb uninstall -k \<package_name> //卸载应用，保留数据和缓存文件
* adb shell pm clear \<package_name> //清除应用数据与缓存
* adb shell am force-stop \<package_name> //强制停止应用
* adb shell pm list packages //列出手机安装所有应用的包名
* adb shell pm list packages -3 //列出除了系统应用的第三方应用包名
* adb shell dumpsys package \<package_name> //根据包名查看应用信息
* adb shell dumpsys meminfo //查看内存使用情况Memory Usage
* adb shell top -m 10 //查看占用内存前10的应用
* adb shell am start \<package_name>/.Activity(Launcher Activity) //启动应用
* adb push <local> <remote> //从本地复制文件到设备
* adb pull <remote> <local> //从设备复制文件到本地

## 查看设备信息
* adb shell getprop ro.build.version.release //获取手机系统版本
* adb shell getprop ro.build.version.sdk //获取手机系统api版本
* adb shell getprop ro.product.model //获取手机设备型号
* adb shell getprop ro.product.brand //获取手机厂商名称
* adb shell cat /sys/class/net/wlan0/address //获取手机MAC地址
* adb shell df //获取手机存储信息
* adb shell wm density //获取手机物理密度
* adb shell wm size //获取手机分辨率

## [adb logcat](https://developer.android.com/studio/command-line/logcat?hl=zh-cn) 相关命令
* adb logcat //查看设备所有日志
* adb logcat -s TagName //仅查看该标签（TagName）日志
* adb logcat > /Users/listenergao/Desktop/log.txt  //将日志信息输出到指定文件中