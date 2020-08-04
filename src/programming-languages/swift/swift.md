# Swift

- [标准库的源码](https://github.com/apple/swift/tree/master/stdlib/public/core)
- [Swift进阶](./advanced-swift.md)
- [Swift Style Guide within Google](https://google.github.io/swift/)
- [SwiftDoc.org](https://swiftdoc.org/) - Swift 开源文档查询
- [Swift-MemoryLayout](https://github.com/TannerJin/Swift-MemoryLayout) - Swift 内存布局

## Apple 文档

- [Preventing Timing Problems When Using Closures](https://developer.apple.com/documentation/swift/preventing_timing_problems_when_using_closures) - 使用闭包时防止时序问题

  <details>
    <summary>笔记概览</summary>

    - 了解同步和异步调用的结果（`@escaping` ?)
    - 不要在多次调用的闭包中编写进行一次性更改的代码（e.g: `FileHandle.close`)
    - 不要将关键代码置于可能不被调用的闭包中

  </details>

- [Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)

  <details>
    <summary>笔记概览</summary>

    - Use structures by default.
    - Use classes when you need Objective-C interoperability.
    - Use classes when you need to control the identity of the data you're modeling.
    - Use structures along with protocols to adopt behavior by sharing implementations.

  </details>

## 链接

- [What's new in Swift?](https://www.whatsnewinswift.com/)
- [Swift Concurrency by Uber](https://github.com/uber/swift-concurrency) - Uber 开源的并发编程实用工具
