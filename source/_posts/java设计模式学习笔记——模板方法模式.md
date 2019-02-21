---
title: java设计模式学习笔记——模板方法模式
date: 2019-02-20 18:36:16
categories:
- java
tags:
- 设计模式
---

### 模板方法模式
 在一个方法中定义一个算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以在不改变算法的结构下，重新定义算法中的某些步骤。
 <!--more-->

### 代码demo
国外人部分人的生活习惯和国内大部分人不同，举例早上起床。电影中常看到国外人起床一般都是先洗澡，然后再洗漱吃早餐。   
程序代码如下：
1. 模板抽象类   

```java
package designPattern.TemplateMethod;

/**
 * Created by Mouri on 2019/2/20 16:59
 */
public abstract class WakeUpTemplate {
        public final void wakeup() {
            leaveBed();
            if(IsTakeBath()){
                takeBath();
            }
            brushTooth();
            washFace();
            eatBrakefast();

        }

        protected void leaveBed() {
            System.out.println("leave the bed first!");
        }

        protected void takeBath(){
            System.out.println("foreigner always take bathes first");
        };

        protected boolean IsTakeBath(){
            return false;
        }

        protected void brushTooth(){
            System.out.println("brush our tooth");
        };

        protected void washFace() {
            System.out.println("wash your face!");
        }

        protected abstract void eatBrakefast();

}
```

2. 国内人起床的实现

```java
package designPattern.TemplateMethod;

/**
 * Created by Mouri on 2019/2/20 17:20
 */
public class Chinese extends WakeUpTemplate {
    @Override
    protected void eatBrakefast() {
        System.out.println("Chinese likes soybean milk and congee");
    }
}

```

3. 国外人的起床实现   

```java
package designPattern.TemplateMethod;

/**
 * Created by Mouri on 2019/2/20 17:23
 */
public class American extends WakeUpTemplate {
    @Override
    protected void eatBrakefast() {
        System.out.println("American like milk and bread");
    }

    @Override
    protected boolean IsTakeBath(){
        return true;
    }
}

```
4. 测试   

```java
package designPattern.TemplateMethod;

/**
 * Created by Mouri on 2019/2/20 17:25
 */
public class test {
    public static void main(String[] args) {
        WakeUpTemplate wakeUpByChinese = new Chinese();
        WakeUpTemplate wakeUpByAmerican = new American();
        wakeUpByChinese.wakeup();
        System.out.println("===============================");
        wakeUpByAmerican.wakeup();
    }
}

```
输出结果
```
leave the bed first!
brush our tooth
wash your face!
Chinese likes soybean milk and congee
===============================
leave the bed first!
foreigner always take bathes first
brush our tooth
wash your face!
American like milk and bread
```
通过子类的继承实现了不同的父类的步骤流程的细节，提高代码的复用。如果某步骤的执行需要判断条件，则可以在父类的模板方法中引入判断语句，如果子类需要实现该方法。则重写父类方法，将判断的值进行修改。

### 优点
1. 子类在实现详细的处理流程时，不会打乱复杂的步骤的执行程序。
2. 通过恰当的继承实现代码的复用。
3. 可通过子类来覆盖父类的基本方法，更换和增加子类很方便，符合单一职责和开闭原则。

### 缺点
1. 如果父类中的可变方法较多，则系统会变得庞大。此时可以结合桥接的模式进行设计（暂时没有学习）

### 适用场景
1. 对一个算法或者流程中进行分析，将其中不变的部分设计成模板方法和父类具体方法，并将可变部分交给子类来实现。
2. 子类的公共行为应被集中到公共父类避免代码重复。
3. 需要通过子类来决定父类算法某个步骤的具体行为。
