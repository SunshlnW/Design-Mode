# Singleton单件模式

## “对象性能”模式

1. 面向对象很好地解决了“抽象”的问题，但是必不可免地要付出一定的代价。对于通常情况来讲，面向对象的成本大都可以忽略不记。但是某些情况，面向对象所带来的成本必须谨慎处理。
2. 典型模式
    * Singleton
    * Flyweight

## 动机（Motivation）

1. 在软件系统中，经常由这样一些特殊的类，必须保证它们在系统中只存在一个实例，才能确保它们的逻辑正确性、以及良好的效率。
2. 如何绕过常规的构造器，提供一种机制来保证一个类只有一个实例？
3. 这应该是类设计者的责任，而不是使用者的责任。

## 定义

保证一个类仅有一个实例，并提供一个该实例的全局访问点。
                                                ---《设计模式》GoF

## 结构（Structure）

![20200101220813.png](https://raw.githubusercontent.com/SunshlnW/Design-Mode/master/image/Singleton%E5%8D%95%E4%BB%B6%E6%A8%A1%E5%BC%8F/20200101220813.png)

```c++
//线程非安全版本，多线程环境下不安全
Singleton* Singleton::getInstance() {
    if (m_instance == nullptr) {
        m_instance = new Singleton();
    }
    return m_instance;
}

//线程安全版本，但锁的代价过高
Singleton* Singleton::getInstance() {
    //lock锁是临时变量，函数结束后会自动析构。
    Lock lock;
    if (m_instance == nullptr) {
        m_instance = new Singleton();
    }
    return m_instance;
}

//双检查锁，但由于内存读写reorder不安全
//如果编译器对指令reorder，可能会出现先给m_instance先分配内存，
//那么m_instance就不是nullptr,但是并没有new构造对象，所以并不能使用m_instance。
Singleton* Singleton::getInstance() {
    if(m_instance==nullptr){
        //lock锁是临时变量，函数结束后会自动析构。
        Lock lock;
        if (m_instance == nullptr) {
            m_instance = new Singleton();
        }
    }
    return m_instance;
}

//C++ 11版本之后的跨平台实现 (volatile)
std::atomic<Singleton*> Singleton::m_instance;
std::mutex Singleton::m_mutex;

Singleton* Singleton::getInstance() {
    Singleton* tmp = m_instance.load(std::memory_order_relaxed);
    std::atomic_thread_fence(std::memory_order_acquire);//获取内存fence
    if (tmp == nullptr) {
        std::lock_guard<std::mutex> lock(m_mutex);
        tmp = m_instance.load(std::memory_order_relaxed);
        if (tmp == nullptr) {
            tmp = new Singleton;
            std::atomic_thread_fence(std::memory_order_release);//释放内存fence
            m_instance.store(tmp, std::memory_order_relaxed);
        }
    }
    return tmp;
}
```

## 要点总结

1. Singleton模式中的实例构造器可以设置为protected以允许子类派生。
2. Singleton模式一般不要支持拷贝构造函数和Clone接口，因为这可能导致多个对象实例，与Singleton模式的初衷违背。
3. 如何实现多线程环境下安全的Singletion？注意对双检查锁的正确实现。
