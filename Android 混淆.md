# Android 混淆

**[Proguard官方文档](https://developer.android.com/studio/build/shrink-code?hl=zh-cn)**

## 混淆规则：
1. **混淆参数设置**

    ```
    -optimizationpasses 5                       # 代码混淆的压缩比例，值介于0-7，默认5
    -verbose                                    # 混淆时记录日志
    -dontoptimize                               # 不优化输入的类文件
    -dontshrink                                 # 关闭压缩
    -dontpreverify                              # 关闭预校验(作用于Java平台，Android不需要，去掉可加快混淆)
    -dontoptimize                               # 关闭代码优化
    -dontobfuscate                              # 关闭混淆
    -ignorewarnings                             # 忽略警告
    -dontwarn com.squareup.okhttp.**            # 指定类不输出警告信息
    -dontusemixedcaseclassnames                 # 混淆后类型都为小写
    -dontskipnonpubliclibraryclasses            # 不跳过非公共的库的类
    -printmapping mapping.txt                   # 生成原类名与混淆后类名的映射文件mapping.txt
    -useuniqueclassmembernames                  # 把混淆类中的方法名也混淆
    -allowaccessmodification                    # 优化时允许访问并修改有修饰符的类及类的成员
    -renamesourcefileattribute SourceFile       # 将源码中有意义的类名转换成SourceFile，用于混淆具体崩溃代码
    -keepattributes SourceFile,LineNumberTable  # 保留行号
    -keepattributes *Annotation*,InnerClasses,Signature,EnclosingMethod # 避免混淆注解、内部类、泛型、匿名类
    -optimizations !code/simplification/cast,!field/ ,!class/merging/   # 指定混淆时采用的算法
    ```
    
2. **保持不被混淆的设置**

    **语法组成：**
    
    ```
    [保持命令] [类] {
        [成员] 
    }
    ```
    
    **保持命令：**
    
    
    ```
    -keep                           # 防止类和类成员被移除或被混淆；
    -keepnames                      # 防止类和类成员被混淆；
    -keepclassmembers               # 防止类成员被移除或被混淆；
    -keepclassmembernames           # 防止类成员被混淆；
    -keepclasseswithmembers         # 防止拥有该成员的类和类成员被移除或被混淆；
    -keepclasseswithmembernames     # 防止拥有该成员的类和类成员被混淆；
    ```
    
    **类：**
    
    具体的类。

    访问修饰符→ public、private、protected。
    
    通配符(*) → 匹配任意长度字符，但不包含包名分隔符(.)。
    
    通配符(**) → 匹配任意长度字符，且包含包名分隔符(.)。
    
    extends→ 匹配实现了某个父类的子类。
    
    implements→ 匹配实现了某接口的类。
    
    $→ 内部类。
    
    **成员：**
    

    匹配所有构造器→ <init>。
    
    匹配所有域→ <field>。
    
    匹配所有方法→ <methods>。
    
    访问修饰符→ public、private、protected。
    
    除了*和**通配符外，还支持***通配符，匹配任意参数类型。
    
    ...→ 匹配任意长度的任意类型参数，如void test(...)可以匹配不同参数个数的test方法。
    
    **常用混淆规则配置：**
    
    
    ```
    # 不混淆某个类的类名，及类中的内容
    -keep class cn.listenergao.myapp.example.Test { *; }
    
    # 不混淆指定包名下的类名，不包括子包下的类名
    -keep class cn.listenergao.myapp*
    
    # 不混淆指定包名下的类名，及类里的内容
    -keep class cn.listenergao.myapp* {*;}
    
    # 不混淆指定包名下的类名，包括子包下的类名
    -keep class cn.listenergao.myapp**
    
    # 不混淆某个类的子类
    -keep public class * extends cn.listenergao.myapp.base.BaseFragment
    
    # 不混淆实现了某个接口的类
    -keep class * implements cn.listenergao.myapp.dao.DaoImp
    
    # 不混淆类名中包含了"entity"的类，及类中内容
    -keep class **.*entity*.** {*;}
    
    # 不混淆内部类中的所有public内容
    -keep class cn.listenergao.myapp.widget.CustomView$OnClickInterface {
        public *;
    }
    
    # 保持某个类名不被混淆（类内部内容会被混淆）
    -keep cn.listenergao.myapp.example.Test
    
    # 保持某个类的 类名 以及内部的所有内容不被混淆
    -keep cn.listenergao.myapp.example.Test{*;}
    
    # 保持类中特定内容，而不是所有内容可以使用如下
    -keep cn.listenergao.myapp.example.Test{    
        <init>; #匹配所有构造器
        <fields>;#匹配所有域
        <methods>;#匹配所有方法
    }
    
    # 可以在<fields>或<methods>前面加上private 、public、native等来进一步指定不被混淆的内容
    -keep class cn.listenergao.myapp.example.Test{
        public <methods>;#保持该类下所有的共有方法不被混淆
        public *;#保持该类下所有的共有内容不被混淆
        private <methods>;#保持该类下所有的私有方法不被混淆
        private *;#保持该类下所有的私有内容不被混淆
        public <init>(java.lang.String);#保持该类的String类型的构造方法
    }
    
    # 在方法后加入参数，限制特定的方法(经测试：仅限于构造方法可以混淆)
    -keep class cn.listenergao.myapp.example.Test{
        public <init>(String);
    }
    
    # 保留一个类中的内部类不被混淆需要用 $ 符号
    # #保持 Test 类中的 MyClass 类不被混淆
    -keep class cn.listenergao.myapp.example.Test$MyClass{*;}
    
    # jni方法不可混淆，因为native方法是要完整的包名类名方法名来定义的，不能修改，否则找不到；
    # 不混淆native方法
    -keepclasseswithmembernames class * {
        native <methods>;
    }
    
    # 不混淆枚举类
    -keepclassmembers enum * {
      public static **[] values();
      public static ** valueOf(java.lang.String);
    }
    
    #不混淆资源类
    -keepclassmembers class **.R$* {
        public static <fields>;
    }
    
    # 不混淆自定义控件
    -keep public class * entends android.view.View {
        *** get*();
        void set*(***);
        public <init>;
    }
    
    # 不混淆实现了Serializable接口的类成员，此处只是演示，也可以直接 *;
    -keepclassmembers class * implements java.io.Serializable {
        static final long serialVersionUID;
        private static final java.io.ObjectStreamField[] serialPersistentFields;
        private void writeObject(java.io.ObjectOutputStream);
        private void readObject(java.io.ObjectInputStream);
        java.lang.Object writeReplace();
        java.lang.Object readResolve();
    }
    
    # 不混淆实现了parcelable接口的类成员
    -keep class * implements android.os.Parcelable {
        public static final android.os.Parcelable$Creator *;
    }
    
    # 注意事项：
    # 
    # ① jni方法不可混淆，方法名需与native方法保持一致；
    # ② 反射用到的类不混淆，否则反射可能出问题；
    # ③ 四大组件、Application子类、Framework层下的类、自定义的View默认不会被混淆，无需另外配置；
    # ④ WebView的JS调用接口方法不可混淆；
    # ⑤ 注解相关的类不混淆；
    # ⑥ GSON、Fastjson等解析的Bean数据类不可混淆；
    # ⑦ 枚举enum类中的values和valuesof这两个方法不可混淆(反射调用)；
    # ⑧ 继承Parceable和Serializable等可序列化的类不可混淆；
    # ⑨ 第三方库或SDK，请参考第三方提供的混淆规则，没提供的话，建议第三方包全部不混淆；
    ```
    
3. **混淆规则的叠加**

    不知道你有没有想过：上面日常使用的创建的代码示例，proguard-rules.pro没有配置混淆规则，却混淆了？

    其实是因为混淆规则是叠加的，而混淆规则的来源不止主模块里的proguard-rules.pro，还有这些：
    
    ① /proguard-rules.pro
    
    不止主模块有proguard-rules.pro，子模块也可以有，因为规则是叠加的，故某个模块的配置都可能影响其它模块。
    
    ② proguard-android-optimize.txt
    
    AGP编译时生成，其中包含了对大多数Android项目都有用的规则，并且启用 @Keep* 注解。
    
    AGP提供的规则文件还有proguard-defaults.txt或proguard-android.txt，可通过getDefaultProguardFile进行设置，不过建议还是使用这个文件(多了些优化配置)。
    
    /build/intermediates/proguard-rules/debug/aapt_rules.txt
    
    自动生成，AAPT2会根据对应用清单中的类、布局及其他应用资源的引用，生成保留规则，如不混淆每个Activity。
    
    ④ AAR库 →/proguard.txt
    ⑤ Jar库 →/META-INF/proguard/
    
    如果想查看所有规则叠加后的混淆规则，可在主目录的proguard-rules.pro添加下述配置：
    
    ```
    # 输出所有规则叠加后的混淆规则
    -printconfiguration ./build/outputs/mapping/full-config.txt
    ```
    
4. **资源压缩**

    资源压缩其实分为两步：资源合并与资源移除，前者无论是否配置shrinkResources true，AGP构建APK时都会执行，当存在两个或更多名称相同的资源才会进行资源合并，AGP会从重复项中选择优先级更高的文件，并只将此资源传递给AAPT2，以供在APK中分发。
    
    级联优先顺序：
    
        依赖项 → 主资源 → 渠道 → 构建类型
    
    比如：重复资源存在于主资源及渠道中，Gradle会选择渠道中的资源；但如果重复资源在同一层次出现，如src/main/res/和src/main/res2中有重复资源，Gradle就会报资源合并错误。
    
    对应打包Task中的processDebugResources，将aapt编译后的flat产物和合并后的清单文件进行链接处理生成._ap文件(包含资源文件、清单文件、资源关系映射表文件resources.arsc) 及R.java文件(保存了资源类型、资源名称及地址的映射关系)。
    
    说完资源合并，接着说下资源移除，开启资源压缩后，所有未被使用的资源默认会被移除，如果你想定义那些资源需要保留，可以在res/raw/路径下创建一个xml文件，如keep.xml，配置示例如下 (此文件不会打包到APK中，支持通配符*，此类文件可有多份)：
    
    
    ```
    <?xml version="1.0" encoding="utf-8"?>
    <resources xmlns:tools="http://schemas.android.com/tools"
        <!-- 定义哪些资源要被保留 -->
        tools:keep="@layout/l_used*_c,@layout/l_used_a,@layout/l_used_b*"
        <!-- 定义哪些资源需要被移除 -->
        tools:discard="@layout/unused2"
        <!-- 开启严苟模式，可选值strict,safe，前者严格按照keep和discard指定的资源保留 -->
        <!-- 后者保守删除未引用资源，如代码中使用Resources.getIdentifier()引用的资源会保留 -->
        tools:shrinkMode="strict"/>
    ```
    
    另外，还可以在build.gradle中添加resConfigs来移除不需要的备用资源文件，如只保留中文：
    
    
    ```
    android {
        ...
        defaultConfig {
            resConfigs "zh-rCN" // 不用支持国际化只需打包中文资源
        }
    } 
    ```
    
## ProGuard 与 R8：

**简述：**

* ProGuard→ 压缩、优化和混淆Java字节码文件的免费工具，开源仓库地址：proguard
https://github.com/Guardsquare/proguard

* R8→ ProGuard的替代工具，支持现有ProGuard规则，更快更强，AGP 3.4.0或更高版本，默认使用R8混淆编译器。


如果不想用R8，想用回ProGuard的话(可以但没必要)，可以在gradle.properties文件中添加下述配置禁用R8：


```
android.enableR8=false
android.enableR8.libraries=false
```

编译APK时可能会报错：

![](https://mweb-image-1258736741.cos.ap-beijing.myqcloud.com/2021/06/08/16231423606522.jpg)

解决方法：

在proguard-rules.pro文件中加上-ignorewarnings即可解决。


建议直接使用 R8，毕竟兼容绝大部分的 ProGuard 规则，更快的编译速度，对 Kotlin 更友好。

**其它：**

另外，使用ProGuard或R8构建项目会在build\outputs\mapping\release输出下述文件：

* mapping.txt→ 原始与混淆过的类、方法、字段名称间的转换；

* seeds.txt→ 未进行混淆的类与成员；

* usage.txt→ APK中移除的代码；

* resources.txt→ 资源优化记录文件，哪些资源引用了其他资源，哪些资源在使用，哪些资源被移除；

**Tips：** 上述文件不一定都有，R8可以在proguard-rules.pro文件添加下述配置输出对应文件：

```
# 输出mapping.txt文件
-printmapping ./build/outputs/mapping/mapping.txt

# 输出seeds.txt文件
-printseeds ./build/outputs/mapping/seeds.txt

# 输出usage.txt文件
-printusage ./build/outputs/mapping/usage.txt
```



    