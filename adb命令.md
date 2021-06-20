# adb 常用命令

## 一、设备连接
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


## 二、常用命令
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

## 三、查看设备信息
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
    
    

## 四、日志打印

Android 系统的日志分为两部分，底层的 Linux 内核日志输出到 /proc/kmsg，Android 的日志输出到 /dev/log。

**常用命令：**

* adb logcat //查看设备所有日志
* adb logcat -s TagName //仅查看该标签（TagName）日志
* adb logcat > /Users/listenergao/Desktop/log.txt  //将日志信息输出到指定文件中
* adb wait-for-device logcat -b all -v time > /Users/listenergao/Desktop/log.txt
    说明：wait-for-device可以去掉，主要目的是当设备连接上后自动启动抓取，没有设备也不会自动断开。-b中的b=buffer，-b all代表抓取所有buffer log，-v time是指在log中追加时间戳信息。
    
#### 1. Android 日志

查看 Android 日志的基本命令格式：

    adb logcat [option] [filter-specs]
如果需要停止 logcat 日志打印，可以按 control + c （mac）或 ctrl + c （win）停止日志输出。
##### 1.1   按日志级别过滤日志

按级别过滤日志的基本命令格式：

    adb logcat [filter-specs]
    
Android 的日志分为如下几个优先级（priority）如下：

| 级别  | 含义  |
|:---:|:---:|
| *:V | 过滤只显示 Verbose 及以上级别(优先级最低) |
| *:D | 过滤只显示 Debug 及以上级别 |
| *:I | 过滤只显示 Info 及以上级别 |
| *:W | 过滤只显示 Warning 及以上级别 |
| *:E | 过滤只显示 Error 及以上级别 |
| *:F | 过滤只显示 Fatal 及以上级别 |
| *:S | 过滤只显示 Silent 及以上级别(优先级最高，什么也不输出) |

按某级别过滤日志则会将该级别及以上的日志输出，如：

    adb logcat *:W

会将 Warning、Error、Fatal 和 Silent 日志输出。

（**注：** 在 macOS 下需要给 *:W 这样以 * 作为 tag 的参数加单引号，如 adb logcat '*:W'，不然会报错 no matches found: *:W。）

##### 1.2 按 tag 和级别过滤日志

按 tag 和级别过滤日志的基本命令格式：

    adb logcat [tag:level] [tag:level] ...
    
    
```
adb logcat ActivityManager:I MyApp:D *:S
// 表示输出 tag ActivityManager 的 Info 以上级别日志，输出 tag MyApp 的 Debug 以上级别日志，及其它 tag 的 Silent 级别日志（即屏蔽其它 tag 日志）。
```

##### 1.3 将日志格式化输出

可以用 adb logcat -v <format> 选项指定日志输出格式。

日志支持按以下几种 <format>：


| 参数  | 显示格式 |
|:---:|:----:|
|  brief  |  \<priority>/\<tag>(\<pid>): \<message>  |
|  process  |  \<priority>(\<pid>): \<message>  |
|  tag  |  \<priority>/\<tag>: \<message>  |
|  raw  |  \<message>  |
|  time  |  \<datetime> \<priority>/\<tag>(\<pid>): \<message>  |
|  threadtime  |  \<datetime> \<pid> \<tid> \<priority> \<tag>: \<message>  |
|  long  |  [ \<datetime> \<pid>:\<tid> \<priority>/\<tag> ] \<message>  |

日志格式默认为 brief，指定格式可与上面的过滤同时使用。比如：

    adb logcat -v long ActivityManager:I *:S

##### 1.4 清空已存在的日志

    adb logcat -c

##### 1.5 将日志显示在控制台

    adb logcat -d
    
##### 1.6 将日志输出到文件

    adb logcat -f <file-path>
    
    // 如：将所有日志信息输出到指定文件中
    adb logcat > /Users/listenergao/Desktop/log.txt
    
##### 1.7 加载一个可使用的日志缓冲区供查看

    adb logcat -b <Buffer>
    
