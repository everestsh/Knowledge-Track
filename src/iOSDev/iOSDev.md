# iOS 开发

精进 + 创造中 🚀

- [WWDC20 观影记录](https://github.com/Binlogo/WWDC20-Track)
- [《iOS 开发高手课》笔记](https://mubu.com/doc/5Iio_eHpUPE)
- [图片储存与展示性能优化](https://mubu.com/doc/fPEZGSYGr0)
- [设计模式 - Swift 示例](https://github.com/Binlogo/Design-Patterns-In-Swift-CN)
- [多线程编程](./threading-programming/threading-programming.md)

  - [线程管理](./threading-programming/thread-management.md)
  - [Run Loops](./threading-programming/run-loops.md)
  - [线程同步](./threading-programming/synchronization.md)

## 性能优化

- [WWDC - Measuring Performance Using Logging](https://developer.apple.com/videos/play/wwdc2018/405)

- [Explore UI animation hitches and the render loop - Tech Talks - Videos - Apple Developer](https://developer.apple.com/videos/play/tech-talks/10855/)
- [Find and fix hitches in the commit phase - Tech Talks - Videos - Apple Developer](https://developer.apple.com/videos/play/tech-talks/10856)
- [Demystify and eliminate hitches in the render phase - Tech Talks - Videos - Apple Developer](https://developer.apple.com/videos/play/tech-talks/10857)

##  特性

### iOS 13

- [WWDC - Modernizing Your UI for iOS 13](https://developer.apple.com/videos/play/wwdc2019/224/)

- [Context Menus](https://developer.apple.com/design/human-interface-guidelines/ios/controls/context-menus/)

  - [Context Menus Tutorial for iOS: Getting Started](https://www.raywenderlich.com/6328155-context-menus-tutorial-for-ios-getting-started)

### Catalyst

- [terhechte/CatalystMaterial](https://github.com/terhechte/CatalystMaterial)
- [The Missing Guide for Mac Catalyst Apps](https://www.craft.do/maccatalyst-guide) - Craft 的 Catalyst 应用开发指南

## 链接

- [iOS Handbook from Infinum](https://infinum.com/handbook/books/ios)
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

- [Tiercel](https://github.com/Danie1s/Tiercel) - 简单易用、功能丰富的纯 Swift 下载框架
- [swift-corelibs-libdispatch](https://github.com/apple/swift-corelibs-libdispatch) - GCD 官方源码
- [Objective-C Runtime 1小时入门教程](https://www.ianisme.com/ios/2019.html)
- [SwiftInfo](https://github.com/rockbruno/SwiftInfo) - 抓取分析 iOS 应用信息以便追踪质量
- [Kernal Programming Guide](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/KernelProgramming/About/About.html#//apple_ref/doc/uid/TP30000905-CH204-TPXREF101)
- [Creating and Distributing an iOS Binary Framework](https://instabug.com/blog/ios-binary-framework/)
- [Optimize your iOS projects creating binaries Frameworks](https://medium.com/@cristianarielbarril/optimize-your-ios-projects-creating-binaries-frameworks-f83cb848f59f)
- [LinkMap解析工具](https://github.com/huanxsd/LinkMap) - 检查每个类占用大小
- [faceboook/Chisel](https://github.com/facebook/chisel) - LLDB 命令工具集
- [Xcode Project Renamer](https://github.com/appculture/xcode-project-renamer) - Xcode 项目重命名脚本
- [What is NSUserDefaults?](http://dscoder.com/defaults.html) - 深入理解 NSUserDefault（由 Cocoa 开发者撰写）
- [peripheryapp/periphery: A tool to identify unused code in Swift projects.](https://github.com/peripheryapp/periphery)
- [Code Signing Guide for Teams](https://codesigning.guide/) - 代码签名团队指南
- [libimobiledevice](https://libimobiledevice.org/) - iOS 设备调试命令行工具集

### 库

- [Flinesoft/BartyCrouch](https://github.com/Flinesoft/BartyCrouch) - 增量更新的文案国际化方式
- [SwiftyBeaver](https://github.com/SwiftyBeaver/SwiftyBeaver) - Swift 编写的日志库
- [SPPermissions](https://github.com/ivanvorobei/SPPermissions) - 友好的权限获取控件
- [NSLogger](https://github.com/fpillet/NSLogger) - 高性能日志库
- [Starscream](https://github.com/daltoniam/starscream) - Swift 编写的 Websocket 库

## Q&A

- 如何加速 CocoaPods 安装？

  [CDN as Default](http://blog.cocoapods.org/CocoaPods-1.8.0-beta/)

  ```ruby
  - source 'https://github.com/CocoaPods/Specs.git'
  + source 'https://cdn.cocoapods.org/'
  ```

- 如何解决 Xcode 不支持最新 iOS 系统真机设备调试？

  下载 [iPhoneOSDeviceSupport](https://github.com/filsv/iPhoneOSDeviceSupport) 对应版本支持文件，解压拷贝到以下路径：
  
  `/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/`