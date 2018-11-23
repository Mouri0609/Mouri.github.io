---
title: 测试工具Junit
date: 2018-11-04 20:47:08
categories:
- 工具
tags:
- java
---

# Junit使用
<!--more-->
-------------------

## Junit 简介
### 简介
- JUnit是一个Java语言的单元测试框架。它由Kent Beck和Erich Gamma建立，逐渐成为源于Kent Beck的sUnit的xUnit家族中最为成功的一个JUnit有它自己的JUnit扩展生态圈。多数Java的开发环境都已经集成了JUnit作为单元测试的工具。

### 优点
- JUnit是用于编写和运行测试的开源框架。
- 提供了注释，以确定测试方法。
- 提供断言测试预期结果。
- 提供了测试运行的运行测试。
- JUnit测试让您可以更快地编写代码，提高质量 JUnit是优雅简洁。
- 它是不那么复杂以及不需要花费太多的时间。
- JUnit测试可以自动运行，检查自己的结果，并提供即时反馈。没有必要通过测试结果报告来手动梳理。
- JUnit测试可以组织成测试套件包含测试案例，甚至其他测试套件。
- Junit显示测试进度的，如果测试是没有问题条形是绿色的，测试失败则会变成红色。

## Junit 安装使用
1. 笔者使用的是**Intellij IDEA**编辑器，该软件在安装时已经默认配置了Junit单元测试插件，读者可以尝试在**"File->Settings->Plugins"**下搜索JUnit，如果没有请自行安装。安装好应用后重启编辑器即可。
2. 在工程目录下创建test目录用于存放测试类，在此目录上右击鼠标并将此目录标记为Test Resources Root。
3. 选择需要测试的类文件，右键点击选择**go to ->test**,然后点击create new test ![](Junit_1.jpg)
4. 选择必要的选项，然后选择需要测试的方法，添加完成后即可生成测试方法。![](Junit_2.png)
5. 在测试方法中添加具体的实现内容后，即可执行相应的测试内容。![](Junit_3.png)

### 常用的注解
1. @Test: 测试方法
　　　　a)(expected=XXException.class)如果程序的异常和XXException.class一样，则测试通过
　　　　b)(timeout=100)如果程序的执行能在100毫秒之内完成，则测试通过
2. @Ignore: 被忽略的测试方法：加上之后，暂时不运行此段代码
3. @Before: 每一个测试方法之前运行
4. @After: 每一个测试方法之后运行
5. @BeforeClass: 方法必须必须要是静态方法（static 声明），所有测试开始之前运行，注意区分before，是所有测试方法
6. @AfterClass: 方法必须要是静态方法（static 声明），所有测试结束之后运行，注意区分 @After
