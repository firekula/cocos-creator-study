# 替换资源等一定要记得备份
# 替换资源等一定要记得备份
# 替换资源等一定要记得备份
# cocos creator

* 资源文件(图片,预制体等)名称不要有特殊符号和中文,可能会有问题  
* ccc需要动态加载的在resources图片的图集，不要使用自动图集，否则会每个图片都生成一个json文件(没卵用)  
* **以下或许只在1.9.5版本**
    * ccc的editbox默认值为空的情况下，在第一次打开的时候设置内容会导致实际显示文字很小，需要手动重新设置fontSize  
    * ccc的editbox就算是节点的active设置成false也会有效，且对于不同机型 enabled=false 可能会无效  
    * ccc的editbox的在IOS上开启Stay On Top可能会导致点击无反应  
* ccc搭建安卓原生环境的时候要注意：ccc安装目录和sdk、ndk、ant的路径都不能有中文和空格  
* ccc编译安卓原生的时候报错可能是因为项目路径太深  
    ``` 
    fatal error: opening dependency file  
    项目所在路径/hello/test/build/jsb-link/frameworks/runtime-src/proj.android-studio/app/build/intermediates/ndkBuild/release/obj/local/armeabi-v7a/objs/cocos2dx_static/scripting/js-bindings/jswrapper/v8/debugger/inspector_socket_server.o.d: No such file or directory  ··
    ```
* ccc调用java代码，java方法需要写静态，在调用jsb.reflection.callStaticMethod(类完整路径,方法名,方法签名,参数...);的时候  
* 方法签名必须和被调用方法保持一致，比如 ()V 表示没有参数也没有返回值，(I)V有int类型参数没有返回值 (I)I有int参数也有int返回值;没有对应会找不到方法  
* 特别注意String的方法签名 Ljava/lang/String; 后面的分号一定要加上去
* cccjava调用js代码，需要js代码是全局可用的:  
window.login();//js  
window['login']=()=>{}//ts
* ts和js混用的时候要注意依赖JS脚本的时候路径是否正确，不正确可能会导致发布后的包报错
* 没改代码突然出现莫名其妙的报错，先在报错的地方打个log再找问题
* 两个ts文件互相引用编辑器会报错

# 原生

* 通过使用PowerManager里面的WakeLock可以使游戏不息屏
    > [Android-WakeLock(唤醒锁与CPU休眠/屏幕常亮)](https://blog.csdn.net/qq_32115439/article/details/80169222)
* 接入一些需要设置某些值的SDK，要判断SDK和游戏都初始化完毕了再做后面的事情
* Google play servers 接入的时候要注意创建OAth2.0客户端，一个调试客户端，一个正式客户端
* 安卓Facebook分享需要base64图片或bitmap，可以在JS里处理图片(截图、拼图等)然后调用jsb.saveImageData()保存在本地，安卓根据路径读取图片直接以bitmap传入即可(会比较块)
* Cocos Creator 生成的配置文件里，主Acticity的任务关联是空字符串，会导致其他的Actictiy独立于App显示在最近任务中
* **android:taskAffinity:**  
    * 与 Activity 有着相似性的任务。从概念上讲，具有同一相似性的 Activity 归属同一任务（从用户的角度来看，则是归属同一“应用”）。任务的相似性由其根 Activity 的相似性确定。  
    * 相似性确定两点内容 — Activity 更改父项后的任务（请参阅 allowTaskReparenting 属性），以及通过 FLAG_ACTIVITY_NEW_TASK 标记启动 Activity 时，用于容纳该 Activity 的任务。
    * 默认情况下，应用中的所有 Activity 都具有同一相似性。您可以设置该属性，以不同方式将其分组，甚至可以在同一任务内放置不同应用中定义的 Activity。如要指定 Activity 与任何任务均无相似性，请将其设置为空字符串。(若主Activity设置为空字符串则所有任务都没有相似性)
* 如果未设置该属性，则 Activity 会继承为应用设置的相似性（请参阅 <application> 元素的 taskAffinity 属性）。应用默认相似性的名称为 <manifest> 元素所设置的软件包名称。
* 安卓查询最近系统发的广播：`adb shell dumpsys |grep BroadcastRecord`
* 谷歌的广告归因需要将firebase接入到项目中
* 谷歌支付如果闪一下返回错误6，有可能是谷歌商店没有允许后台运行和后台开启
* 或许会用到的东西：
  * 使用外部存储：
    > [关于获得安卓外部存储读写权限](https://www.cnblogs.com/zanzg/p/9129375.html)  
        [Android 文件外/内部存储的获取各种存储目录路径](https://blog.csdn.net/csdn_aiyang/article/details/80665185)  
        [如何在 Android 平台上使用 JavaScript 直接调用 Java 方法](https://docs.cocos.com/creator/manual/zh/advanced-topics/java-reflection.html?h=java)
* 接入Facebook Banner广告可以新建一个Activity然后把AdView添加到该Activity上
# QQ小游戏
* 远程加载资源有不支持的文件格式，下载不支持的格式会报错 Download Fielded
* ccc在QQ小游戏里获取环境是安卓，要加判断
* ccc发布QQ小游戏流程：发布微信小游戏，用VS Code打开微信小游戏文件夹，全局搜索'wx.'替换成'qq.'  saveFile前需改成fs(和微信不一样)
* qq小程序开发者工具需保持最新
# 工作注意
* 遇事不决先保存
* 一个需求完成后，先提交一次再继续做别的需求
* 废弃代码及时清理
* 空闲的时候看以前写的代码是否可以优化