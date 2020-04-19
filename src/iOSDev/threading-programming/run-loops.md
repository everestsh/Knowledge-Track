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

#### Run Loop Modes

Run Loop Mode 包含了**需要被监听的 input source 和 timer 集合，以及需要接收通知的 observer 集合**。Run loop 的每次运行都会处在某个特定模式下，而且**只有这个模式所包含的 item 集合才会参与发送事件(被监听)和接收通知**。

开发者使用 run loop mode 时直接指定名字就行，Cocoa 和 Core Foundation 定义了一些默认和常用的 Mode。Run Loop Mode 对应的类是 `CFRunLoopModeRef`，但是并没有作为公有 API 开放出来，但可以通过 Core Foundation [源码](https://github.com/apple/swift-corelibs-foundation/blob/master/CoreFoundation/RunLoop.subproj/CFRunLoop.c#L658)了解下:

```c
typedef struct __CFRunLoopMode *CFRunLoopModeRef;

struct __CFRunLoopMode {
    CFRuntimeBase _base; // CF 的基石，遍地可见
    pthread_mutex_t _lock; // 确保 CF 中的 Run Loop 线程安全
    CFStringRef _name; // Mode 的名字，比如 kCFRunLoopDefaultMode
    Boolean _stopped; 
    char _padding[3];
    CFMutableSetRef _sources0; // Input Sources 中的 Custom Input Source 集合
    CFMutableSetRef _sources1; // Input Sources 中的 Port-Based Source 集合
    CFMutableArrayRef _observers; // Run Loop Observers 数组
    CFMutableArrayRef _timers; // Timer Sources 数组
    CFMutableDictionaryRef _portToV1SourceMap; // 端口(port)与 sources1 的映射表
    __CFPortSet _portSet; // 端口集合
... 省略后面源码
};
```

Core Foundation 中所有实例都以 `CFRuntimeBase` 开始，仅限于内部使用。通过[它的结构](https://github.com/apple/swift-corelibs-foundation/blob/c5c35c1a59b0a9a05b2e1ffbf8a7bab0a3e59baa/CoreFoundation/Base.subproj/CFRuntime.h#L185)可以看出这里面保存了一些基本信息，比如 isa 指针，retainCount 等。

```c
/* All CF "instances" start with this structure.  Never refer to
 * these fields directly -- they are for CF's use and may be added
 * to or removed or change format without warning.  Binary
 * compatibility for uses of this struct is not guaranteed from
 * release to release.
 */
#if DEPLOYMENT_RUNTIME_SWIFT

typedef struct __attribute__((__aligned__(8))) __CFRuntimeBase {
    // This matches the isa and retain count storage in Swift
    uintptr_t _cfisa;
    uintptr_t _swift_rc;
    // This is for CF's use, and must match __NSCFType/_CFInfo layout
    _Atomic(uint64_t) _cfinfoa;
} CFRuntimeBase;

#define INIT_CFRUNTIME_BASE(...) {0, _CF_CONSTANT_OBJECT_STRONG_RC, 0x0000000000000080ULL}

#else

typedef struct __CFRuntimeBase {
    uintptr_t _cfisa;
#if TARGET_RT_64_BIT
    _Atomic(uint64_t) _cfinfoa;
#else
    _Atomic(uint32_t) _cfinfoa;
#endif
} CFRuntimeBase;
```

不同 Mode 直接是靠事件的来源 (Source) 区分的，而不是事件的类型。比方说 Mode 不能只搭配鼠标点击事件或键盘事件，但可以让某个 Mode 监听一些端口、暂停 timer、修改 source 和 observer 等。

下面的表格列出了[一些系统定义的 Mode](https://developer.apple.com/documentation/foundation/runloop/run_loop_modes)，大多数情况下会使用 Default Mode。[iphonedevwiki](http://iphonedevwiki.net/index.php/CFRunLoop) 列出了 Core Foundation 中更多的 Mode，很多是系统私有的。使用不同的 Mode 可以过滤不同 Source 发出的事件，比如在要求时效性操作的场景下使用自定义 Mode 来阻止低优先级 Source 发送事件。

| Mode           | 名称                                                         | 描述                                                         |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| Default        | NSDefaultRunLoopMode (Cocoa), kCFRunLoopDefaultMode (Core Foundation) | 大多数操作下最常用的 Mode，运行 Run Loop 和配置 Source 的首选 |
| ~~Connection~~ | ~~NSConnectionReplyMode (Cocoa)~~                            | ~~Cocoa 中结合 NSConnection 使用，用于监听回复(Reply)，极少用到。(已弃用)~~ |
| Modal          | NSModalPanelRunLoopMode (Cocoa)                              | Cocoa 中 modal panel 使用它接收与之相关 Source 的事件        |
| Event tracking | NSEventTrackingRunLoopMode (Cocoa), UITrackingRunLoopMode (Cocoa Touch) | Cocoa 用它限定鼠标拖拽事件之类的用户交互轨迹                 |
| Common modes   | NSRunLoopCommonModes (Cocoa), kCFRunLoopCommonModes (Core Foundation) | 可配置的通用模式集合，将某个 Input Source 关联到此 Mode 也会将其关联到集合中所有 Mode。Cocoa 框架中的 Common modes 默认包含 Default, Modal, Event tracking 三种 Mode；CF 框架起初只包含 Default，可以使用 `CFRunLoopAddCommonMode`函数向集合中添加自定义 Mode。 |

#### Input Sources

Input Sources 有两种实现：

* 基于端口(Port-based)
* 自定义(Custom)

它们**都向线程异步分发事件**，而**唯一的不同就是分发信号（signal）的方式**。基于端口的事件源会**自动由内核发信号**，自定义事件源需要**被其他线程手动发信号**。

Input Source 会被添加到一些 Mode 中，**如果某个 input source 不在当前的 Mode 中，那么它生成的事件在 run loop 处于正确的 mode 之前会先被 hold 住**。

##### Port-Based Sources(Source1)

Cocoa 和 Core Foundation 使用**端口相关的对象和函数提供了对创建基于端口的事件源的内建支持**。比如在 Cocoa 中，只需创建一个端口对象并使用 [`Port`](https://developer.apple.com/documentation/foundation/port) 的方法来向 run loop 添加端口。端口对象为你处理好了创建和配置 input source 的事情。

在 Core Foundation 中需要手动创建端口和 run loop source。涉及到的 API 有 [`CFMachPort`](https://developer.apple.com/documentation/corefoundation/cfmachport-rsc), [`CFMessagePort`](https://developer.apple.com/documentation/corefoundation/cfmessageport-rs2) , [`CFSocketRef`](https://developer.apple.com/documentation/corefoundation/cfsocket-rg7) 。

##### Custom Input Sources(Source0)

只能使用 Core Foundation 中的 [`CFRunLoopSource`](https://developer.apple.com/documentation/corefoundation/cfrunloopsource-rhr) 相关函数来创建自定义事件源。在处理到来的事件、从 run loop 移除 source 后都会有函数回调，可以通过实现这些回调函数来配置 source。

除此之外还需定义事件分发机制。source 有一部分是在单独的线程运行的，负责为 input source 提供数据，并在数据准备好后对 source 发信号。事件分发机制取决于开发者，但别弄得太过复杂。

##### Cocoa Perform Selector Sources

Cocoa 定义了一种在任何线程执行 `selector` 的 custom input source。与基于端口的事件源相同之处是在目标线程依次执行 `selector`，缓解了一条线程运行多个方法时可能发生的同步问题；不同之处在于 `selector` 执行后会将 source 从 run loop 挪走。

在任意线程 perform selector 的前置条件是线程必须有一个活跃的 run loop。对于自己创建的线程，`selector` 直到启动 run loop 之后才会运行；主线程会自动配置并运行 run loop，然而要在应用的 `applicationDidFinishLaunching:` delegate 方法调用后才生效。Run Loop **每次循环会处理队列中所有的 `selector`，而不是循环一次处理一个**。

下表中列出了 `NSObject` 类提供的在任何线程执行 `selector` 的 API。在任何线程下，只要能拿到 Objective-C 对象就能使用下面的 API，包括 POSIX 线程。这些方法并不会为了执行 `selector` 真的去创建一个新线程。

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `performSelectorOnMainThread:withObject:waitUntilDone:`, `performSelectorOnMainThread:withObject:waitUntilDone:modes:` | 在应用主线程 run loop 的下次循环执行特定的 `selector`，并提供了选项可以在执行 `selector` 之前阻塞当前线程。 |
| `performSelector:onThread:withObject:waitUntilDone:`, `performSelector:onThread:withObject:waitUntilDone:modes:` | 在任意 `NSThread` 对象执行 `selector`，同上。                |
| `performSelector:withObject:afterDelay:`, `performSelector:withObject:afterDelay:inModes:` | 在当前线程 run loop 的下次循环延迟一段时间执行 `selector`。因为需要等到下次 run loop 循环才会依次执行队列中的 `selector`，所以本身就会有一点延时。 |
| `cancelPreviousPerformRequestsWithTarget:`, `cancelPreviousPerformRequestsWithTarget:selector:object:` | 取消 `performSelector:withObject:afterDelay:` 或 `performSelector:withObject:afterDelay:inModes:` 方法向当前线程发送的消息。 |

#### Timer Sources

Timer source 会在未来一个预定时间向线程同步分发事件。线程可以用 Timer 来通知自己做一些事情。比如用户在搜索栏输入一连串字符之后的某个时间自动搜索一次结果。正是因为有了个延时，才让用户有机会在自动搜索发生前尽可能打出想要的搜索字符串。

Timer 并不是实时的，会有误差。如果一个 timer 不在正在运行的 run loop 监控的 mode 中，需要一直等到 run loop 运行在一个支持这个 timer 的 mode 时，timer 才会触发。如果一个 timer 触发的时候恰巧 run loop 正忙于执行某个 handler 程序，这个 timer 的 handler 程序需要等到下次才会通过 run loop 执行。如果 run loop 根本不在运行，timer 永远都不会触发。

可以配置 timer 只生成一次或重复多次事件。重复的 timer 每次会根据已经编排的触发时间自动重新编排。如果实际的触发时间太过于延迟，甚至是晚了一个或多个周期，那么也只会触发一次，而非连续多次。之后会重新编排下次触发时间。

[`Timer`](https://developer.apple.com/documentation/foundation/timer) 和 [`CFRunLoopTimer`](https://developer.apple.com/documentation/corefoundation/cfrunlooptimer-rhk) 是 [toll-free bridged](https://developer.apple.com/library/archive/documentation/CoreFoundation/Conceptual/CFDesignConcepts/Articles/tollFreeBridgedTypes.html#//apple_ref/doc/uid/TP40010677) 的，设置好时间和回调函数后加到正在运行的 run loop 中即可。

#### Run Loop Observers

不同于 source 在同步或异步事件发生时触发，observer 会在 run loop 运行期间的某些特殊地方触发。这些 run loop 中『特殊』的地方列举如下：

1. 进入 run loop
2. 当 run loop 即将处理一个 timer
3. 当 run loop 即将处理一个 input source
4. 当 run loop 即将休眠
5. 当 run loop 已经被唤醒，但在它处理唤醒它的事件之前
6. 退出 run loop

可以使用 Core Foundation 的 [`CFRunLoopObserver`](https://developer.apple.com/documentation/corefoundation/cfrunloopobserver-ri3) 类创建 run loop observer。`CFRunLoopObserver` 记录了回调函数和关注的事件类型（上面 6 种时间的掩码），它跟 timer 一样可以在创建的时候选择只触发一次或重复触发。

#### Run Loop 事件顺序

线程的 run loop 每次运行都会处理待决的事件，并为绑定的所有 observer 生成通知。次序如下：

1. 通知 observer 已经进入 run loop
2. 通知 observer 有 timer 将要触发
3. 通知 observer 有非基于端口的 input source 将要触发
4. 触发所有已就绪的非基于端口的 input source
5. 如果一个基于端口的 input source 已就绪并等待触发，立即处理事件，并转至**第 9 步**
6. 通知 observer 线程即将休眠
7. 让线程休眠，直到被以下条件唤醒：
   - 有基于端口的 input source 事件到达
   - timer 触发
   - run loop 设定的超时时间到了
   - run loop 被手动唤醒
8. 通知 observer 线程刚刚被唤醒
9. 处理待决事件
   - 如果用户定义的 timer 触发了，处理 timer 事件并重启 run loop，跳回到**第 2 步**
   - 如果 input source 触发了，分发事件
   - 如果 run loop 被唤醒且没有超时，重启 run loop，跳回到**第 2 步**
10. 通知 observer 已经退出 run loop

由于 timer 和 input source 对 observer 的通知是在事件真正发生前就已经发出，所以这之间会有时间间隔。如果对事件时间的掌控很严格，可以使用休眠和唤醒的通知帮你关联实际事件的时机。

由于 timer 和其他周期性事件是在运行 run loop 的时候发送的，**绕过 loop 会打断这些事件的发送**。典型的案例就是在实现鼠标追踪程序中写了个不断从应用请求事件的循环逻辑，按理说应该是让应用正常地分发这些事件，而不是主动抓取。这就导致 timer 被开发者写的循环逻辑阻塞而一直无法触发。

可以使用 run loop 对象将其手动唤醒，其他事件也可能导致 run loop 被唤醒。比如添加另一个非基于端口的 input source 唤醒 run loop，input source 就能立刻被处理，而不是一直等到其他事件发生。

### 该何时使用 Run Loop？

需要手动运行 run loop 的场景只有一个，那就是你创建次级线程的时候。应用主线程的 run loop 是基础设施中至关重要的部分。应用框架会把自动运行主线程 run loop 的程序写好，比如 `UIApplication` 或 `NSApplication` 中的 `run`。如果使用 Xcode 带的模板创建工程，千万不要去调用这些方法。

对于次级线程是否有必要手动开启 run loop，那要看实际情况了。比如使用线程执行一些预先设定好的运行时间较长的任务，可能就不需要开启 run loop 了。Run Loop 是为**『想要与线程更多交互』**的场景准备的，例如：

- 使用 input source 与其他线程通信
- 在线程中使用 timer
- 在 Cocoa 应用中使用任何 `performSelector...` 系列的方法
- 让线程执行周期性任务

如果选择使用 run loop，配置和启动是很简单的。可是对所有的线程编程而言，应该计划好在合适的场景下退出次级线程，这比强行退出要好。

### 使用 Run Loop 对象

Run Loop 对象提供了向 run loop 中添加 input source、timer 和 run-loop observer 的主要接口，并运行起来。**每个线程都关联一个单独的 run loop。**在 Cocoa 中，Run Loop 对象是个 `RunLoop` 类的实例，在 Core Foundation 中是 `CFRunLoop` 指针。但它们不是 toll-free bridge 的。

#### 获取 Run Loop 对象

获取当前线程的 run loop 对象有两种方式：

- Cocoa 框架 `RunLoop` 的类属性 [`current`](https://developer.apple.com/documentation/foundation/runloop/1412291-current)
- `CFRunLoopGetCurrent` 函数

可以从 `RunLoop` 对象的 `getCFRunLoop` 方法获取到 `CFRunLoop`，这样就可以传给 Core Foundation 程序使用了。二者都指向同一个 run loop，所以可以混用。

#### 配置 Run Loop

在次级线程运行 run loop 之前，必须向其添加至少一个 input source 或 timer，否则 run loop 会因没有可监控的 source 而在运行后立刻退出。

除了用 source 外，还可以用 run loop observer 观察 run loop 的各种运行阶段。做法是创建一个 `CFRunLoopObserver` 类型的对象并用 `CFRunLoopAddObserver` 函数将其添加到 run loop 中。注意的是只能用 Core Foundation 创建 run loop observer，Cocoa 框架无能为力。

下面的示例代码在线程入口函数中创建了 run loop observer 并将其添加到 run loop 中。observer 监听了 run loop 所有的活动，并省略了回调函数 `myRunLoopObserver` 的实现。

```swift
@objc func threadMain() {
    autoreleasepool() {
        let myRunLoop = RunLoop.current

        // Create a run loop observer and attach it to the run loop.
        let context = CFRunLoopObserverContext(version: 0, info: self, retain: nil, release: nil, copyDescription: nil)
        if let observer = CFRunLoopObserverCreate(kCFAllocatorDefault, CFRunLoopActivity.allActivities.rawValue, true, 0, &myRunLoopObserver, &context) {
            let cfLoop = myRunLoop.getCFRunLoop()
            CFRunLoopAddObserver(cfLoop, observer, .defaultMode)
        }
        // Create and schedule the timer.
        Timer.scheduledTimer(timeInterval: 0.1, target: self, selector: #selector(doFireTimer), userInfo: nil, repeats: true)
        var loopCount = 10
        while loopCount > 0 {
            myRunLoop.run(until: Date(timeIntervalSinceNow: 1))
            loopCount -= 1
        }
    }
}
```

为了不让 run loop 刚运行就立刻退出，上面的代码向 run loop 添加了一个 timer。因为 timer 一旦触发就无效了，依然会导致 run loop 退出，所以这里 `repeats` 参数传入 `YES`。但这样会让 run loop 一直运行很久，并需要周期性触发 timer 来唤醒线程，这实际上是轮询的另一种形式罢了。相反，输入源等待事件发生，让线程一直处于休眠状态，直到事件发生再唤起。

[`CFRunLoopObserverContext`](https://github.com/apple/swift-corelibs-foundation/blob/05b5a05fa4f3be28eb1fd16203b34286ccc7d541/CoreFoundation/RunLoop.subproj/CFRunLoop.h#L131) 结构体定义如下，查文档可知第二个参数 `info` 会在回调函数被调用时当做参数传入，这里传入 `self`。

```c
typedef struct {
    CFIndex	version;
    void *	info;
    const void *(*retain)(const void *info);
    void	(*release)(const void *info);
    CFStringRef	(*copyDescription)(const void *info);
} CFRunLoopObserverContext;
```

#### 启动 Run Loop

只有在应用的次级线程才需要启动 run loop，而且需要有至少一个 input source 或 timer，否则 run loop 启动后会立刻退出。

启动 run loop 的几种方式包括：

- 无条件：
- 设定时间限制
- 处于特定模式（Mode）

| 方式         | 方法名(NSRunLoop)   | 解释                                                         |
| :----------- | :------------------ | :----------------------------------------------------------- |
| 无条件       | run                 | 最简单但也最不可取的方案。会让线程进入无限循环，对 run loop 很难控制。可以添加和移除 input source 和 timer，但只能通过 kill 的方式停止 run loop。也无法在自定义模式下运行 run loop。 |
| 设定时间限制 | runUntilDate:       | run loop 在收到事件或超时前会一直运行。run loop 结束后可以重启，并处理接下来的事情。比上一种方式更好，提供了时间限制。 |
| 处于特定模式 | runMode:beforeDate: | 相比上一种方式，增加了在特定模式下运行 run loop。            |

`run` 和 `runUntilDate:` 方法会使用 `NSDefaultRunLoopMode` 参数不断调用 `runMode:beforeDate:` 方法。

下面的代码展示了一个线程入口函数的大纲，主要是 run loop 的基本构成。本质上就是配置好 run loop 并运行后，每轮运行后不断检查是否需要退出线程。使用 Core Foundation 可以检查 run loop 每次运行的结果，并决定是否需要退出线程。当然也可以使用上面 `NSRunLoop` 提供的 API，而且无需检查每次运行的返回值。后面会有例子。

```objective-c
- (void)skeletonThreadMain
{
    // Set up an autorelease pool here if not using garbage collection.
    BOOL done = NO;
 
    // Add your sources or timers to the run loop and do any other setup.
 
    do
    {
        // Start the run loop but return after each source is handled.
        SInt32    result = CFRunLoopRunInMode(kCFRunLoopDefaultMode, 10, YES);
 
        // If a source explicitly stopped the run loop, or if there are no
        // sources or timers, go ahead and exit.
        if ((result == kCFRunLoopRunStopped) || (result == kCFRunLoopRunFinished))
            done = YES;
 
        // Check for any other exit conditions here and set the
        // done variable as needed.
    }
    while (!done);
 
    // Clean up code here. Be sure to release any allocated autorelease pools.
}
```

其实上面这段调用 `CFRunLoopRunInMode()` 的逻辑跟 `CFRunLoopRun()` 差不多。

可以递归启动 run loop。也就是说可以在 input source 或 timer 的回调处理函数中调用 `CFRunLoopRun`, `CFRunLoopRunInMode` 或上面提到的 `NSRunLoop` 的三个方法，而且嵌套的 run loop 可以在任意 Mode 下运行。

#### 退出 Run Loop

在 run loop 已经将事件处理之前有两种退出的方式：

1. 给 run loop 配置 timeout 值
2. 告诉 run loop 停止

推荐第一种方法，因为它会让 run loop 完成一切正常的处理，包括在退出前向 observer 发通知。

使用 `CFRunLoopStop` 函数停止 run loop 的结果跟第一种方式差不多，run loop 会把剩下的通知发出去，然后退出。不同点在于可以用这个函数停止以无条件方式（`run` 方法）启动的 run loop。**要注意的是 `CFRunLoopStop` 只会停止对 `CFRunLoopRun` 和 `CFRunLoopRunInMode` 的调用，对于 Cocoa 框架相当于只停止一次 `runMode:beforeDate:` 的调用，而不是退出 run loop。stop 一次运行和 exit 整个 run loop 是不一样的**。

虽然移除 run loop 的 input source 和 timer 也会导致其退出，但**这种方法不可靠**。因为有些系统程序会向 run loop 中添加 input source，开发者根本不知道有这回事，移除的时候就会漏掉，自然就不会导致 run loop 退出。

#### 线程安全和 Run Loop 对象

使用 Core Foundation 中的函数操作 run loop 对象一般都是线程安全的，可以在任何线程调用。如果要更改 run loop 的配置，尽可能在 run loop 自己的线程操作。

Cocoa 中对应的 `NSRunLoop` 内部并不是线程安全的，必须在 run loop 所在的线程修改它。向其他线程的 run loop 添加 input source 或 timer 都会导致 crash 或异常行为。

// TODO: