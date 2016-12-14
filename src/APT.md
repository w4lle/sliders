# 编译时注解

author : 王令龙

## 目录

 - 什么是注解
 - 注解的分类
 - 自定义运行时注解
 - 自定义编译时注解

## 什么是注解

Annontation是Java5开始引入的新特征。中文名称一般叫注解。它提供了一种安全的类似注释的机制，用来将任何的信息或元数据（metadata）与程序元素（类、方法、成员变量等）进行关联。

## 注解的分类

### 元注解

1.@Target 描述注解的使用范围，取值：
 - ElementType.CONSTRUCTOR:描述构造器
 - ElementType.FIELD:描述成员变量
 - ElementType.VARIABLE: 描述局部变量
 - ElementType.METHOD: 描述方法
 - ElementType.PACKAGE: 描述包
 - ElementType.PARAMETER：描述方法的参数
 - ElementType.Type: 描述类，接口（包括注解类型）或enum声明.

2.@Retention 注解的声明周期，即在什么级别保留，取值：
 - RetentionPoicy.SOURCE :在源文件中有效（在.java文件中有效）
 - RetentionPoicy.CLASS: 在class文件中有效
 - RetentionPoicy.RUNTIME:在运行时有效

### 内建注解

 - @Override
 - @Deprecated
 - @SuppressWarnings

## 自定义运行时注解

## 自定义编译时注解
