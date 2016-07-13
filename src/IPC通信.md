# IPC通信

author: 王令龙

## 目录

 - IPC简介
 - Binder
 - Android中的IPC方式
 - Binder连接池
 - 总结

## IPC简介

介绍一些基本概念

### 进程

线程：CPU可调度的最小单元。
进程：指一个程序或者一个应用，可以包含多个线程。

### IPC使用场景

 - 应用内部
 - 应用之间

### 开启多进程

 - android:process=":remote"，应用的私有进程
 - android:process="com.package.remote"，全局进程

### 多进程的问题

 - 静态成员变量和单例完全失效
 - 线程同步机制完全失效
 - SharedPreferences的可靠性下降
 - Application会多次创建

每个进程拥有一个独立的虚拟机和单独的内存，所以，我们可以把应用内的多进程当做具有相同ShareUID的两个应用。

### 序列化

 - Serializabe
 - Parcelable

## Binder
 
 - Binder是Android中的一个类，它实现了IBinder接口
 - 从IPC角度来说，Binder是Android中的一种跨进程通信方式，Binder还可以理解为一种虚拟的物理设备，它的设备驱动是/dev/binder
 - 从Android Framework角度来说，Binder是ServiceManager连接各种Manager（ActivityManager、WindowManager，等等）和ManagerService的桥梁
 - 从Android应用层来说，Binder是客户端和服务端进行通信的媒介

## Android中的IPC方式

### Bundle

 - Activity、Service、Receiver都支持在Intent中传递Bundle数据，由于Bundle实现了Parcelable接口，所以它可以方便地在不同的进程间传输。
 - 使用Bundle传输的数据必须能够被序列化，比如基本类型、实现了Parcelable接口的对象、实现了Serializable接口的对象

### AIDL

 - 客户端发起请求，当前线程会被挂起
 - 服务端的Binder方法运行在Binder线程池中，所以不管是否耗时都应该同步实现
 - 客户端回调方法运行在客户端的Binder线程池并非UI线程

### Messenger

轻量级的IPC方案，基于AIDL

### ContentProvider

 - ContentProvider是Android中提供的专门用于不同应用间进行数据共享的方式，ContentProvider的底层实现是Binder。
 - ContentProvider主要以表格的形式来组织数据，并且可以包含多个表，ContentProvider还支持文件数据（比如MediaStore），比如图片、视频等。
 - ContentProvider对底层的数据存储方式没有任何要求，可以使用SQLite数据库，也可以使用普通的文件，甚至可以采用内存中的一个对象来进行数据的存储。

内部实现还是Binder，CRUD方法运行在Binder线程池中，由于SQLite对数据库的操作做了同步，所以不需要手动同步；如果底层数据集是一块内存的话比如List之类的就需要我们处理同步问题。

### Socket

套接字它分为流式套接字和用户数据报套接字两种，分别对应于网络的传输控制层中的TCP和UDP协议。

## Binder连接池

 - 当AIDL很多时，不可能每一个都对应一个Service
 - Binder连接池的作用：将每个业务模块的Binder请求统一转发到远程Service中去执行，从而避免重复创建Service的过程。

## 总结：选择合适的IPC方式

![此处输入图片的描述][1]


  [1]: http://img.blog.csdn.net/20160303161226017