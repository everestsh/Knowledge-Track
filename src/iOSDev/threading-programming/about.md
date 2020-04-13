# 关于多线程编程

### 概要和术语

多线程编程有利也有弊：在提升程序运行效率和用户体验的同时也带来了线程间同步的种种问题。现在大多数 CPU 都是多核的，所以很多程序都是并发执行来提升性能。

* 提升交互响应体验
* 提升多核设备的实时性能

多线程编程的代码更复杂，尤其是在存取同一个数据时更需要注意。

如果把进程(process)理解为一个运行着多个线程(thread)的程序，线程运算调度一条独立路径的代码。任务(work)可以理解成需要被执行的代码块或函数之类的抽象概念。

### 替代技术

亲自去创建底层意义上的线程很难操作，也很容易出问题。创建一个新的线程会消耗很多 CPU 和内存资源，所以尽可能先考虑下是否真的有必要创建线程。对于直接手动创建线程执行任务来说，可以替代的技术还有很多：

- Operation objects: 封装了一套在辅助线程执行任务的 API，只关注提交的任务即可，线程管理的底层工作交给系统。
- **Grand Central Dispatch**: 也是基于任务的 API，功能更强大，队列的使用更加灵活，**强烈推荐**。
- Idle-time notifications: 如果一项任务较轻且优先级较低，可以趁着不忙的时候执行。使用 `NotificationCenter` 的 [`post(_:)`](https://developer.apple.com/documentation/foundation/notificationcenter/1410472-post) 方法可以立即发出通知，这是个同步执行的方法。其实通知会先进入一个先入先出的通知队列中，出列后才会被分发到通知中心。可以使用 `NotificationQueue` 的 [`enqueue(_:postingStyle:)`](https://developer.apple.com/documentation/foundation/notificationqueue/1416340-enqueue) 方法异步发送通知，**postingStyle 设为 [`.whenIdle`](https://developer.apple.com/documentation/foundation/notificationqueue.postingstyle/1418001-whenidle) 即可在 RunLoop 空闲时发送通知**。
- Asynchronous functions: 使用系统自带的异步函数，把线程创建和管理的工作交给系统。
- Timers: 使用定时器在主线程做一些微小的周期性任务，而无需手动创建线程。（参见：[Timer Sources](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW21)）
- Separate processes: 如果任务跟应用关联不紧密、占用大量内存或是需要 root 权限，可以创建进程，而非线程。使用 `fork` 函数创建进程后记得调用 `exec`。

### 苹果系统的支持

macOS 和 iOS 提供了几种创建线程的技术，以及对线程之间管理和同步任务的支持。

#### 对线程的封装

线程的底层实现是在 Mach 层的，虽然它提供了抢占式多任务处理模型和线程调度能力，但我们常用的还是 POSIX API 及其衍生出来的 API：

1. Cocoa 线程: `NSThread` 类以及 `NSObject` 提供的线程 API。参见：[线程管理](./thread-management.md)
2. POSIX 线程: C 语言接口，比如创建线程的函数 `pthread_create`。 `pthread_t` 为线程句柄。
3. ~~Multiprocessing Services: 远古产物，弃用~~

不同系统中线程的生命周期都差不多，在线程退出之前，会在**运行(running)、阻塞(blocked)、准备(ready)**状态之间切换。创建线程需要指定其入口函数，入口函数 `return` 后会终止线程，线程会被系统回收。创建线程会占内存和 CPU 资源，所以在入口函数里执行比较重的任务才值当。或者可以用 RunLoop 做一些循环的任务。

#### Run Loops

run-loop 管理了线程接收的事件。它监听事件 source，有事件发生时，系统会唤起线程并将事件发送给 run-loop，然后调用事件对应的回调处理函数。没有事件发生时会让线程休眠。

如果没有 run loop，线程的入口函数 `return` 后就会终止线程。**run loop 可以让线程保活的同时消耗最少的资源。**它在没有事件发生时会让线程休眠，而不是让 CPU 空转。**run loop 与线程是一对一的关系，线程创建时默认是没有 run loop 的。**

**系统的主线程自动配置好了 run loop**，可以通过 **`CFRunLoopGetMain()`** 获得。其他线程可以使用 **`CFRunLoopGetCurrent()`** 获得对应的 run loop 对象，**在第一次执行 `CFRunLoopGetCurrent()` 时，线程才会创建它的 run loop**。获得 run loop 对象后，可以配置其事件处理函数，并运行 run loop。

run loop 对应的 API 有两种：`NSRunLoop` 和 `CFRunLoop`：

- `NSRunLoop` Cocoa 的 API，**非线程安全，必须在当前线程上下文中调用**。它是对 `CFRunLoop` 的封装。
- `CFRunLoop` 为 `Core Foundation` 框架的 C 语言 API，**线程安全**。使用 `CFRunLoopRef` 引用 run loop 对象。

#### 同步工具

多线程操作同一资源时要注意同步的问题，可以使用**加锁**、**条件变量**、**原子操作**等技术进行同步。

**加锁可以确保一段代码在某一时刻只能在一个线程中执行**，最基本的是互斥锁(mutex, mutual exclusion)。Cocoa 提供了很多种锁来满足各种场景。

**当两个线程竞争同一资源时，如果对资源的访问顺序敏感，就称存在竞态条件。**条件变量通过**阻塞线程的方式**确保了任务按正确的顺序执行，POSIX 层和 Foundation 框架都有条件变量对应的 API。此外，Cocoa 提供的 operation objects 也能设置任务执行的顺序。

**原子操作适合同步多线程对标量数据类型的数学和逻辑运算**，它跟锁相比，采用**硬件指令优化**，是一种轻量级同步工具。

> 链接： [Swift Concurrency](https://github.com/uber/swift-concurrency)

#### 跨线程通信

因为线程之间共用它们所属进程的相同空间，所以跨线程通信的方法有很多。它们各有优劣，按复杂度从低到高排列如下：

- NSObject 对象直接发消息: [`perform(_:on:with:waitUntilDone:)`](https://developer.apple.com/documentation/objectivec/nsobject/1414476-perform) 等方法。参见：[Cocoa Perform Selector Sources](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/Multithreading/RunLoopManagement/RunLoopManagement.html#//apple_ref/doc/uid/10000057i-CH16-SW44)
  - 依赖于 run loop 机制
- **全局变量，共用内存和对象**: 相比直接发消息**更快更容易**，但也**更脆弱**。需要加锁之类的同步机制来确保代码的正确性，否则可能会导致竞态条件、数据错乱和崩溃。
- **条件变量**: 之前说过它也是一种线程同步工具，只有当符合某个条件时才让线程执行下去，相当于守门员的作用。
- **Run loop sources**: run loop input sources 有两种：port-based 和 custom。这里说的是使用 custom run loop source 在某个线程上接收应用特定的消息。整个事件分发机制需要自己实现，包括设置 handler 函数，为 custom run loop source 提供数据，并手动给它发信号（signal）。
- **端口和套接字**: 基于端口的跨线程通信技术更复杂但也更可靠。更重要的是端口和套接字也可以与外部实体通信，比如其他进程和服务。**为了高效，端口使用 port-based run loop sources 实现**。
- ~~**消息队列**: 古老的 Multiprocessing Services 定义了一个简单方便的 FIFO 队列来管理数据进出，但没其他跨线程通信技术效率高。~~
- ~~Cocoa distributed objects: 对端口通信进行高级封装的 Cocoa API，其开销之大更适合跨进程通信。不建议用于跨线程通信，杀鸡用牛刀。~~

### 设计要点

这些建议和经验可以帮助开发者确保代码逻辑正确，性能更佳。

#### 避免直接创建线程

手动创建线程很蛋疼还容易出错，所以要用其他 API 隐式实现并发，之前也提到了一些替代技术。建议使用 GCD 和 operation objects，可以根据当前系统负载自动调整活跃线程数量。

#### 让线程占用率适当

手动创建管理线程时要注意线程消耗宝贵的系统资源，要做到物尽其所用，不能杀鸡用牛刀。更要毫不犹豫地终止大部分时间处于空闲状态的线程。线程占用的大量内存中有一部分是联动(wired)内存，所以释放那些使用率低的线程不仅可以减少应用的内存占用，也会为其他系统进程的运行释放更多物理内存。

科普下苹果的内存使用相关术语：

- **Wired(联动)**: 系统核心占用的，一直存在 RAM 上，永远不会被挪到硬盘中。联动内存占用跟使用的应用有关。
- Active(活跃): 表示这些内存数据正在使用中，或者刚被使用过。
- Inactive(非活跃): 表示这些内存中的数据是有效的，但是最近没有被使用。如果打开一个应用再退出，其所占用内存会变为非活跃内存，再次打开这个应用时如果那块内存没被其他应用使用，那么会那块非活跃内存会变为活跃内存（无需从硬盘加载），使得应用打开速度更快。
- Free(可用空间): RAM 中没被使用的空间。

在终止线程前需要记录下当前的性能指标，并在终止线程之后再次记录，参照对比下是否真的提升了性能。

#### 避免共用数据结构

避免线程相关资源冲突**最简单的方法是每个线程都拷贝一份需要用的数据**。**线程之间通讯越少越好。**

即便对所有多线程场景下的共用数据加锁，就算再怎么仔细，代码可能依然在语义上不安全。比如代码逻辑要求用特定的顺序修改公用的数据结构，否则就会出问题。使用基于事务模型的代码可以弥补这个缺陷，但会进而抵消了多线程的性能优势。将资源竞争消灭在萌芽之中才会让方案更简单，性能更出众。

#### 线程与用户界面

建议在接受跟用户相关的事件和更新界面时使用主线程。因为 Cocoa 跟 UI 相关的 API 使用了一些全局变量，如果在其他线程去更新 UI 或者处理用户事件，就会发生一些同步问题。

当然这个『在主线程更新界面』的规则也有些例外，比如在次级线程进行图像处理可以显著地提升性能。如果拿不准主意，那就干脆全在主线程去做吧。这样代码逻辑简单，易维护。

Cocoa 图形绘制可以参考 [Cocoa Drawing Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaDrawingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40003290)。

#### 注意退出应用时线程的行为

POSIX 线程按照资源释放方式分两种：joinable(non-detached) 和 detached。

detached 类型的线程结束之后系统会自动回收其资源。假如在 A 线程中使用 `pthread_create` 创建线程 B，不传入线程属性的话默认是 joinable 类型的线程。需要在线程 A 调用 `pthread_join` 来连接，这样才会在线程 B 结束后回收其资源。

**默认情况下只有主线程是 joinable 的，当所有 joinable 线程都结束了，它们所属的进程也就终止执行了**。因为系统会认为 detached 线程做的都是可选的任务，所以当用户退出了一个应用时，通常会立刻结束所有的 detached 线程。

如果想让线程在后台做一些诸如保存数据到磁盘之类的重要工作，需要使用 joinable 类型的线程，以防止应用退出时数据丢失。但大多数的对线程高层封装的 API 不会默认创建 joinable 线程，所以需要使用 POSIX API 的 `pthread_create` 创建 joinable 线程。

在使用 Cocoa API 时，`applicationShouldTerminate:` delegate 方法可以延迟一阵子退出应用或取消退出。如果需要延迟退出程序，还要在所有关键线程完成任务之后调用 `replyToApplicationShouldTerminate:` 方法告诉 `NSApplication` 对象是否可以真的退出了。

#### 处理异常

每个线程都负责捕获和处理其调用栈上抛出的异常，任何线程上没被捕获的异常都能终止其所属进程。**不能将没捕获的线程抛给其他线程处理。**

如果想要通知其他线程有异常，应该先捕获异常，然后给其他线程发消息。异常被捕获后可能会继续执行，或者等待命令，或者退出。

Cocoa 的 `NSException` 被捕获后可以在线程之间传递。

`@synchronized` 会自动捕获和处理异常。

#### 干净利落地终止线程

最好是让线程运行结束后自动退出，如果万不得已非要立刻终止线程，**会导致线程没有释放和清理它占用的资源**，比如：创建的内存、打开的文件以及获得的其他资源。无法回收这些资源意味着内存泄露和其他潜在的问题。

#### Library 中的线程安全

**开发第三方库的时候必须假设调用方随时都会处于多线程环境中，代码中的关键部分一定要加锁。**虽然可以监听 `NSWillBecomeMultiThreadedNotification` 通知来获知应用处于多线程，但可能 library 被调用之前就已经发过通知了。别指望当调用方处于多线程环境时才创建锁，而是需要在调用初始化 library 的方法中就提前创建好锁对象。但如果在 library 的静态初始化中创建锁会延长 library 的加载时间，影响性能。

互斥锁的加锁和解锁操作要成对出现，不要指望调用方提供一个线程安全的环境，对数据结构该用锁就用。