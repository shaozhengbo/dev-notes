# [Spring实战（第4版）](../../../../)

## [第1部分 Spring的核心](../../../)

### [第1章 Spring之旅](../../)

#### [1.1 简化Java开发](../)

##### 1.1.1 激发POJO的潜能

很多框架通过强迫应用继承它们的类或者接口从而导致应用与框架绑死，这种侵入式的编程方式在早期版本的Struts、WebWork、Tapestry以及无数其他的Java规范和框架中都能看到

**Spring竭力避免因自身的API而弄乱你的应用代码。Spring不会强迫你实现Spring的规范的接口或继承Spring规范的类，相反，在基于Spring构建的应用中，它的类通常没有任何痕迹表明你使用Spring。最坏的场景是，一个类或许会使用Spring注解，但它依旧是POJO。**

> 程序清单1.1 Spring不会在HelloWorldBean上有任何不合理的要求
```Java
package com.habuma.spring;

public class HelloWorldBean {
    public String sayHello() {
        return "Hello World";
    }
}
```

Spring的非侵入编程模型意味着这个类在Spring应用中和非Spring应用中都可以发挥同样的作用
全
Spring赋予POJO魔力的方式之一就是通过DI来装配它们