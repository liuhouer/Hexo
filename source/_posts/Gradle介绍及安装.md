---
title: Gradle介绍及安装
date: 2016-04-05 20:36:32
tags: [Gradle]
categories: [Gradle]
---

#  对Gradle进行一个简单的介绍，以及它的安装。

## Gradle介绍
- Gradle是一个基于JVM的构建工具，它提供了：
- 像Ant一样，通用灵活的构建工具
- 可以切换的，基于约定的构建框架
- 强大的多工程构建支持
- 基于Apache Ivy的强大的依赖管理
- 支持maven, Ivy仓库
- 支持传递性依赖管理，而不需要远程仓库或者是pom.xml和ivy.xml配置文件。
- 对Ant的任务做了很好的集成
- 基于Groovy，build脚本使用Groovy编写
- 有广泛的领域模型支持构建
## Gradle 概述
- 基于声明和基于约定的构建。
- 依赖型的编程语言。
- 可以结构化构建，易于维护和理解。
- 有高级的API允许你在构建执行的整个过程当中，对它的核心进行监视，或者是配置它的行为。
- 有良好的扩展性。有增量构建功能来克服性能瓶颈问题。
- 多项目构建的支持。
- 多种方式的依赖管理。
- 是第一个构建集成工具。集成了Ant, maven的功能。
- 易于移值。
- 脚本采用Groovy编写，易于维护。
- 通过Gradle Wrapper允许你在没有安装Gradle的机器上进行Gradle构建。
- 自由，开源。

<!--more-->

## Gradle 安装
1，安装JDK，并配置`JAVA\_HOME`环境变量。因为Gradle是用Groovy编写的，而Groovy基于JAVA。另外，Java版本要不小于1.5.
2，下载。地址是：`http://www.gradle.org/downloads`。在这里下载你要的版本。
3，解压。如果你下载的是gradle-xx-all.zip的完整包，它会有以下内容：
- 二进制文件
- 用户手册（包括PDF和HTML两种版本）
- DSL参考指南
- API手册（包括Javadoc和Groovydoc）
- 样例
- 源代码，仅供参考使用。
4，配置环境变量。配置`GRADLE\_HOME`到你的gradle根目录当中，然后把`%GRADLE\_HOME%/bin`（linux或mac的是`$GRADLE\_HOME/bin`）加到PATH的环境变量。
linux用户可以在/.bashrc文件中配置。

配置完成之后，运行`gradle -v`，检查一下是否安装无误。如果安装正确，它会打印出Gradle的版本信息，包括它的构建信息，Groovy, Ant, Ivy, 当前JVM和当前系统的版本信息。

另外，可以通过`GRADLE\_OPTS`或`JAVA\_OPTS`来配置Gradle运行时的JVM参数。不过，JAVA\_OPTS设置的参数也会影响到其他的JAVA应用程序。