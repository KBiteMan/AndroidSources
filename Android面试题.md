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
#### 进程空间划分
- 一个进程空间分为**用户空间**&**内核空间**，即把进程内 **用户**&**内核 **隔离开来
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

