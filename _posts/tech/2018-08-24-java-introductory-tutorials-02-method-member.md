---
layout: post
title: Java基础02 方法与数据成员 
category: 技术
tags: Java
keywords: Java
description: Java 快速入门教程 方法与数据成员
---
 

在 [Java基础01][1] 从HelloWorld到面向对象，我们初步了解了对象(object)。对象中的数据成员表示对象的状态。对象可以执行方法，表示特定的动作。

此外，我们还了解了类(class)。同一类的对象属于相同的类型(type)。我们可以定义类，并使用该定义来产生对象。

我们进一步深入到对象。了解Java中方法与数据成员的一些细节。


### 调用同一对象的数据成员
方法可以调用该对象的数据成员。比如下面我们给Human类增加一个getHeight()的方法。该方法返回height数据成员的值:

```java

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human();
        System.out.println(aPerson.getHeight());
    }

}

class Human
{/**
     * accessor
     */
    int getHeight()
    {
        return this.height;
    }

    int height;
}
```

我们新增了getHeight方法。这个方法有一个int类型的返回值。Java中使用return来返回值。

 

注意this，它用来指代对象自身。当我们创建一个aPerson实例时，this就代表了aPerson这个对象。this.height指aPerson的height。

this是隐性参数(implicit argument)。方法调用的时候，尽管方法的参数列表并没有this，Java都会“默默”的将this参数传递给方法。

(有一些特殊的方法不会隐性传递this，我们会在以后见到)

 

this并不是必需的，上述方法可以写成:

```java

    /**
     * accessor
     */
    int getHeight()
    {
        return height;
    }
```

Java会自己去判断height是类中的数据成员。但使用this会更加清晰。

我们还看到了 /** comments  */ 的添加注释的方法。


### 方法的参数列表
Java中的方法定义与C语言中的函数类似。Java的方法也可以接收 **参数列表(argument list)** ，放在方法名后面的括号中。下面我们定义一个growHeight()的方法，该方法的功能是让人的height增高:

```java

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human();
        System.out.println(aPerson.getHeight());
        aPerson.growHeight(10);
        System.out.println(aPerson.getHeight());
    }

}

class Human
{
/**
     * accessor
     */
    int getHeight()
    {
        return this.height;
    }

    /**
     * pass argument
     */
    void growHeight(int h)
    {
        this.height = this.height + h;
    }

    int height;
}
```

在growHeight()中，h为传递的参数。在类定义中，说明了参数的类型(int)。在具体的方法内部，我们可以使用该参数。该参数只在该方法范围，即growHeight()内有效。

在调用的时候，我们将10传递给growHeight()。aPerson的高度增加了10。

 
### 调用同一对象的其他方法
在方法内部，可以调用同一对象的其他方法。在调用的时候，使用this.method()的形式。我们还记得，this指代的是该对象。所以this.method()指代了该对象自身的method()方法。

比如下面的repeatBreath()函数:

```java

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human();
        aPerson.repeatBreath(10);
    }

}

class Human
{
    void breath()
    {
        System.out.println("hu...hu...");
    }


   /**
    * call breath()
    */
    void repeatBreath(int rep)
    {
        int i;
        for(i = 0; i < rep; i++) {
            this.breath();
        }
    }
    int height;
}
```

为了便于循环，在repeatBreath()方法中，我们声明了一个int类型的对象i。i的作用域限定在repeatBreath()方法范围内部。

(这与C语言函数中的自动变量类似)


### 数据成员初始化
在Java中，数据成员有多种 **初始化(initialize)** 的方式。比如上面的getHeight()的例子中，尽管我们从来没有提供height的值，但Java为我们挑选了一个 **默认** 初始值0。

基本类型的数据成员的默认初始值:

数值型: 0
布尔值: false
其他类型: null
 

我们可以在声明数据成员同时，提供数据成员的初始值。这叫做显式初始化(explicit initialization)。显示初始化的数值要硬性的写在程序中：

```java

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human();
        System.out.println(aPerson.getHeight());
    }

}

class Human
{/**
     * accessor
     */
    int getHeight()
    {
        return this.height;
    }

    int height = 175;
}
```

这里，数据成员height的初始值为175，而不是默认的0了。

Java中还有其它初始化对象的方式，我将在以后介绍。


### 总结
return

this, this.field, this.method()

默认初始值，显式初始化


[1]:https://xzchsia.github.io/2018/08/24/java-introductory-tutorials-01.html

