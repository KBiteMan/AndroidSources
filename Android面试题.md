# Android面试题


## Binder机制
### 什么是Binder
![Binder机制.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/Binder%E6%98%AF%E4%BB%80%E4%B9%88.png?raw=true)
- 从机制、模型角度讲
  - Binder是一种Android中实现跨进程通信（IPC）的方式，即Binder机制模型
  - 在Android中实现跨进程通信
- 从模型结构、组成讲
  - Binder是一种虚拟的物理设备驱动，即Binder驱动
  - 连接Service进程、Client进程和ServiceManager进程
- 从Android代码的实现角度讲
  - Binder是个类，实现了IBinder接口，即Binder类
  - 将Binder机制模型以代码的形式具体是现在Android中

### 相关基础知识
#### 进程空间划分
- 一个进程空间分为**用户空间**&**内核空间**，即把进程内 **用户**&**内核**隔离开来
- 二者区别
  1. 进程间，用户空间的数据==不可共享==，用户空间=不可共享空间
  2. 进程间，内核空间的数据==可共享==，内核空间=可共享空间
> ==所有进程共用一个内核空间==
- 进程内**用户空间**&**内核空间**进行交互，需要通过系统调用，主要通过函数：
> 1.  copy_from_user（）：将用户空间的数据拷贝到内核空间
> 2.  copy_to_user（）：将内核空间的数据拷贝到用户空间

![Linux跨进程通信.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/Linux%E8%B7%A8%E8%BF%9B%E7%A8%8B%E9%80%9A%E4%BF%A1.png?raw=true)

#### 进程隔离 & 跨进程通信（IPC）
- 进程隔离
  - 为了保证安全性&独立性，一个进程不能直接操作或者访问另一个进程，即Android中的进程是相互独立、隔离的
- 跨进程通信（IPC）
  - 进程间需要进行数据交换、通信
- 跨进程通信基本原理
![跨进程原理.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/%E8%B7%A8%E8%BF%9B%E7%A8%8B%E5%8E%9F%E7%90%86.png?raw=true)

> 1.而Binder的作用是：连接两个进程，实现mmap()系统调用，主要负责创建数据接收的缓存空间和管理数据接收空间
> 2.传统的跨进程通信需要拷贝数据2次，但Binder机制只需1次，主要使用到了内存映射

#### 内存映射

### Binder跨进程通信机制——模型
该模型基于C\S模式（Client-Service）
![C\S模型原理.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/C%5CS%E6%A8%A1%E5%9E%8B%E5%8E%9F%E7%90%86.png?raw=true)

| 角色      | 作用    |  备注 |
| :-------: | :-------- | ----- |
| Client进程| 使用服务的进程 | APP |
| Service进程| 提供服务的基础|   服务器端 |
| ServiceManager进程| 管理Service注册与查询（将字符形式的Binder名字，转化成Client中对该Binder的引用） |   类似路由器|
| Binder驱动 |一种虚拟设备驱动，是连接Service进程、Client进行和ServiceManager的桥梁，具体作用为：</br>1.传递跨进程的数据：通过内存映射</br>2.实现线程控制：采用Binder线程池，并由Binder驱动自身进行管理|Binder驱动持有每个Server进程在内核空间中的Binder实体，并给Client进程提供Binder实体的引用|

### Binder驱动的作用&原理
作用：连接Service进程、Client进程和ServiceManager桥梁
原理：内存映射，即内部调用mmap（）函数
实际用途：1.创建数据接收的缓存空间；2.地址映射

![原理.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/944365-65a5b17426aed424.png?raw=true)

![原理.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/944365-d3c78b193c3e8a38.png?raw=true)


## ActivityThread工作原理
main方法：
```java
public static void main(String[] args) {
    Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "ActivityThreadMain");    
    // CloseGuard defaults to true and can be quite spammy.  We
    // disable it here, but selectively enable it later (via 
    // StrictMode) on debug builds, but using DropBox, not logs.
    CloseGuard.setEnabled(false);    
    Environment.initForCurrentUser();    
    // Set the reporter for event logging in libcore
    EventLogger.setReporter(new EventLoggingReporter());    
    // Make sure TrustedCertificateStore looks in the right place for CA certificates
    final File configDir = Environment.getUserConfigDirectory(UserHandle.myUserId());
    TrustedCertificateStore.setDefaultUserDirectory(configDir);    
    Process.setArgV0("<pre-initialized>");    
    Looper.prepareMainLooper();    
    // Find the value for {@link #PROC_START_SEQ_IDENT} if provided on the command line.
    // It will be in the format "seq=114"  long startSeq = 0;
    if (args != null) {
        for (int i = args.length - 1; i >= 0; --i) {
            if (args[i] != null && args[i].startsWith(PROC_START_SEQ_IDENT)) {
                startSeq = Long.parseLong(
                        args[i].substring(PROC_START_SEQ_IDENT.length()));
            }
        }
    }
    ActivityThread thread = new ActivityThread();
    thread.attach(false, startSeq);   if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler();
    }

    if (false) {
        Looper.myLooper().setMessageLogging(new
        LogPrinter(Log.DEBUG, "ActivityThread"));
    }

    // End of event ActivityThreadMain.
    Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
    Looper.loop();   
    throw new RuntimeException("Main thread loop unexpectedly exited"); 
}
```


