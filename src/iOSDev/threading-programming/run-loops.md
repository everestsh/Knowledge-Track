# Run Loops

Run Loop 作为线程相关基础设施的一部分，充当着**循环处理**、**调度事件/消息**的角色。它使得线程不会执行完单个任务后就立刻结束，而是让线程在没有任务时保持休眠状态，在需要处理消息时被立刻唤醒。

Run Loop 其实是个对象，但不需要主动去创建它，而且**每个线程都有对应的 run loop 对象**。Run Loop 的管理机制并不完全是自动的，有时需要设计好 run loop 的运行时间和事件处理回调。**次级线程需要开发者手动去配置并运行它的 run loop**，在应用启动过程中**主线程的 run loop 已经自动配置并运行**了。

Run Loop 作为苹果提供的 [Event Loop](https://en.wikipedia.org/wiki/Event_loop) 机制的实现方案，在 Cocoa 和 Core Foundation 有两个对应的类：[`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) 和 [`CFRunLoop`](https://developer.apple.com/documentation/corefoundation/cfrunloop)。

## Run Loop 剖析

Run Loop 可能需要开发者自己写 `while` 或 `for` 循环，并在里面驱动 run loop 对象运行，每轮运行都会处理接收到事件的回调。

Run Loop 接收的**事件来源 (source)** 有两种。

- **Input Source** 传送来自其他应用或线程的异步事件/消息；
- **Timer Source** 传送的是基于定时器的同步事件，可以定时或重复发送。

下图展示了 run loop 与多种 source 的概念架构。运行 `RunLoop` 实例的方式有三种，[`run(until:)`](https://developer.apple.com/documentation/foundation/runloop/1415778-run) 方法是其中的一种，后面会讲。**Input Source 发送的异步事件产生的回调会使 `run(until:)` 退出；Timer Source 则不会。**

![Structure of a run loop and its sources](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Multithreading/Art/runloop.jpg)

Run Loop 的一些行为会发通知，开发者可以注册成为 run-loop 观察者 (observer)。

Input Source, Timer Source, Run Loop Observer 统称为 Mode Item，这里的 Mode 指的是 Run Loop Mode。一个 Run Loop 包含若干个 Mode，每个 Mode 又包含若干个 Item。Item 与 Mode 是多对多的关系，没有 Item 的 Model 会立刻退出。

下面几节会详细讲述上面提到的这些概念。

// TODO