Android log 输出量巨大，特别是通信系统的log，因此，Android把log输出到不同的缓冲区中，目前定义了四个log缓冲区：


| 缓冲区 | 含义  |
|:---:|:---:|
|  Radio  |  输出通信系统的 log  |
|  System  |  输出系统组件的 log  |
|  Event  |  输出 event 模块的 log  |
|  Main  |  所有 java 层的 log 以及不属于上面3层的 log  |

缓冲区主要给系统组件使用，一般的应用不需要关心，应用的log都输出到main缓冲区中。默认log输出（不指定缓冲区的情况下）是输出System和Main缓冲区的log。


```
// wait-for-device可以去掉，主要目的是当设备连接上后自动启动抓取，没有设备也不会自动断开。-b中的b=buffer，-b all代表抓取所有buffer log，-v time是指在log中追加时间戳信息。
adb wait-for-device logcat -b all -v time > /Users/listenergao/Desktop/log.txt
```

##### 1.8 打印置顶日志缓冲区大小

    adb logcat -g

#### 2. 内核日志


```
adb shell dmesg

输出示例：
<6>[14201.684016] PM: noirq resume of devices complete after 0.982 msecs
<6>[14201.685525] PM: early resume of devices complete after 0.838 msecs
<6>[14201.753642] PM: resume of devices complete after 68.106 msecs
<4>[14201.755954] Restarting tasks ... done.
<6>[14201.771229] PM: suspend exit 2016-08-28 13:31:32.679217193 UTC
<6>[14201.872373] PM: suspend entry 2016-08-28 13:31:32.780363596 UTC
<6>[14201.872498] PM: Syncing filesystems ... done.

中括号里的 [14201.684016] 代表内核开始启动后的时间，单位为秒。

通过内核日志我们可以做一些事情，比如衡量内核启动时间，
在系统启动完毕后的内核日志里找到 Freeing init memory 那一行前面的时间就是。
```



## 五、adb 命令模拟按键/输入

在 adb shell 里有个很实用的命令叫 input，通过它可以做一些有趣的事情。 可以执行adb shell input命令查看完整 help 信息如下：

```
Usage: input [<source>] <command> [<arg>...]

The sources are:
      dpad
      keyboard
      mouse
      touchpad
      gamepad
      touchnavigation
      joystick
      touchscreen
      stylus
      trackball

The commands and default sources are:
      text <string> (Default: touchscreen)
      keyevent [--longpress] <key code number or name> ... (Default: keyboard)
      tap <x> <y> (Default: touchscreen)
      swipe <x1> <y1> <x2> <y2> [duration(ms)] (Default: touchscreen)
      draganddrop <x1> <y1> <x2> <y2> [duration(ms)] (Default: touchscreen)
      press (Default: trackball)
      roll <dx> <dy> (Default: trackball)
```

