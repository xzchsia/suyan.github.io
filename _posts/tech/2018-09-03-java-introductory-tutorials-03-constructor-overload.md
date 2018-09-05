---
layout: post
title: Java基础03 构造器与方法重载 
category: 技术
tags: Java
keywords: Java
description: Java基础03 构造器与方法重载
---


在[方法与数据成员中][j02]，我们提到，Java中的对象在创建的时候会初始化(initialization)。初始化时，对象的数据成员被赋予初始值。我们可以显式初始化。如果我们没有给数据成员赋予初始值，数据成员会根据其类型采用默认初始值。

显式初始化要求我们在写程序时就确定初始值，这有时很不方便。我们可以使用构造器(constructor)来初始化对象。构造器可以初始化数据成员，还可以规定特定的操作。这些操作会在创建对象时自动执行。

 

### 定义构造器  

构造器是一个方法。像普通方法一样，我们在类中定义构造器。构造器有如下基本特征:

- 构造器的名字和类的名字相同
- 构造器没有返回值
 

我们定义Human类的构造器:

```java  

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human(160);
        System.out.println(aPerson.getHeight());
    }

}

class Human
{
    /**
     * constructor
     */
    Human(int h)
    {
        this.height = h;
        System.out.println("I'm born");
    }

    /**
     * accessor
     */
    int getHeight()
    {
        return this.height;
    }

    int height;
}

```

上面的程序会打印

`I'm born`
`160`


构造器可以像普通方法一样接收参数列表。这里，构造器Human()接收一个整数作为参数。在方法的主体中，我们将该整数参数赋予给数据成员height。构造器在对象创建时做了两件事:

- 为数据成员提供初始值 `this.height = h;`
- 执行特定的初始操作 `System.out.println("I'm born");`

这样，我们就可以在调用构造器时，灵活的设定初始值，不用像显示初始化那样拘束。


#### 构造器是如何被调用的呢？

我们在创建类的时候，采用的都是new Human()的方式。实际上，我们就是在调用Human类的构造器。当我们没有定义该方法时，Java会提供一个空白的构造器，以便使用new的时候调用。但当我们定义了构造器时，在创建对象时，Java会调用定义了的构造器。在调用时，我们提供了一个参数160。从最后的运行结果中也可以看到，对象的height确实被初始化为160。

 

### 初始化方法的优先级

在方法与数据成员中，我们可以看到，如果我们提供显式初始值，那么数据成员就会采用显式初始值，而不是默认初始值。但如果我们既提供显式初始值，又在构造器初始化同一数据成员，最终的初始值将由构造器决定。比如下面的例子:

```java  

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human(160);
        System.out.println(aPerson.getHeight());
    }

}

class Human
{
    /**
     * constructor
     */
    Human(int h)
    {
        this.height = h; 
    }

    /**
     * accessor
     */
    int getHeight()
    {
        return this.height;
    }

    int height=170; // explicit initialization
}
```

运行结果为:

`160`

对象最终的初始化值与构建方法中的值一致。因此:

> 构建方法 > 显式初始值 > 默认初始值

(事实上，所谓的优先级与初始化时的执行顺序有关，我将在以后深入这一点)


### 方法重载

一个类中可以定义不止一个构造器，比如:

```java  
public class Test
{
    public static void main(String[] args)
    {
        Human neZha   = new Human(150, "shit");
        System.out.println(neZha.getHeight()); 
    }

}

class Human
{
    /**
     * constructor 1
     */
    Human(int h)
    {
        this.height = h;
        System.out.println("I'm born");
    }

    /**
     * constructor 2
     */
    Human(int h, String s)
    {
        this.height = h;
        System.out.println("Ne Zha: I'm born, " + s);
    }

    /**
     * accessor
     */
    int getHeight()
    {
        return this.height;
    }

    int height;
}
```
运行结果:

`Ne Zha: I'm born, shit`
`150`

上面定义了两个构造器，名字都是Human。两个构造器有不同的参数列表。

在使用new创建对象时，Java会根据提供的参数来决定构建哪一个构造器。比如在构建neZha时，我们提供了两个参数: 整数150和字符串"shit"，这对应第二个构建方法的参数列表，所以Java会调用第二个构建方法。

在Java中，Java会同时根据方法名和参数列表来决定所要调用的方法，这叫做方法重载(method overloading)。构建方法可以进行重载，普通方法也可以重载，比如下面的breath()方法:

```java

public class Test
{
    public static void main(String[] args)
    {
        Human aPerson = new Human();
        aPerson.breath(10);
    }

}

class Human
{
    /**
       * breath() 1
       */
    void breath()
    {
        System.out.println("hu...hu...");
    }


   /**
    * breath() 2
    */
    void breath(int rep)
    {
        int i;
        for(i = 0; i < rep; i++) {
            System.out.println("lu...lu...");
        }
    }

    int height;
}
```

运行结果:

```java
lu...lu...
lu...lu...
lu...lu...
lu...lu...
lu...lu...
lu...lu...
lu...lu...
lu...lu...
lu...lu...
lu...lu...
```

可以看到，由于在调用的时候提供了一个参数: 整数10，所以调用的是参数列表与之相符的第二个breath()方法。
 

### 总结

- constructor特征: 与类同名，无返回值

- constructor目的: 初始化，初始操作

- 方法重载: 方法名 + 参数列表 -> 实际调用哪一个方法



[j02]:https://xzchsia.github.io/2018/08/24/java-introductory-tutorials-02-method-member.html

