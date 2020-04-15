# 线程管理

每个进程都至少有一个线程，每个线程代表了一条执行代码的独立路径。应用从一个线程的 `main` 函数开始运行，然后产生新的线程。**每个线程在进程内部都是独立的实体，具有自己的调用栈，并由内核做时分调度。**线程可以与其他线程和进程**通信**，**执行 I/O 操作**等。一个应用进程内部的所有现场**共享虚拟内存空间**，并与进程拥有相同的访问权限。

## 线程成本

使用线程对应用和系统来说都是有成本的，具体体现在**内存使用和性能**上。每个线程需要占用内核内存空间和程序内存空间。内核会使用联动(wired)内存为线程创建用于管理线程和协调调度的数据结构。线程的栈空间和数据存储在程序的内存空间中。这些数据结构大部分会在创建线程的时候初始化，进程也因与内核有必要的交互而成本变得相对更高。

成本项    | 大致成本                                             | 注释
:----- | :----------------------------------------------- | :-------------------------------------------------------------------------
内核数据结构 | 大约 1 KB                                          | 用于存储线程数据结构和属性的内存，很多都是联动（wired）内存，所以不能在磁盘上分页
栈空间    | 次级线程：512 KB ; macOS 主线程：8 MB ; iOS 主线程：1 MB      | 次级线程的栈空间最小为 16 KB，且必须是 4 KB 的整数倍。创建线程的时候可以设置栈空间的大小，但是只有在需要用它的时候才会创建真的分页内存。
创建时间   | 大约 90 微秒（macOS 10.5，2 GHz Core Duo CPU，1 GB RAM） | 从开始创建线程到线程入口函数开始执行的耗时，是个粗略的估值。处理器负载、计算机速度、系统和程序可用内存都会对创建时间造成较大的影响。

由于底层内核的支持，operation objects 经常可以更快地创建线程。它使用**内核线程池**中已经存在的线程来节省创建时间，而不是每次都从零开始创建线程。

线程相关代码的开发工作也是一种**生产成本**。设计多线程程序有时需要对应用数据结构的组织方式进行彻底的改变。**避免使用同步**，因为它在设计较差的应用上会严重降低性能。设计数据结构和 debug 线程相关代码都会增加开发成本。

## 创建线程

创建线程需要指定入口主函数，并可设置一些线程的配置项。最后调用运行线程的方法。下面介绍几种创建线程的技术。

### 使用 `Thread`

使用 `Thread` 创建线程有两种方式：