比如使用 adb shell input keyevent \<keycode> 命令，根据 keycode 实现不同的功能，完整的 keycode 列表详见 [KeyEvent](https://developer.android.com/reference/android/view/KeyEvent.html)，摘引部分常用的 keycode 如下：
    
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

**input 命令用法示例**

##### 1. 电源键
    adb shell input keyevent 26
    
##### 2. 菜单键
    adb shell input keyevent 82

##### 3. HOME 键
    adb shell input keyevent 3

##### 4. 返回键
    adb shell input keyevent 4

##### 5. 音量控制
* 增加音量

```
adb shell input keyevent 24
```
* 降低音量

```
adb shell input keyevent 25
```
* 静音

```
adb shell input keyevent 164
```

##### 6. 媒体控制
* 播放/暂停


```
adb shell input keyevent 85
```
* 停止播放

```
adb shell input keyevent 86
```
* 下一首

```
adb shell input keyevent 87
```
* 上一首

```
adb shell input keyevent 88
```
* 恢复播放

```
adb shell input keyevent 126
```
* 暂停播放

```
adb shell input keyevent 127
```
##### 7. 输入文本

当焦点处于某文本框时，可以通过 input 命令来输入文本。

```
adb shell input text hello_world
```

## 六、系统页面跳转
* adb shell am start com.android.settings/com.android.settings.Settings //跳转设置页面

## 七、应用管理相关
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

应用卸载的基本命令格式：

    adb uninstall [-k] <package-name>
    // <package-name> 表示应用的包名，-k 参数可选，表示卸载应用但保留数据和缓存目录。
    
**预置应用卸载**

```
adb shell pm uninstall [-k] [--user USER_ID] 包名
    
1. 卸载预置应用，无需root权限。
2. -k 卸载应用且保留数据与缓存，如果不加 -k 则全部删除。
3. --user 指定用户 id，Android 系统支持多个用户，默认用户只有一个，id=0。
4. 例如，卸载360浏览器：adb shell pm uninstall -k --user 0 com.qihoo.browser
```

    

**4. 清除应用数据与缓存**

    adb shell pm clear <package-name>
    // <package-name> 表示应用名包，这条命令的效果相当于在设置里的应用信息界面点击了「清除缓存」和「清除数据」。

**5. 查看前台 Activity**

    adb shell dumpsys activity activities | grep mFocusedActivity

**6. 查看正在运行的 Services**

    adb shell dumpsys activity services [<package-name>]
    // <package-name> 参数不是必须的，指定 <package-name> 表示查看与某个包名相关的 Services，
    不指定表示查看所有 Services。<package-name> 不一定要给出完整的包名，
    可以只给一部分，那么所给包名相关的 Services 都会列出来。

**7. 查看应用详细信息**

    adb shell dumpsys package <package-name>
    // <package-name> 表示应用包名。运行次命令的输出中包含很多信息，
    包括 Activity Resolver Table、Registered ContentProviders、包名、userId、
    安装后的文件资源代码等路径、版本信息、权限信息和授予状态、签名版本信息等。


**8. 查看应用安装路径**

    adb shell pm path <package-name>


## 八、应用交互相关

与应用交互主要是使用 am \<command> 命令，常用的 \<command> 如下：


| Command                         | 含义                       |
|:-------------------------------:|:------------------------:|
| start [options] \<intent>        | 启动 \<intent> 指定的 Activity |
| startservice [options] \<intent> | 启动 \<intent> 指定的 Service  |
| broadcast [options] \<intent>    | 发送 \<intent> 指定的广播        |
| force-stop \<package-name>       | 停止 \<package-name> 相关的进程  |

\<intent> 参数很灵活，和写 Android 程序时代码里的 Intent 相对应。

决定 intent 对象的选项如下：


| 参数             | 含义                                                  |
|:--------------:|:---------------------------------------------------:|
| -a \<action>    | 指定 action，比如 android.intent.action.VIEW             |
| -c \<category>  | 指定 category，比如 android.intent.category.APP_CONTACTS |
| -n \<component> | 指定完整 component 名，用于明确指定启动哪个 Activity                |

\<intent> 中可以携带参数，就像写代码时的 Bundle 一样：

| 参数 | 含义 |
| :-: | :-: |
| --esn \<extra-key> | null 值(只有 key 名) |
| -e\|--es \<extra-key> \<extra-string-value> | string 值 |
| --ez \<extra-key> \<extra-boolean-value> |  boolean 值 |
| --ei \<extra-key> \<extra-int-value> | integer 值 |
| --el \<extra-key> \<extra-long-value> | long 值 |
| --ef \<extra-key> \<extra-float-value> | float 值 |
| --eu \<extra-key> \<extra-uri-value> | URI |
| --ecn \<extra-key> \<extra-component-name-value> | component name |
| --eia \<extra-key> \<extra-int-value>[,<extra-int-value...] | integer 数组 |
| --ela \<extra-key> \<extra-long-value>[,<extra-long-value...] | long 数组 |

#### 1. 启动应用/调起 Activity

    adb shell am start [options] <intent>
    
例如：

```
adb shell am start -a android.settings.SETTINGS                   // 打开系统设置页面
adb shell am start -a android.intent.action.DIAL -d tel:10086     // 打开拨号页面
adb shell am start -n com.android.mms/.ui.ConversationList        // 打开短信会话列表
```

options 是一些改变其行为的选项，支持可选参数及含义如下：


| 参数  | 含义   |
|:---:|:----:|
| -D | 启用调试 |
| -W | 等待启动完成 |
| --start-profiler file | 启动分析器并将结果发送到 file |
| -P file | 类似于 --start-profiler，但当应用进入空闲状态时分析停止 |
| -R count | 重复 Activity 启动次数 |
| -S | 启动 Activity 前强行停止目标应用 |
| --opengl-trace | 	启用 OpenGL 函数的跟踪 |
| --user user_id \| current | 指定要作为哪个用户运行；如果未指定，则作为当前用户运行 |


#### 2. 调起 Service

    adb shell am startservice [options] <intent>
    
一个典型的用例是如果设备上原本应该显示虚拟按键但是没有显示，可以试试这个：

    adb shell am startservice -n com.android.systemui/.SystemUIService

#### 3. 停止 Service

    adb shell am stopservice [options] <intent>
    
#### 4. 发送广播

    adb shell am broadcast [options] <INTENT>
    
可以向所有组件广播，也可以只向指定组件广播。

```
// 向所有组件广播 BOOT_COMPLETED
adb shell am broadcast -a android.intent.action.BOOT_COMPLETED

// 向 com.android.receiver.test/.BootCompletedReceiver 广播 BOOT_COMPLETED  
adb shell am broadcast -a android.intent.action.BOOT_COMPLETED -n com.android.receiver.test/.BootCompletedReceiver
```

这类用法在测试的时候很实用，比如某个广播的场景很难制造，可以考虑通过这种方式来发送广播。

既能发送系统预定义的广播，也能发送自定义广播。如下是部分系统预定义广播及正常触发时机（以下广播均可使用 adb 触发）：


| action | 触发时机 |
|:------:|:----:|
| android.net.conn.CONNECTIVITY_CHANGE | 网络连接发生变化 |
| android.intent.action.SCREEN_ON | 屏幕点亮 |
| android.intent.action.SCREEN_OFF | 屏幕熄灭 |
| android.intent.action.BATTERY_LOW | 电量低，会弹出电量低提示框 |
| android.intent.action.BATTERY_OKAY | 电量恢复了 |
| android.intent.action.BOOT_COMPLETED | 设备启动完毕 |
| android.intent.action.DEVICE_STORAGE_LOW | 存储空间过低 |
| android.intent.action.DEVICE_STORAGE_OK | 存储空间恢复 |
| android.intent.action.PACKAGE_ADDED | 安装了新的应用 |
| android.net.wifi.STATE_CHANGE | WiFi连接状态发生变化 |
| android.net.wifi.WIFI_STATE_CHANGED | WiFi状态变为启用/关闭/正在启动/正在关闭/未知 |
| android.intent.action.BATTERY_CHANGED | 电池电量发生变化 |
| android.intent.action.INPUT_METHOD_CHANGED | 系统输入法发生变化 |
| android.intent.action.ACTION_POWER_CONNECTED | 外部电源连接 |
| android.intent.action.ACTION_POWER_DISCONNECTED | 外部电源断开连接 |
| android.intent.action.DREAMING_STARTED | 系统开始休眠 |
| android.intent.action.DREAMING_STOPPED | 系统停止休眠 |
| android.intent.action.WALLPAPER_CHANGED | 壁纸发生变化 |
| android.intent.action.HEADSET_PLUG | 插入耳机 |
| android.intent.action.MEDIA_UNMOUNTED | 卸载外部介质 |
| android.intent.action.MEDIA_MOUNTED | 挂载外部介质 |
| android.os.action.POWER_SAVE_MODE_CHANGED | 省电模式开启 |

#### 5. 强制停止应用

    adb shell am force-stop <packagename>

#### 6. 收紧内存

    adb shell am send-trim-memory  <pid> <level>
    
**参数说明：**
* pid: 进程 ID。
* level:HIDDEN、RUNNING_MODERATE、BACKGROUND、RUNNING_LOW、MODERATE、RUNNING_CRITICAL、COMPLETE。


## 九、文件管理相关

#### 1. 从模拟器/设备下载指定的文件到计算机

从模拟器/设备下载指定的文件到计算机的基本命令格式是：

    adb pull <remote> [local]

**参数说明：**
* remote: 模拟器/设备里的文件路径。
* local:计算机上的目录，参数可以省略，默认复制到当前目录。

例如，将 /sdcard/music.mp4 下载到计算机的当前目录：

    adb pull /sdcard/music.mp4

将 /sdcard/music.mp4 下载到计算机桌面的 test 目录(目录需存在)：

    adb pull /sdcard/music.mp4 /Users/ListenerGao/Desktop/test


#### 2. 将指定的文件从计算机上传到模拟器/设备

将指定的文件从计算机上传到模拟器/设备的基本命令格式是：

    adb push <local> <remote>
    
**参数说明：**

* local:计算机上的文件路径。
* remote: 模拟器/设备里的目录。

例如，将 /Users/ListenerGao/Desktop/test/music.mp4 下载到设备的/sdcard/music/目录：

    adb push /Users/ListenerGao/Desktop/test/music.mp4 /sdcard/music/


#### 3. 查看指定目录的内容

列出模拟器/设备上指定目录的内容的基本命令格式是：

    adb shell ls [options] <directory>

\<directory> 表示指定目录，可以省略，表示列出根目录下的所有文件和目录。 adb shell ls 后面可以跟一些可选参数进行过滤查看不同的列表，可用参数及含义如下：


| 参数  | 含义  |
|:---:|:---:|
|  无   |   显示目录下的所有文件和目录  |
|  -a  |  显示目录下的所有文件(包括隐藏的)   |
|  -i  |   显示目录下的所有文件和索引编号  |
|  -s  |  显示目录下的所有文件和文件大小   |
|  -n  |  显示目录下的所有文件及其 UID和 GID   |
|  -R  |  显示目录下的所有子目录中的文件   |

#### 4. 切换到目标目录

    adb shell cd <directory>
    
第一步：执行adb shell命令； 第二步：执行cd <directory>命令切换到目标目录。

#### 5. 删除文件或目录

    adb shell rm [options] <files or directory>
    
* 第一步：执行adb shell命令； 
* 第二步：执行rm [options] <files or directory>命令删除文件或目录。

rm 后面可以跟一些可选参数进行不同的操作，可用参数及含义如下：


| 参数  | 含义  |
|:---:|:---:|
|  无   |  删除文件   |
|  -f   |   强制删除文件，系统不提示  |
|   -r  |   强制删除指定目录中的所有文件和子目录  |
|   -d  |   删除指定目录，即使它是一个非空目录  |
|  -i   |   交互式删除，删除前提示  |

rm -d 等同于 rmdir 命令，有些版本不包含-d 参数。

#### 6. 创建目录

    adb shell mkdir [options] <directory-name>
    
* 第一步：执行adb shell命令； 
* 第二步：执行mkdir [options] <directory-name>命令创建目录。

mkdir 后面可以跟一些可选参数进行不同的操作，可用参数及含义如下：


| 参数  | 含义  |
|:---:|:---:|
| 无 | 创建指定目录 |
| -m | 创建指定目录并赋予读写权限 |
| -p | 创建指定目录及其父目录 |

#### 7. 创建空文件或改变文件时间戳

    adb shell touch [options] <file>
    
* 第一步：执行adb shell命令； 
* 第二步：执行touch [options] <file>命令创建空文件或改变文件时间戳。

可通过ls -n <directory> 命令查看文件的时间。

#### 8. 输出当前目录路径

    adb shell pwd

* 第一步：执行adb shell命令； 
* 第二步：执行pwd命令输出当前目录路径。

#### 9. 复制文件或目录

    adb shell cp [options] <source> <dest>

* 第一步：执行adb shell命令； 
* 第二步：执行cp [options] <source> <dest>命令复制文件和目录。

**参数说明：**
* source:源文件路径。
* dest: 目标文件路径。

#### 10. 移动或重命名文件

    adb shell mv [options] <source> <dest>
    
* 第一步：执行adb shell命令； 
* 第二步：执行mv [options] <source> <dest>命令移动或重命名文件。

**参数说明：**
* source:源文件路径。
* dest: 目标文件路径。

## 十、网络管理相关

#### 1. 查看网络统计信息


```
// 查看网络统计信息
adb shell netstat

// 将网络统计信息输出到指定文件
adb shell netstat><file-path>

// 如：将网络日志输出到/Users/ListenerGao/Desktop/test/netstat.log中
adb shell netstat>/Users/ListenerGao/Desktop/test/netstat.log
```

#### 2. 测试网络延迟


```
// ping 命令格式如下
adb shell ping [-aAbBdDfhLnOqrRUvV] [-c count] [-i interval] [-I interface]
[-m mark] [-M pmtudisc_option] [-l preload] [-p pattern] [-Q tos]
[-s packetsize] [-S sndbuf] [-t ttl] [-T timestamp_option]
[-w deadline] [-W timeout] [hop1 ...] destination

// ping 一个域名，不结束的话会一直 ping 下去，可以 control + c (mac), ctrl + c (win) 停止 ping 操作。
adb shell ping www.google.com

// 指定 ping 的次数
adb shell ping -c 4 www.google.com
```

#### 3. 通过配置文件配置和管理网络连接


```
adb shell netcfg [<interface> {dhcp|up|down}]    // Android 6.0 以上系统
```

#### 4. 显示、操作路由、设备、策略路由和隧道

    adb shell ip [ options ] object
    
* options := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |-f[amily] { inet | inet6 | ipx | dnet | link } |-l[oops] { maximum-addr-flush-attempts } |-o[neline] | -t[imestamp] | -b[atch] [filename] |-rc[vbuf] [size]}
* object := { link | addr | addrlabel | route | rule | neigh | ntable |tunnel | tuntap | maddr | mroute | mrule | monitor | xfrm |netns | l2tp }

