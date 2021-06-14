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

1. 在 Android 设备上安装一个终端模拟器（可通过[Terminal Emulator for Android Downloads](https://jackpal.github.io/Android-Terminal-Emulator/)下载）。
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


## 应用管理
**1. 查看应用列表**

查看应用列表基本命令格式：

    adb shell pm list packages [-f] [-d] [-e] [-s] [-3] [-i] [-u] [--user USER_ID] [FILTER]
    
adb shell pm list packages 后面可以添加一些可选参数查看不通的列表，可选参数及含义如下：


| 参数       | 含义                    |
|:--------:|:---------------------:|
| 无        | 显示所有应用                |
| -f       | 显示应用关联的 apk 文件        |
| -d       | 只显示 disabled 的应用      |
| -e       | 只显示 enabled 应用        |
| -s       | 只显示系统应用               |
| -3       | 只显示第三方应用              |
| -i       | 显示应用的 installer       |
| -u       | 包含已卸载应用               |
| \<filter> | 显示包名包含 \<filter> 字符串应用 |

**示例命令**

* adb shell pm list packages //查看所有应用
* adb shell pm list packages -s //查看系统应用
* adb shell pm list packages -3 //查看第三方应用
* adb shell pm list packages huawei //查看包名包含字符串 huawei 的应用列表


**2. 安装应用**

安装应用基本命令格式：

    adb install [-l] [-r] [-t] [-s] [-d] [-g] <apk-file>
    
adb install 后面可以添加一些可选参数来控制安装 APK 的行为，可选参数及含义如下：


| 参数  | 含义                   |
|:---:|:--------------------:|
| -l  | 将应用安装到保护目录 /mnt/asec |
| -r  | 允许覆盖安装               |
| -t  | 允许安装 debug 应用        |
| -s  | 将应用安装到 sdcard        |
| -d  | 允许降级覆盖安装             |
| -g  | 安装时，授予所有权限           |

运行完命令后可以看到输出内容，包含安装进度和状态，安装状态如下：
* Success：表示安装成功。
* Failure：表示安装失败。APK 安装失败的情况有很多，Failure 状态之后，控制台会有相关错误代码输出。可以参考下表处理异常代码：


| 输出代码                          | 含义                 | 解决方法                                                       |
|:-----------------------------:|:------------------:|:----------------------------------------------------------:|
| INSTALL_FAILED_ALREADY_EXISTS | 应用已经存在，或卸载了但没卸载干净。 | adb install 时使用 -r 参数，或者先 adb uninstall <packagename> 再安装。 |
| INSTALL_FAILED_INVALID_APK | 无效的 APK 文件。 |  |
| INSTALL_FAILED_INVALID_URI | 无效的 APK 文件名。 | 确保 APK 文件名里无中文。 |
| INSTALL_FAILED_INSUFFICIENT_STORAGE | 存储空间不足。 | 清理存储空间。 |
| INSTALL_FAILED_DUPLICATE_PACKAGE | 已经存在同名程序。 |  |
| INSTALL_FAILED_NO_SHARED_USER | 请求的共享用户不存在。 |  |
| INSTALL_FAILED_UPDATE_INCOMPATIBLE | 以前安装过同名应用，但卸载时数据没有移除；或者已安装该应用，但签名不一致。 | 先 adb uninstall <packagename> 再安装。 |
| INSTALL_FAILED_SHARED_USER_INCOMPATIBLE | 请求的共享用户存在但签名不一致。 |  |
| INSTALL_FAILED_MISSING_SHARED_LIBRARY | 安装包使用了设备上不可用的共享库。 |  |
| INSTALL_FAILED_REPLACE_COULDNT_DELETE | 替换时无法删除。 |  |
| INSTALL_FAILED_DEXOPT | dex 优化验证失败或空间不足。 |  |
| INSTALL_FAILED_OLDER_SDK | 设备系统版本低于应用要求。 |  |
| INSTALL_FAILED_CONFLICTING_PROVIDER | 设备里已经存在与应用里同名的 content provider	。 |  |
| INSTALL_FAILED_NEWER_SDK | 设备系统版本高于应用要求。 |  |
| INSTALL_FAILED_TEST_ONLY | 应用是 test-only 的，但安装时没有指定 -t 参数。 | 确定安装 debug 应用，使用 adb install -r <apk-file> 安装。 |
| INSTALL_FAILED_CPU_ABI_INCOMPATIBLE | 包含不兼容设备 CPU 应用程序二进制接口的 native code	。 |  |
| INSTALL_FAILED_MISSING_FEATURE | 应用使用了设备不可用的功能。 |  |
| INSTALL_FAILED_CONTAINER_ERROR | 1. sdcard 访问失败; 2. 应用签名与 ROM 签名一致，被当作内置应用。 | 1. 确认 sdcard 可用，或者安装到内置存储; 2. 打包时不与 ROM 使用相同签名。 |
| INSTALL_FAILED_INVALID_INSTALL_LOCATION | 1. 不能安装到指定位置; 2. 应用签名与 ROM 签名一致，被当作内置应用。 | 1. 切换安装位置，添加或删除 -s 参数; 2. 打包时不与 ROM 使用相同签名。 |
| INSTALL_FAILED_MEDIA_UNAVAILABLE | 安装位置不可用。 | 一般为 sdcard，确认 sdcard 可用或安装到内置存储。 |
| INSTALL_FAILED_VERIFICATION_TIMEOUT | 验证安装包超时。 |  |
| INSTALL_FAILED_VERIFICATION_FAILURE | 验证安装包失败。 |  |
| INSTALL_FAILED_PACKAGE_CHANGED | 应用与调用程序期望的不一致。 |  |
| INSTALL_FAILED_UID_CHANGED | 以前安装过该应用，与本次分配的 UID 不一致。 | 清除以前安装过的残留文件。 |
| INSTALL_FAILED_VERSION_DOWNGRADE | 已经安装了该应用更高版本。 | 使用 -d 参数。 |
| INSTALL_FAILED_PERMISSION_MODEL_DOWNGRADE | 已安装 target SDK 支持运行时权限的同名应用，要安装的版本不支持运行时权限。 |  |
| INSTALL_PARSE_FAILED_NOT_APK | 指定路径不是文件，或不是以 .apk 结尾。 |  |
| INSTALL_PARSE_FAILED_BAD_MANIFEST | 无法解析的 AndroidManifest.xml 文件。 |  |
| INSTALL_PARSE_FAILED_UNEXPECTED_EXCEPTION | 解析器遇到异常。 |  |
| INSTALL_PARSE_FAILED_NO_CERTIFICATES | 安装包没有签名。 |  |
| INSTALL_PARSE_FAILED_INCONSISTENT_CERTIFICATES | 已安装该应用，且签名与 APK 文件不一致。 | 先卸载设备上的该应用，再安装。 |
| INSTALL_PARSE_FAILED_CERTIFICATE_ENCODING | 解析 APK 文件时遇到 CertificateEncodingException。 |  |
| INSTALL_PARSE_FAILED_BAD_PACKAGE_NAME | manifest 文件里没有或者使用了无效的包名。 |  |
| INSTALL_PARSE_FAILED_BAD_SHARED_USER_ID | manifest 文件里指定了无效的共享用户 ID。 |  |
| INSTALL_PARSE_FAILED_MANIFEST_MALFORMED | 解析 manifest 文件时遇到结构性错误。 |  |
| INSTALL_PARSE_FAILED_MANIFEST_EMPTY | 在 manifest 文件里找不到找可操作标签（instrumentation 或 application）。 |  |
| INSTALL_FAILED_INTERNAL_ERROR | 因系统问题安装失败。 |  |
| INSTALL_FAILED_USER_RESTRICTED | 用户被限制安装应用。 |  |
| INSTALL_FAILED_DUPLICATE_PERMISSION | 应用尝试定义一个已经存在的权限名称。 |  |
| INSTALL_FAILED_NO_MATCHING_ABIS | 应用包含设备的应用程序二进制接口不支持的 native code。 |  |
| INSTALL_CANCELED_BY_USER | 应用安装需要在设备上确认，但未操作设备或点了取消。 | 在设备上同意安装。 |
| INSTALL_FAILED_ACWF_INCOMPATIBLE | 应用程序与设备不兼容。 |  |
| INSTALL_FAILED_TEST_ONLY | APK 文件是使用 Android Studio 直接 RUN 编译出来的文件。 | 通过 Gradle 的 assembleDebug 或 assembleRelease 重新编译，或者 Generate Signed APK。 |
| does not contain AndroidManifest.xml | 无效的 APK 文件。 |  |
| is not a valid zip file	 | 无效的 APK 文件。 |  |
| Offline | 设备未连接成功。 | 先将设备与 adb 连接成功。 |
| unauthorized | 设备未授权允许调试。 |  |
| error: device not found | 没有连接成功的设备。 | 先将设备与 adb 连接成功。 |
| protocol failure | 设备已断开连接。 | 先将设备与 adb 连接成功。 |
| Unknown option: -s | Android 2.2 以下不支持安装到 sdcard。 | 不使用 -s 参数。 |
| No space left on device | 存储空间不足。 | 清理存储空间。 |
| Permission denied ... sdcard ... | sdcard 不可用。 |  |
| signatures do not match the previously installed version; ignoring! |  已安装该应用且签名不一致。 | 先卸载设备上的该应用，再安装。 |




**3. 卸载应用**
**4. 清除应用数据与缓存**
**5. 查看前台 Activity**
**6. 查看正在运行的 Services**
**7. 查看应用详细信息**
**8. 查看应用安装路径**

























