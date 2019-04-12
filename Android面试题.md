# Android面试题


## Binder机制
### 什么是Binder
![Binderæ¯ä»ä¹.png](https://github.com/KBiteMan/AndroidSources/blob/master/img/Binder%E6%98%AF%E4%BB%80%E4%B9%88.png?raw=true)
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
Linux相关。
- 一个进程空间分为**用户空间**&**内核空间**，即把进程内 **用户**&**内核 **隔离开来
- 二者区别
  1. 进程间，用户空间的数据==不可共享==，用户空间=不可共享空间
  2. 进程间，内核空间的数据==可共享==，内核空间=可共享空间
> ==所有进程共用一个内核空间==
- 进程内**用户空间**&**内核空间**进行交互，需要通过系统调用，主要通过函数：
> 1.  copy_from_user（）：将用户空间的数据拷贝到内核空间
> 2.  copy_to_user（）：将内核空间的数据拷贝到用户空间


## ActivityThread工作原理

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

## ThreadLocal

## Synchronized原理

## Volatile实现原理

## Handler消息机制

## AsyncTask源码分析

## HashMap原理，Hash碰撞，自动扩容

## Retrofit源码分析

## Glide缓存源码，加载原理

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

## 小程序实现原理

