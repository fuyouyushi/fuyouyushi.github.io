---
title: 'Java 中的getName(), getCanonicalName()和getSimpleName()'
date: 2018-08-09 17:41:50
tags: Java
---

先定义一个类和一个它的内部类:

<!-- more -->

```
class Name {
  class InnerClass {
  }
  
  void printName() {
    
  }
}
```

# 外部类的差别

先打印出Name 的getName(), getCanonicalName()和getSimpleName() 这三个的值：

```
System.out.println(Name.class.getName());
System.out.println(Name.class.getCanonicalName());
System.out.println(Name.class.getSimpleName());
```

输出结果

```
com.testjava.testclassname.Name
com.testjava.testclassname.Name
Name
```

结论：可以看到对于一个外部类， getName() 和 getCanonicalName() 结果一样，都打印出了包含类的包名的路径，getSimpleName() 只打印出了不包含路径的简单的类名。

# 内部类的差别

打印逻辑如下:

```
System.out.println(Name.InnerClass.class.getName());
System.out.println(Name.InnerClass.class.getCanonicalName());
System.out.println(Name.InnerClass.class.getSimpleName());
```

输出结果:

```
com.testjava.testclassname.Name$InnerClass
com.testjava.testclassname.Name.InnerClass
InnerClass
```

结论：从结果看，getName() 使用了 *Name$InnerClass* 来标识InnerClass 是Name 的一个内部类，getCanonicalName() 则仍是打印出了和外部类一样的包名和路径，getSimpleName() 仍然只是打印出了简单的类名。

# 匿名内部类的差别

打印逻辑如下:

```
public class MyClassName {
  public static void main(String[] args) {
    new Name() {
      @Override
      void printName() {
        System.out.println(this.getClass().getName());
        System.out.println(this.getClass().getCanonicalName());
        System.out.println(this.getClass().getSimpleName());
        System.out.println(“print end”);
      }
    }.printName();
}
```
输出结果:

```
com.testjava.testclassname.MyClassName$1
null

print end
```
结论：对于一个匿名内部类，getName()打印出来的是MyClassName$1, getCanonicalName()对于一个匿名内部类打印出了null, getSimpleName() 的结果则是一个空的字符串。

# 数组对象差别

打印代码逻辑如下:

```
System.out.println((new Name[10]).getClass().getName());
System.out.println((new Name[10]).getClass().getCanonicalName());
System.out.println((new Name[10]).getClass().getSimpleName());

System.out.println((new int[10]).getClass().getName());
System.out.println((new int[10]).getClass().getCanonicalName());
System.out.println((new int[10]).getClass().getSimpleName());
```

输出结果如下:

```
[Lcom.testjava.testclassname.Name;
com.testjava.testclassname.Name[]
Name[]
[I
int[]
int[]
```
结论：对于数组类型，getName()通过一个或者多个 `[` 来表示数组，`[` 后面紧接着的是元素类型；   getCanonicalName() 则是用 `[]` 来表示数组，对于基础类型则是直接显示出来；getName() 同样也用 `[]` 来表示数组类型.

> 附：
getName()中提到的元素类型，对应关系如下所示:

|Element Type	| Encoding  |
|:-- |:-- |
|boolean	| Z  |
|byte	| B |
|char	|C |
|class or interface	|Lclassname; |
|double |D |
|float	|F |
|int |I |
|long	|J |
|short	|S |