options 是一些修改ip行为或者改变其输出的选项。所有的选项都是以-字符开头，分为长、短两种形式，支持的可选参数及含义如下：


| 参数  | 含义  |
|:---:|:---:|
| -V,-Version | 打印ip的版本并退出 |
| -s,-stats,-statistics | 输出更为详尽的信息(如果这个选项出现两次或者多次，输出的信息将更为详尽) |
| -f,-family | 强调使用的协议种类(包括：inet、inet6或者link) |
| -4 | 	是-family inet的简写 |
| -6 | 是-family inet6的简写 |
| -0 | 是-family link的简写 |
| -o,-oneline | 对每行记录都使用单行输出，回行用字符代替 |
| -r,-resolve | 查询域名解析系统，用获得的主机名代替主机IP地址 |

object 是你要管理或者获取信息的对象。目前ip认识的对象包括：


| 参数  | 含义  |
|:---:|:---:|
| link | 网络设备 |
| address | 一个设备的协议(IP或者IPV6)地址 |
| neighbour | ARP或者NDISC缓冲区条目 |
| route | 路由表条目 |
| rule | 路由策略数据库中的规则 |
| maddress | 多播地址 |
| mroute | 多播路由缓冲区条目 |
| tuntap | 管理 TUN/TAP 设备 |
| netns | 管理网络空间 |


```
// 查看 WIFI IP 地址
adb shell ip -f inet addr show wlan0
```


    