## Activity的启动流程

## Android类加载器

## View的绘制流程

### MeasureSpec为何高两位代表测量模式，而不拆成两个参数进行传递？

## View和ViewGroup事件分发

## SurfaceView和View的区别

## Android动画

## requestLayout，invalidate，postInvalidate区别与联系

## 多线程

## 线程池

## 进程管理机制

## 进程保活
进程的优先级是什么？
分**黑、白、灰**三种手段保活
- 黑：不同App进程，用广播相互唤醒
- 白：启动前台Service
- 灰：利用系统的漏洞启动前台Service

### 黑色保活
- 场景1：开机，网络切换、拍照、拍视频时候，利用系统产生的广播唤醒app
- 场景2：接入第三方SDK也会唤醒相应的app进程，如微信sdk会唤醒微信，支付宝sdk会唤醒支付宝。由此发散开去，就会直接触发了下面的 场景3
- 场景3：假如你手机里装了支付宝、淘宝、天猫、UC等阿里系的app，那么你打开任意一个阿里系的app后，有可能就顺便把其他阿里系的app给唤醒了。（只是拿阿里打个比方，其实BAT系都差不多）
### 白色保活
白色保活手段非常简单，就是调用系统api启动一个前台的Service进程，这样会在系统的通知栏生成一个Notification，用来让用户知道有这样一个app在运行着，哪怕当前的app退到了后台。如下方的LBE和QQ音乐这样。
### 灰色保活
灰色保活，这种保活手段是应用范围最广泛。它是利用系统的漏洞来启动一个前台的Service进程，与普通的启动方式区别在于，它不会在系统通知栏处出现一个Notification，看起来就如同运行着一个后台Service进程一样。这样做带来的好处就是，用户无法察觉到你运行着一个前台进程（因为看不到Notification）,但你的进程优先级又是高于普通后台进程的。那么如何利用系统的漏洞呢，大致的实现思路和代码如下：
- 思路一：API < 18，启动前台Service时直接传入new Notification()；
- 思路二：API >= 18，同时启动两个id相同的前台Service，然后再将后启动的Service做stop处理

系统出于体验和性能上的考虑，app在退到后台时系统并不会真正的kill掉这个进程，而是将其缓存起来。打开的应用越多，后台缓存的进程也越多。在系统内存不足的情况下，系统开始依据自身的一套进程回收机制来判断要kill掉哪些进程，以腾出内存来供给需要的app。这套杀进程回收内存的机制就叫 Low Memory Killer ，它是基于Linux内核的 OOM Killer（Out-Of-Memory killer）机制诞生。

进程的重要性，划分5级：
- 前台进程 (Foreground process)
- 可见进程 (Visible process)
- 服务进程 (Service process)
- 后台进程 (Background process)
- 空进程 (Empty process)

了解完 Low Memory Killer，再科普一下oom_adj。什么是oom_adj？它是linux内核分配给每个系统进程的一个值，代表进程的优先级，进程回收机制就是根据这个优先级来决定是否进行回收。对于oom_adj的作用，你只需要记住以下几点即可：

进程的oom_adj越大，表示此进程优先级越低，越容易被杀回收；越小，表示进程优先级越高，越不容易被杀回收

普通app进程的oom_adj>=0,系统进程的oom_adj才可能<0

有些手机厂商把这些知名的app放入了自己的白名单中，保证了进程不死来提高用户体验（如微信、QQ、陌陌都在小米的白名单中）。如果从白名单中移除，他们终究还是和普通app一样躲避不了被杀的命运，为了尽量避免被杀，还是老老实实去做好优化工作吧。

所以，进程保活的根本方案终究还是回到了性能优化上，进程永生不死终究是个彻头彻尾的伪命题！

## ThreadLocal

## Synchronized原理

## Volatile实现原理

## Handler消息机制

## AsyncTask源码分析

## HashMap原理，Hash碰撞，自动扩容

## 常用设计模式以及使用场景

## 常用排序算法

## 常用加解密算法

## 图片的二次采样

## Serializable 和Parcelable的区别

## jvm垃圾回收机制

## ANR信息收集

## RecyclerView和ListView详解，局部刷线

## Android两种虚拟机的联系与区别

## APK打包流程

## APK安装流程

## APK瘦身

## 引用类型
- 强
- 软
- 弱

## Fragment懒加载，ViewPager缓存机制

## Java中反射、注解的理解

## LruCache
原理
双向链表

## 小程序实现原理

