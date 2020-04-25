# iOS 开发

精进 + 创造中 🚀

- [《iOS 开发高手课》笔记](https://mubu.com/doc/5Iio_eHpUPE)
- [图片储存与展示性能优化](https://mubu.com/doc/fPEZGSYGr0)
- [设计模式 - Swift 示例](https://github.com/Binlogo/Design-Patterns-In-Swift-CN)
- [多线程编程](./threading-programming/threading-programming.md)
  - [线程管理](./threading-programming/thread-management.md)
  - [Run Loops](./threading-programming/run-loops.md)
  - [线程同步](./threading-programming/synchronization.md)

## 性能优化

- [WWDC - Measuring Performance Using Logging](https://developer.apple.com/videos/play/wwdc2018/405)

##  特性

### iOS 13

- [WWDC - Modernizing Your UI for iOS 13](https://developer.apple.com/videos/play/wwdc2019/224/)

- [Context Menus](https://developer.apple.com/design/human-interface-guidelines/ios/controls/context-menus/)

  - [Context Menus Tutorial for iOS: Getting Started](https://www.raywenderlich.com/6328155-context-menus-tutorial-for-ios-getting-started)

## 链接

- [iOS Playbook from Babylon](https://github.com/babylonhealth/ios-playbook) - [Babylon 团队](http://github.com/babylonhealth) 的 iOS 攻略手册
- [Timelane](https://github.com/icanzilb/TimelaneCore) - 异步代码性能可视化调试，支持 RxSwift/Combine
- [蜂鸟](https://github.com/onevcat/FengNiao) - 清理无用图片资源
- [解密 Runloop](http://mrpeak.cn/blog/ios-runloop/) - 有助于理解 RunLoop 的一篇文章

```objective-c
while (alive) {
  performTask() //执行任务
  callout_to_observer() //通知外部
  sleep() //休眠
}
```

![RunLoop](http://mrpeak.cn/images/rl00.png)

- [Tiercel](https://github.com/Danie1s/Tiercel) - 简单易用、功能丰富的纯 Swift 下载框架
- [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch) - GCD 官方源码