1. 使用 [`detachNewThreadSelector(_:toTarget:with:)`](https://developer.apple.com/documentation/foundation/thread/1415633-detachnewthreadselector) 类方法产生新的线程
2. 创建一个新的 `Thread` 对象，并调用它的 `start` 方法。仅支持 iOS 和 macOS 10.5 以后的版本。

这两种方法都会创建一个 **detached** 线程，它**退出后系统会自动回收其资源**，无需手动 join 操作。

下面两种创建线程的代码是等效的，但是推荐使用第二种方式。因为它支持在运行线程之前**设置各种线程属性**，不像第一种方式那样创建线程的时候必须立刻运行。

```swift
// 方式 1
Thread.detachNewThreadSelector(#selector(myThreadMainMethod), toTarget: self, with: nil)

// 方式 2（推荐）
let myThread = Thread(target: self, selector: #selector(myThreadMainMethod), object: nil)
myThread.start()  // 实际创建线程
```

如果不想使用 `init(target:selector:object:)` 方法传入线程的入口函数，也可以继承 `Thread`，并覆写子类的 `main` 方法（不用调用 `super` 方法）。

`perform(_:on:with:waitUntilDone:)` 方法可以在正在运行的线程发送消息，前提是这个线程必须有运行中的 run loop，因为消息会在线程的 run loop 处理过程中被执行。**注意使用此方法进行跨线程通信时需要有同步机制。**此方法并不适合用于实现对高即时性有一定要求和频繁的跨线程通信。

### 使用 POSIX 线程

iOS 和 macOS 提供了基于 C 语言的 POSIX 线程 API 来创建线程，优点是跨平台。创建线程的函数是 `pthread_create`，默认**创建的是 joinable 线程**，所以需要设置线程属性来将其创建为 detached 线程，这样当线程退出的时候其资源就会立刻被系统回收利用了。

对于 C 语言的应用，可以使用端口、条件变量或共用内存来跨线程通信。跨线程通信有便于主线程检查其他线程的状态，在应用退出时做一些操作。

### 使用 `NSObject` 产生线程

iOS 和 macOS 10.5 之后 `NSObject` 提供了`performSelectorInBackground:withObject:` 方法，可以让所有对象都创建 detached 线程，并传入 `selector` 作为入口函数。跟它等效的方法是 `NSThread` 的 `detachNewThreadSelector:toTarget:withObject:`。在 `selector` 方法里可以继续配置线程，比如添加 `autoreleasepool` 和 run loop。

### 在 Cocoa 应用中使用 POSIX 线程

在 Cocoa 中使用 POSIX 线程需要注意它们之间的交互，并遵守以下原则。

#### 保护 Cocoa 框架

**Cocoa 框架使用加锁之类的同步机制来确保多线程工作正常，但只有在第一次使用 `Thread` 产生新线程的时候才会真的创建锁。这可以避免单线程情况下加锁会降低性能。**但如果只使用 POSIX API 来产生新线程，Cocoa 就不会收到应用转换为多线程的通知，进而导致应用不稳定，甚至 crash。可以用 `Thread` 创建一个新线程，入口函数啥都不做，这样线程就会立刻退出。这样 Cocoa 就知道应用处于多线程了，加锁也会生效。

PS：这么抖机灵的做法当然也会产生创建线程的开销，有些违背之前说的不要随意创建线程的原则。

`Thread` 的 `isMultiThreaded` 方法可以检查应用是否是多线程。

#### 混合使用 POSIX 和 Cocoa 锁

在一个应用中混合使用 POSIX 和 Cocoa 两套 API 的锁是安全的，因为后者实质上只是对前者的封装。但不能用一种 API 的方法操作另一种 API 创建的锁。比如不能使用 `NSLock` 对象操作 `pthread_mutex_init` 函数创建的互斥锁，反之亦然。

## 配置线程属性

### 配置线程的栈尺寸

创建线程后系统会为其创建一块内存作为栈空间，开发者可以在创建线程前配置这块空间的尺寸。所有的线程技术都会提供某种设置栈尺寸的方法，但 `Thread` 只有在 iOS 和 macOS 10.5 之后支持设置栈的尺寸。

技术                       | 选项
:----------------------- | :------------------------------------------------------------------------------------------------------------------
Cocoa                    | 前提是不要使用 `detachNewThreadSelector(_:toTarget:with:)` 创建线程。创建线程对象后，在调用线程对象的 `start` 方法之前，使用 `setStackSize:` 方法指定栈的大小。
POSIX                    | 创建 `pthread_attr_t` 栈属性结构体并使用 `pthread_attr_setstacksize` 函数修改默认的栈尺寸。在使用 `pthread_create` 函数创建线程时将属性结构体传入。
Multiprocessing Services | 使用 `MPCreateTask` 函数创建线程时可以传入栈尺寸。

### 配置 TLS(Thread-Local Storage)

每个线程都维护了一个存储键值对的字典，在线程内随处都能存取。可以用它**记录一些贯穿于线程执行过程中的信息，比如 run loop 迭代次数。**

Cocoa 与 POSIX 存储线程字典的方式不同，所以两种技术的 API 不能混用。始终用其中一种技术。

具体方式如下：

- Cocoa: `Thread` 的 [`threadDictionary`](https://developer.apple.com/documentation/foundation/thread/1411433-threaddictionary) 方法获取 `NSMutableDictionary` 字典对象，然后进行存取。
- POSIX: 直接用 `pthread_setspecific` 和 `pthread_getspecific` 函数存取字典。

### 设置线程的 Detached 状态

大部分高级线程技术创建的线程默认都是 detached 状态，这是因为大部分场景下都需要系统在线程执行完成后立刻回收资源。好处是代码干净，是否获取线程执行结果可以自行决定。

joinable 线程需要其他线程调用 `pthread_join` 函数对其进行 join 操作之后才能被系统回收资源。joinable 线程可以向 `pthread_exit` 函数传入数据，其他线程可以在调用 `pthread_join` 函数的时候获得此数据。参考 [pthread_join() and pthread_exit()](https://stackoverflow.com/questions/8513894/pthread-join-and-pthread-exit)。

应用退出时，detached 线程可以立刻结束，但是 joinable 线程必须全都被 join 后进程才能退出。**因此 joinable 线程适用于执行不能中断的重要工作，比如向磁盘写入数据。**

只能用 POSIX API 来创建 joinable 线程，如果不设置线程属性，默认就是 joinable 线程。可以在创建线程之前调用 `pthread_attr_setdetachstate` 函数修改线程 detached 状态。在线程开始运行之后，可以调用 `pthread_detach` 函数将一个 joinable 线程变成 detached 线程。

### 设置线程优先级

任何新创建的线程都有个默认优先级。内核的调度算法在决定运行哪个线程的时候都会考虑到线程优先级，优先级越高越可能先运行。**拥有更高优先级的线程并不保证有特定的运行时间，只是在与低优先级线程比较时更容易被调度算法选中罢了。**

**最好让你创建的线程保持默认优先级**。提升一些线程的优先级也会提升低优先级线程饥饿的可能性。如果高优先级线程和低优先级线程之间有交互，低优先级线程饥饿可能会阻塞其他线程，并造成性能瓶颈。(**优先级倒置**)

设置线程优先级的方式如下：

- Cocoa: `Thread` 的 `setThreadPriority:` 类方法可以设置当前线程的优先级。
- POSIX: `pthread_setschedparam` 函数设置优先级。

## 编写线程入口程序

各平台上的线程入口函数结构都差不多，一般都会初始化数据结构，执行一些工作，或者使用 run loop 保活，最后在工作完成后清理占用的资源。这里讲一下编写线程入口程序需要做的一些额外步骤。

### 创建自动释放池

使用 Objective-C 框架的应用必须**确保每条线程至少有一个自动释放池**。

使用 `@autoreleasepool` 创建自动释放池比 `NSAutoreleasePool` 更方便。而且 `NSAutoreleasePool` 不能在 ARC 下使用。

```swift
@objc func myThreadMainMethod() {
    autoreleasepool {
        // Do thread work here
    }
}
```

有时需要创建更多的自动释放池，比如在循环内部使用自动释放池降低内存开销。

### 设置 Exception Handler

未被捕获的异常会导致应用崩溃，虽然最好应该在异常发生的地方进行异常处理，但是在线程的入口函数再加上个 try/catch 比较好。

更多内容参考： [Exception Programming Topics](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/Exceptions/Exceptions.html#//apple_ref/doc/uid/10000012i)

### 设置 Run Loop

编写线程上运行的代码有两种选择：

1. 代码简单粗暴，任务执行完线程就退出了；
2. 设置线程的 run loop，处理不断到来的请求。

macOS 和 iOS 内置了 run loop 的实现，应用的主线程自动开启 run loop。开发者创建的次级线程需要手动配置和开启 run loop。

更多内容参考：[Run Loops](./run-loops.md)

## 终止线程

建议让线程的入口函数正常退出。虽然 Cocoa，POSIX 和 Multiprocessing Services 提供了直接杀线程的方法，但强烈不建议使用。**强杀线程会导致无法回收资源，可能导致内存泄露、资源没被正确清理，进而导致后续的隐患**。

如果预料到需要中途结束线程，设计线程之初就应该响应到取消或退出的消息。带有 run loop 这种周期操作的线程可以每次查看是否收到退出的消息。如果需要退出线程，则可以清理线程资源后退出；否则继续处理其他工作。

run loop 可以使用 input source 接受其他线程发的消息，但需要为 `RunLoop` 配置 `CFRunLoopSourceRef`，假设这部分代码在 `myInstallCustomInputSource` 已经实现好了。下面的代码还省略了 `while` 主循环中做的实际工作。将标记是否需要退出线程的局部变量 `exitNow` 的值放在 TLS 中同步是为了方便 input source 的事件处理函数对 `exitNow` 的存取，因为事件处理是在另外一个函数，不能直接存取 `exitNow`，需要利用 [TLS](#配置-tlsthread-local-storage)。当 input source 收到退出消息后，对应的事件处理函数便可以清理线程的资源，准备退出。

```swift
@objc func threadMainRoutine() {
    autoreleasepool {
        var moreWorkToDo = true
        var exitNow = false
        let runLoop = RunLoop.current

        // Add the exitNow BOOL to the thread dictionary.
        let threadDict = Thread.current.threadDictionary
        threadDict.setValue(exitNow, forKey: "ThreadShouldExitNow")

        // Install an input source.
        self.myInstallCustomInputSource()

        while moreWorkToDo && !exitNow {
            // Do one chunk of a larger body of work here.
            // Change the value of the moreWorkToDo Boolean when done.

            // Run the run loop but timeout immediately if the input source isn't waiting to fire.
            runLoop.run(until: Date())

            exitNow = threadDict.value(forKey: "ThreadShouldExitNow")
        }
    }
}
```
