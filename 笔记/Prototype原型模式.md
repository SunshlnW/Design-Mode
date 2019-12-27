# Prototype原型模式

## “对象创建”模式

1. 通过“对象创建”模式绕开new，来避免对象创建（new）过程中所导致的紧耦合（依赖具体类），从而支持对象创建的稳定。它是接口抽象之后的第一步工作。
2. 典型模式
    * Factory Method
    * Abstract Factory 
    * Prototype
    * Builder

## 动机（Motivation）

1. 在软件系统中，经常面临着“**某些结构复杂的对象**”的创建工作；由于需求的变化，这些对象经常面临着剧烈的变化，但是它们却拥有比较稳定一致的接口。
2. 如何应对这种变化？如何向“客户程序（使用这些对象的程序）”隔离出“这些易变对象”，从而使得“依赖这些易变对象的客户程序”不随着需求改变而改变？

## 模式定义

使用**原型实列**指定创建对象的种类，然后通过**拷贝（深拷贝）**这些原型来创建新的对象。
                                                ---《设计模式》GoF

```c++
//Prototype.cpp
//抽象类
class ISplitter{
public:
    virtual void split()=0;
    virtual ISplitter* clone()=0; //通过克隆自己来创建对象
    virtual ~ISplitter(){}
};

//ConcretePrototype.cpp
//具体类
class BinarySplitter : public ISplitter{
public:
    virtual ISplitter* clone(){
        return new BinarySplitter(*this);
    }
};

class TxtSplitter: public ISplitter{
public:
    virtual ISplitter* clone(){
        return new TxtSplitter(*this);
    }
};

//Client.cpp
class MainForm : public Form
{
    ISplitter*  prototype;//原型对象
public:
    MainForm(ISplitter*  prototype){
        this->prototype=prototype;
    }
    void Button1_Click(){
        ISplitter * splitter=
            prototype->clone(); //克隆原型
        splitter->split();
    }
};
```

## 结构（Structure）

![20191227224104.png](https://raw.githubusercontent.com/SunshlnW/Design-Mode/master/image/Prototype%E5%8E%9F%E5%9E%8B%E6%A8%A1%E5%BC%8F/20191227224104.png)

*Prototype*对应于*ISplitter*，*ConcretePrototype1*和*ConcretePrototype2*对应于*BinarySplitter*和*TxtSplitter*，*Client*对应于*MainForm*。

## 要点总结

1. Prototype模式同样用于隔离类对象的使用者和具体类型（易变类）之间的耦合关系，它同样要求这些“易变类”拥有“**稳定的接口**”。
2. Prototype模式对于“如何创建易变类的实体对象”采用“原型克隆”的方法来做，它使得我们可以非常灵活地动态创建“拥有某些稳定接口”的新对象---所需工作仅仅是**注册一个新类的对象（即原型）**，然后在任何需要的地方Clone。
3. Prototype模式中的Clone方法可以利用某些框架中的序列化来实现深拷贝。
4. 比如有些对象易变或者比较复杂，但是其接口比较稳定，比如工厂模式中实现，假设我们不是需要对象的最初始new状态，我们需要对象的某一时刻状态，这时可以使用原型模式来Clone该状态的对象。
