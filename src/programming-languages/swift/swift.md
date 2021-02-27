# Swift

- [标准库的源码](https://github.com/apple/swift/tree/master/stdlib/public/core)
- [Swift进阶](./advanced-swift.md)
- [Swift 方法派发纪要](./programming-languages/swift/swift-method-dispatch-notes.md)
- [Swift Style Guide within Google](https://google.github.io/swift/)
- [SwiftDoc.org](https://swiftdoc.org/) - Swift 开源文档查询
- [Swift-MemoryLayout](https://github.com/TannerJin/Swift-MemoryLayout) - Swift 内存布局
- [Swift: Attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html)
  <details>
    <summary>笔记概览</summary>
    
    - Declaration Attributes
      - available
      - discardableResult
      - dynamicCallable
      - dynamicMemberLookup
      - frozen
      - GKInspectable
      - inlinable
      - main
      - nonobjc
      - NSApplicationMain
      - NSCopying
      - NSManaged
      - objc
      - objcMembers
      - propertyWrapper
      - requires_stored_property_inits
      - testable
      - UIApplicationMain
      - usableFromInline
      - warn_unqualified_access
      - Declaration Attributes Used by Interface Builder
    - Type Attributes
      - autoclosure
      - convention
      - escaping
    - Switch Case Attributes
      - unknown
  </details>

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

- [Adopting Common Protocols](https://developer.apple.com/documentation/swift/adopting_common_protocols) - 采用通用协议

  <details>
    <summary>笔记概览</summary>

    - Conform Automatically to Equatable and Hashable
    - Conform Manually to Equatable and Hashable
      - Use All Significant Properties for Equatable and Hashable
    - Customize NSObject Subclass Behavior
        > If you override one of these declarations, you must also override the other to maintain that guarantee.
    
    > ⚠️ Important
    > 
    > Always use the same properties in both your == and hash(into:) methods. 
    > Using different groups of properties in the two methods can lead to unexpected behavior or performance when using your custom type in sets and dictionaries.

  </details>

- [Imported C and Objective-C APIs](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis) - Swift 与 C/Objective-C 的交互

## 链接

- [What's new in Swift?](https://www.whatsnewinswift.com/)
- [Swift Concurrency by Uber](https://github.com/uber/swift-concurrency) - Uber 开源的并发编程实用工具
- [Burritos](https://github.com/guillermomuntaner/Burritos) - Swift Property Wrappers 特性应用合集
- [Swift Evolution](https://apple.github.io/swift-evolution/) - Swift 演进路径

### 响应式编程

- [DeclarativeHub/ReactiveKit](https://github.com/DeclarativeHub/ReactiveKit)
- [DeclarativeHub/Bond](https://github.com/DeclarativeHub/Bond)

### 设计模式

- [Swift 常用设计模式](https://refactoringguru.cn/design-patterns/swift)

## Q&A

- 踩坑：iOS10 Swift KVO 的 Crash 问题如何解决与避免
  - TODO: 待补充