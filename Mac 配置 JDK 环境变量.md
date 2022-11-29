# Mac 配置 JDK 环境变量

* **[JDK 各版本下载地址](https://www.oracle.com/cn/java/technologies/downloads/archive/)**
* **JDK 安装**
    安装路径是默认的，只是 JDK 版本好的区别，如 JDK8：`/Library/Java/JavaVirtualMachines/jdk1.8.0_341.jdk/Contents/Home`
    
* **配置 JAVA 环境变量**

1. 打开终端

    * 如果是第一次配置的话，使用命令 `touch .bash_profile` 创建一个名为 .bash_profile 隐藏配置文件;
    * 如果不是第一次配置环境变量，使用命令 `open .bash_profile` 打开配置文件；

    
1. 在配置文件中输入一下内容：

    ```
    // 安装单个 JDK 版本 
    export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_341.jdk/Contents/Home
    
    // 安装多个 JDK 版本，如 JDK7 和 JDK8
    export JAVA_7_HOME=/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home
    export JAVA_8_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_341.jdk/Contents/Home
     
    alias jdk7="export JAVA_HOME=$JAVA_7_HOME" #编辑一个命令jdk7，输入则切换到jdk1.7
    alias jdk8="export JAVA_HOME=$JAVA_8_HOME" #编辑一个命令jdk8，输入则切换jdk1.8
     
    export JAVA_HOME=`/usr/libexec/java_home`  #最后安装的版本，这样当自动更新时，始终指向最新版本

    
    ```

3. 执行命令 `source .bash_profile` 使配置文件生效


    

