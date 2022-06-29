# 从 Android 手机提取已安装应用

1. 手机打开 USB 调试模式，将手机与电脑通过数据线连接；
2. 确定电脑是否成功连接手机，打开终端，输入 adb devices （查看已连接设备），当连接成功时会显示出一连接设备名称。（ps：需要配置adb，可自行搜索）
3. 执行 adb shell pm list packages 指令，查看手机上所有应用的包名，找到你要获取应用的包名。（ps：可以通过 LibChecker 应用查看已安装应用的包名）
4. 执行 adb shell pm path 包名，获取应用安装路径；如：adb shell pm path com.tencent.mobileqq (获取 QQ 安装应用的路径)
5. 执行 adb pull 安装包路径 输出电脑端路径；如：adb pull /data/app/com.tencent.mobileqq/base.apk /Users/listenergao/Desktop/ ，将 QQ 应用输出保存到电脑桌面上。