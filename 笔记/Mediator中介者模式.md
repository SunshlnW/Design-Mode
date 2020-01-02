# Mediator中介者模式

## 接口隔离模式

1. 在组建构建过程中，某些接口之间直接的依赖常常会带来很多问题、甚至根本无法实现。采用添加一层**间接**（稳定）接口，来隔离本来互相紧密关联的接口是一种常见的解决方案。
2. 典型模式
    * Facade
    * Proxy
    * Adapter
    * Mediator

## 动机（Motivation）

1. 在软件构建过程中，经常出现**多个对象**互相关联交互的情况，对象之间常常会维持一种**复杂**的引用关系，如果遇到一些需求的更改，这种直接的引用关系将面临不断的变化。
2. 在这种情况下，我们可使用一个“中介对象”来管理对象间的关联关系，避免相互交互的对象之间的紧耦合引用关系，从而更好地抵御变化。

## 模式定义

用一个中介对象来封装（**封装变化**）一系列的对象交互。中介者使各对象不需要**显示的相互引用**（**编译时依赖**--->运行时依赖），从而使其耦合松散（管理变化），而且可以独立地改变它们之间的交互。
                                                ---《设计模式》GoF

## 结构（Structure）

![20200102224135.png](https://raw.githubusercontent.com/SunshlnW/Design-Mode/master/image/Mediator%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F/20200102224135.png)

Colleague依赖于Mediator，ConcreteMediator依赖于ConcreteColleague1和ConcreteColleague2，而ConcreteColleague1和ConcreteColleague2并不互相依赖。

![20200102224300.png](https://raw.githubusercontent.com/SunshlnW/Design-Mode/master/image/Mediator%E4%B8%AD%E4%BB%8B%E8%80%85%E6%A8%A1%E5%BC%8F/20200102224300.png)

Facade解决的是系统外和系统内的隔离，而Mediator解决的是系统内各个对象间的隔离。

## 要点总结

1. 将多个对象间复杂的关联关系解耦，Mediator模式将多个对象间的控制逻辑进行集中管理，变“多个对象互相关联”为“多个对象和一个中介者关联”，简化了系统的维护，抵御了可能的变化。
2. 随着控制逻辑的复杂化，Mediator具体对象的实现可能相当复杂。这时候可以对Mediator对象进行分解处理。
3. Facade模式是解耦系统间（单向）的对象关联关系；Mediator模式是解耦系统内各个对象之间（双向）的关联关系。

