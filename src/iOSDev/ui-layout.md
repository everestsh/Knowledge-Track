# 视图布局

掌握视图布局方式，并理解背后的原理。

## 链接

- [WWDC 220 Session High Performance Auto Layout](https://developer.apple.com/videos/play/wwdc2018/220)
- [The Cassowary Linear Arithmetic Constraint Solving Algorithm](https://constraints.cs.washington.edu/solvers/cassowary-tochi.pdf)
- [从 Auto Layout 的布局算法谈性能](https://draveness.me/layout-performance/)
- [Facebook - Flexible Layouts with Yoga](https://yogalayout.com/) - 跨平台 Flexbox 布局（C++）
- [Visly - Stretch](https://vislyhq.github.io/stretch/) - 跨平台 Flexbox 布局（rust）
- [LinkedIn - LayoutKit](https://github.com/linkedin/LayoutKit) - 快速视图布局库，声明式，写法类似 Flexbox（但不是，基于原生 frame）
- [TextureGroup - Texture](https://github.com/TextureGroup/Texture) - 异步布局库

### 视图组件库

- [QMUI iOS](https://github.com/QMUI/QMUI_iOS/) - 腾讯开源的“致力于提高项目 UI 开发效率的解决方案”

## Q&A

1. Layout feedback loop 导致 OOM 问题的排查与解决？ 
  - [Debugging Out of Memory Issues: Catching Layout Feedback Loop with the Runtime Magic](https://www.appcoda.com/layout-feedback-loop/)
    - [代码](https://github.com/rsrbk/LayoutLoopHunter)
    <details>
      <summary>笔记概览</summary>
      OOM 常见原因：

      - retain cycles;
      - race conditions;
      - abandoned threads;
      - deadlocks;
      - layout feedback loop.

      一般解决办法：
      
      - Allocations and Leaks instruments for resolving retain cycles and other types of leaks
      - Memory Debugger which has been introduced in Xcode 8 and replaces some functionality from Allocations and Leaks instruments
      - Thread Sanitizers help you find race conditions, abandoned threads or deadlocks

    </details>
  - [Debugging Auto Layout feedback loops](https://www.hackingwithswift.com/articles/59/debugging-auto-layout-feedback-loops)
    <details>
      <summary>笔记概览</summary>

      1. Go to the Product menu, hold down Option, and choose “Run…”
      2. Select the Arguments tab, and click + to add a new entry
      3. For iOS apps enter `-UIViewLayoutFeedbackLoopDebuggingThreshold 100`
      4. For macOS apps enter `-NSViewLayoutFeedbackLoopDebuggingThreshold 100`

    </details>

