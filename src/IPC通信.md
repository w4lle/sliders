# IPC通信

author: 王令龙

## 目录

 - IPC简介
 - 基础概念
 - Android中的IPC方式
 - Binder
 - 总结
 
## IPC简介

### 进程
线程：CPU可调度的最小单元
进程：指一个程序或者一个应用，可以包含多个线程

### IPC使用场景

 - 应用内部
 - 应用之间

### 开启多进程

 - android:process=":remote"，应用的私有进程
 - android:process="com.package.remote"，全局进程


 
