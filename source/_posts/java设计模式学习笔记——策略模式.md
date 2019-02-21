---
title: java设计模式学习笔记——策略模式
date: 2019-02-21 09:41:05
categories:
- java
tags:
- 设计模式
---

### 问题
在软件开发中，我们会碰到类似的情况，实现一个功能有多种途径，比如游乐园打折方案：老人，成人，儿童，亲子等不同打折模式。如果将这些打折方案的具体实现写进了同一个Ticket()类中，会出现以下三个问题：
1. Ticket类的calculate()方法非常庞大，它包含各种打折算法的实现代码，在代码中出现了较长的if…else…语句，不利于测试和维护。

2. 增加新的打折算法或者对原有打折算法进行修改时必须修改Ticket类的源代码，违反了“开闭原则”，系统的灵活性和可扩展性较差。

3. 算法的复用性差，如果在另一个系统（如商场销售管理系统）中需要重用某些打折算法，只能通过对源代码进行复制粘贴来重用，无法单独重用其中的某个或某些算法（重用较为麻烦）。

### 策略模式
 策略模式定义了算法族，分别封装起来，让他们之间可以相互替换，此模式让算法的变化独立于使用算法的客户。
 <!--more-->

### 代码demo
在github上star了一个设计模式的教程，不仅会讲解，最后还会给出题目练习。题目如下：

Sunny软件公司欲开发一款飞机模拟系统，该系统主要模拟不同种类飞机的飞行特征与起飞特征，需要模拟的飞机种类及其特征如表24-1所示：
表24-1 飞机种类及特征一览表

飞机种类	|起飞特征	|飞行特征   
--|:--:|--:
直升机(Helicopter)|	垂直起飞(VerticalTakeOff)|	亚音速飞行(SubSonicFly)
客机(AirPlane)| 长距离起飞(LongDistanceTakeOff)|	亚音速飞行(SubSonicFly)
歼击机(Fighter)|	长距离起飞(LongDistanceTakeOff)|	超音速飞行(SuperSonicFly)
鹞式战斗机(Harrier)|	垂直起飞(VerticalTakeOff)|	超音速飞行(SuperSonicFly)

为将来能够模拟更多种类的飞机，试采用策略模式设计该飞机模拟系统。



```java
public abstract class Plane {
    //维持飞机行为抽象类的引用
    FlySpeed flySpeed;    
    StartBehavior startBehavior;

    public Plane(){
    }

    public void setFly(FlySpeed fly) {
        this.flySpeed = fly;
    }

    public void performFly() {
        flySpeed.fly();
    }

    public void setStart(StartBehavior start) {
        this.startBehavior = start;
    }

    public void performStart() {
        startBehavior.start();
    }

}


public interface StartBehavior {
    public void start();
}


public interface FlySpeed {
    public void fly();
}


public class Helicopter extends Plane {
    public Helicopter() {
        flySpeed = new SubSonicFly();
        startBehavior = new VerticalTakeOff();
    }
}

public class Fighter extends Plane {
    public Fighter() {
        flySpeed = new SuperSonicFly();
        startBehavior = new LongDistanceTakeOff();
    }
}
```
起飞特征实现
```java
public class LongDistanceTakeOff implements StartBehavior {
    public void start(){
        System.out.println("LongDistanceTakeOff");
    }
}

public class VerticalTakeOff implements StartBehavior {
    public void start(){
        System.out.println("VerticalTakeOff");
    }
}
```

飞行特征实现
```java
public class SubSonicFly implements FlySpeed {
    public void fly(){
        System.out.println("SubSonicFly");
    }
}

public class SuperSonicFly implements FlySpeed {
    public void fly(){
        System.out.println("SuperSonicFly");
    }
}

```
测试
```java
public class test {
    public static void main(String[] args) {
        Plane helicopter = new Helicopter();
        helicopter.performFly();
        helicopter.performStart();

        helicopter.setStart(new LongDistanceTakeOff()); //修改起飞行为
        helicopter.performStart();
        System.out.println("=======================");

        Plane fighter = new Fighter();
        fighter.performFly();
        fighter.performStart();
    }
}
```
输出结果
```java
SubSonicFly
VerticalTakeOff
LongDistanceTakeOff
=======================
SuperSonicFly
LongDistanceTakeOff
```
可以看到能够动态的修改飞机的行为，如果需要扩展一种新的飞机，无需修改原来的代码。

### 优点

1.  策略模式提供了对“开闭原则”的完美支持，用户可以在不修改原有系统的基础上选择算法或行为，也可以灵活地增加新的算法或行为。

2. 策略模式提供了管理相关的算法族的办法。策略类的等级结构定义了一个算法或行为族，恰当使用继承可以把公共的代码移到抽象策略类中，从而避免重复的代码。

3. 策略模式提供了一种可以替换继承关系的办法。如果不使用策略模式，那么使用算法的环境类就可能会有一些子类，每一个子类提供一种不同的算法。但是，这样一来算法的使用就和算法本身混在一起，不符合“单一职责原则”，决定使用哪一种算法的逻辑和该算法本身混合在一起，从而不可能再独立演化；而且使用继承无法实现算法或行为在程序运行时的动态切换。

4. 使用策略模式可以避免多重条件选择语句。多重条件选择语句不易维护，它把采取哪一种算法或行为的逻辑与算法或行为本身的实现逻辑混合在一起，将它们全部硬编码(Hard Coding)在一个庞大的多重条件选择语句中，比直接继承环境类的办法还要原始和落后。

5. 策略模式提供了一种算法的复用机制，由于将算法单独提取出来封装在策略类中，因此不同的环境类可以方便地复用这些策略类。



### 缺点

1. 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时选择恰当的算法。换言之，策略模式只适用于客户端知道所有的算法或行为的情况。

2. 策略模式将造成系统产生很多具体策略类，任何细小的变化都将导致系统要增加一个新的具体策略类。

3. 无法同时在客户端使用多个策略类，也就是说，在使用策略模式时，客户端每次只能使用一个策略类，不支持使用一个策略类完成部分功能后再使用另一个策略类来完成剩余功能的情况。

### 适用场景

1. 一个系统需要动态地在几种算法中选择一种，那么可以将这些算法封装到一个个的具体算法类中，而这些具体算法类都是一个抽象算法类的子类。换言之，这些具体算法类均有统一的接口，根据“里氏代换原则”和面向对象的多态性，客户端可以选择使用任何一个具体算法类，并只需要维持一个数据类型是抽象算法类的对象。

2. 一个对象有很多的行为，如果不用恰当的模式，这些行为就只好使用多重条件选择语句来实现。此时，使用策略模式，把这些行为转移到相应的具体策略类里面，就可以避免使用难以维护的多重条件选择语句。

3. 不希望客户端知道复杂的、与算法相关的数据结构，在具体策略类中封装算法与相关的数据结构，可以提高算法的保密性与安全性。
