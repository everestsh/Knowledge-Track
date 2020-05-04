## 面试题总结 - 2020

面试中，最主要先理解考察意图，再有的放矢针对性作答。

### 1. Swift 的主要特性以及优缺点

看起来是基础问题，实际考察对 Swift 的掌握以及其他编程语言理解的程度。

> 关键点：静态类型、协议、引用类型&值类型、可选型、泛型

Swift 是一门**静态类型**的编译语言，注重类型安全，这是核心特点，其他的特性和优缺点也围绕这点展开。

优点：

* 「协议」、「泛型」、「可选型」、「元组」、「闭包」等多种特性使其更好地应用多种编程范式
  * 面向协议、函数式、面向对象
* 强大简洁的枚举和类型匹配以及集合类型
* 编译时警告与错误易于及早发现问题，保障运行时安全
* 注重值类型的运用，
  * struct 支持方法、扩展、协议等，
  * 运用「须时拷贝」，兼顾执行效率与多线程环境下的安全
* 注重方法的静态派发，相较于动态派发，执行效率更快
* 文件结构简单，且拥有诸多易用的语法糖，编码效率和可读性高
  * `do`、`guard`、`defer`、`repeat `等关键字支持更灵活的控制流

缺点：

* 以往版本不够稳定（Swift 5 之后 ABI 稳定）
* 兼容 Objc 需要桥接

-----

更多资料：

* [About Swift](https://swift.org/about/)

### 2. 如何理解 iOS 应用？是什么样的架构？如何实现的？

不管怎么问，乍一听会有点懵，实际考察对 iOS 应用整体架构的理解，包括核心框架以及应用生命周期等。

> 关键点：Run Loop、交互、推送通知、声明周期事件、[UIApplication](https://developer.apple.com/documentation/uikit/uiapplication)、[UIAppDelegate](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)/[UISceneDelegate](https://developer.apple.com/documentation/uikit/uiscenedelegate)

一个应用程序是特定需求集合的体现。在 iOS 系统中，一个典型应用程序本身是大的 Run Loop，支持手势交互、系统事件包括推送通知、来电、横竖屏等，并且通过 UIKit 中的 UIApplication、AppDelegate 提供抽象。剩下的工作主要是在合适的应用回调时机根据特定需求执行特性代码。

---

更多资料：

- [UIApplicationDelegate (developer.apple.com)](https://developer.apple.com/documentation/uikit/uiapplicationdelegate)
- [Responding to the Launch of Your App (developer.apple.com)](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app)
- [UISceneDelegate (developer.apple.com)](https://developer.apple.com/documentation/uikit/uiscenedelegate)
- [Preparing Your UI to Run in the Foreground (developer.apple.com)](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_foreground)
- [Preparing Your UI to Run in the Background (developer.apple.com)](https://developer.apple.com/documentation/uikit/app_and_environment/scenes/preparing_your_ui_to_run_in_the_background)

### 3. iOS 开发中内存管理机制是怎样的？

内存管理在任何程序开发中都至关重要，尤其在 iOS 设备内存硬件有限的前提下。 

> 关键点：ARC/MRC、引用类型、值类型、循环引用

采用 ARC，引用计数机制进行内存管理。引用类型实例被分配或取消分配给变量或属性时进行计数，当强引用计数为 0 时，释放内存。值类型不会被引用计数，其在栈内存上，且「须时拷贝」。不特别指定，默认的引用都是强引用，使用闭包时由于其上下文捕获的特性，需要特别注意避免循环引用导致的内存泄露问题。

---

更多资料：

- [Automatic Reference Counting (docs.swift.org)](https://docs.swift.org/swift-book/LanguageGuide/AutomaticReferenceCounting.html)

### 4. 什么是 MVC？

// TODO

### 5. 怎么理解单例模式？简单说说应用场景

### 6. delegate 和 KVO 有什么区别？

### 7. iOS 应用开发中常用的设计模式有哪些？

### 8. 还知道哪些设计模式？

### 9. 解释下什么是 SOLID 原则？举例说明

### 10. 有哪些数据存储或持久化方式？分别有什么特点

### 11. 有哪些 HTTP 网络实现方式？

### 12. 如何序列化/反序列化网络数据？

### 13. 有哪些 UI 布局方式？

### 14. 如何优化 UITableView 或 UICollectionView 的滚动性能？

### 15. 如何执行多线程任务？

### 16. 如何管理依赖？

### 17. 开发中如何调试或优化代码？

### 18.  如何单元测试或 UI 测试？

### 19. mock、stubs 和 fake 有什么区别？

### 20. 什么是函数响应式编程？

### 21. 常用的 iOS 架构有哪些？



---

原文：[iOS Interview Questions for Senior Developers in 2020](https://iosinterviewguide.com/ios-interview-questions-for-senior-developers-in-2020)





