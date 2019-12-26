# Factory Method工厂方法

## “对象创建”模式

1. 通过“对象创建”模式绕开new，来避免对象创建（new）过程中所导致的紧耦合（依赖具体类），从而支持对象创建的稳定。它是接口抽象之后的第一步工作。
2. 典型模式
    * Factory Method
    * Abstract Factory
    * Prototype
    * Builder

## 动机（Motivation）

1. 在软件系统中，经常面临着创建对象的工作；由于需求的变化，需要创建的对象的具体类型经常变化。
2. 如何应对这种变化？如何绕过常规的对象创建方法（new），提供一种“封装机制”来避免客户程序和这种“具体对象创建工作”的紧耦合？

## 模式定义

定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使得一个类的实例化**延迟（目的：解耦，手段：虚函数）到子类**。
                                                ---《设计模式》GoF

```c++
//ISplitterFactory.cpp

//抽象类
class ISplitter{
public:
    virtual void split()=0;
    virtual ~ISplitter(){}
};

//工厂基类
class SplitterFactory{
public:
    virtual ISplitter* CreateSplitter()=0;
    virtual ~SplitterFactory(){}
};


//MainForm2.cpp
class MainForm : public Form
{
    //依赖于抽象基类Splitter Factory
    SplitterFactory*  factory;//工厂
public:
    MainForm(SplitterFactory*  factory){
        this->factory=factory;
    }
    void Button1_Click(){
        //依赖于抽象基类ISplitter
        ISplitter * splitter=
            factory->CreateSplitter(); //多态new
        splitter->split();
        }
};
```

将MainForm中各具体类的创建利用虚函数和抽象基类延迟到具体工厂时创建，MainForm类只依赖两个抽象基类，并不依赖具体实现！

## 结构（Structure）

![20191226231812.png](https://raw.githubusercontent.com/SunshlnW/Design-Mode/master/image/Factory%20Method%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95/20191226231812.png)

## 要点总结

1. Factory Method模式用于隔离类对象的使用者和具体类型之间的耦合关系。面对一个经常变化的具体类型，紧耦合关系（new）会导致软件的脆弱。（将具体类如*BinarySplitter*的创建从*MainForm*中分离开来）
2. Factory Method模式通过面向对象的手法，将所要创建的具体对象工作**延迟**到子类，从而实现一种**扩展（而非更改，具体工厂和具体类直接继承扩展）**的策略，较好地解决了这种紧耦合关系。
3. Factory Method模式解决“**单个对象**”的需求变化。**缺点在于要求创建方法/参数相同**。
