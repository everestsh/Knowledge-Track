# 性能优化

> 极致，永无止境

## Apple WWDC & 文档
- [WWDC - Measuring Performance Using Logging](https://developer.apple.com/videos/play/wwdc2018/405)

- [Explore UI animation hitches and the render loop - Tech Talks - Videos - Apple Developer](https://developer.apple.com/videos/play/tech-talks/10855/)
- [Find and fix hitches in the commit phase - Tech Talks - Videos - Apple Developer](https://developer.apple.com/videos/play/tech-talks/10856)
- [Demystify and eliminate hitches in the render phase - Tech Talks - Videos - Apple Developer](https://developer.apple.com/videos/play/tech-talks/10857)
- [Reducing Your App’s Size](https://developer.apple.com/documentation/xcode/reducing-your-app-s-size)

### 内存

- [Detect and diagnose memory issues - WWDC21](https://developer.apple.com/videos/play/wwdc2021/10180/)
    - Perfermance XCTests collections
        - Ktrace files
        - Memory graph
    - MetricKit & Xcode Orgernizer
    - Issues to look
        - Leaks
        - Heap size issues
            * Heap allocation regression
                - `vmmap -summary ./PathToXXXXX.memgraph`
                - `heap -diffFrom=PathToXXXPre.memgraph PathToXXXPost.memgraph`
                - `heap -addresses=non-object[500k-] PathToXXX.memgraph`
                - `leaks --trackTree=0x1138XXXX PathToXXX.memgraph`
                - `leaks --referenceTree --groupByType PathToXXX.memgraph`
                - `malloc_history -fullStacks PathToXXX.memgraph 0x1138XXXX`
            * Fragmentation
                - Allocate object with similar lifetimes close to each other
                - Aim for 25% fragmentation or less
                - Use autorelease pools
                - Pay extra attention to long running processes
                - Use allocations track in Instruments
                    - ref: Getting Started with Instruments

- [Getting Started with Instruments - WWDC19](https://developer.apple.com/videos/play/wwdc2019/411)
- [iOS Memory Deep Dive - WWDC18](https://developer.apple.com/videos/play/wwdc2018/416)

### 包大小

- [ImageOptim](https://imageoptim.com/howto.html) - 图片资源大小、加载优化工具
- [WBBlades](https://github.com/wuba/WBBlades) - 基于 Mach-O文件 解析的工具集，包括无用代码检测、包大小分析等

## 书籍

- 《高性能 iOS 应用开发》- 各方面的性能衡量到工程实践
    > 注：看完本书不久，其中一位译者成为了我的 leader 😄

## Instrument 百宝箱

- [Instruments Help](https://help.apple.com/instruments/mac/current/)
- [Xcode Organizer](https://developer.apple.com/videos/play/wwdc2020/10076)

## 链接

- [x] [iOS 高刷屏监控 + 优化：从理论到实践全面解析](https://mp.weixin.qq.com/s/gMxTq0_nmE-xW7GA3pkBJg) 
	- 官方文档参考：[Optimizing ProMotion Refresh Rates for iPhone 13 Pro and iPad Pro](https://developer.apple.com/documentation/quartzcore/optimizing_promotion_refresh_rates_for_iphone_13_pro_and_ipad_pro))
	- **帧率**: 屏幕内容的变化频率
		- 刷新帧率，决定上限
		- 渲染帧率，决定下限
	- **动态刷新率**：ProMotion 本质上是对 [Adaptive-Sync](https://en.wikipedia.org/wiki/Variable_refresh_rate) 显示标准的一种实现
		- The iPhone 13 Pro and iPhone 13 Pro Max ProMotion displays can present content on the display using the following refresh rates and timings: `120Hz (8ms), 80Hz (12ms), 60Hz (16ms), 48Hz (20ms), 40Hz (25ms), 30Hz (33ms), 24Hz (41ms), 20Hz (50ms), 16Hz (62ms), 15Hz (66ms), 12Hz (83ms), 10Hz (100ms)`
		- The iPad Pro’s ProMotion display can present content on the display using the following refresh rates and timings: `120Hz (8ms), 60Hz (16ms), 40Hz (25ms), 30Hz (33ms), 24Hz (41ms)`
	- 关键 API：
		- `CADisableMinimumFrameDurationOnPhone`
		- `preferredFrameRateRange`
	- 测试指标：
		- 1.  `CADisplayLink` 计算帧率
		- 2.  Xcode GPU Report 帧率：Xcode -> Show Debug Navigator -> FPS 中显示的帧率
		- 3.  Instruments Core Animation FPS
		- 4.  Instruments Display/VSync 信号频率
			- Display：指对应显示器的单个 Surface 上屏持续的时间，对应 CPU-GPU 管线的**渲染频率**
			- VSync：指垂直同步信号时间戳，对应屏幕硬件的**刷新频率**
	- **双缓冲刷新机制** v.s **三缓冲刷新机制**
	-  `display_timer_callback` 逻辑的变化
	- iOS 15 上 Apple 改变了在 ProMotion 设备的渲染事件循环的驱动方式，CoreAnimation 的事务提交不再由完全由 RunLoop 驱动，而是涉及了多个信号源
	- 默认的 CADisplayLink 的回调频率与实际帧率并不匹配，之前基于 CADisplayLink 进行帧率监控的方案在 ProMotion 设备上变得不再可行
	- 动态帧率的应用场景：
		- 监控动态帧率下的流畅度表现：通过在 CADisplayLink 回调中确认 `duration` 参数，计算得到当前屏幕的实时刷新率，并修改 `preferredFrameRateRange` 来进行跟踪。
			- `Hitch Time Ratio` 比单纯的 FPS 更能适配不同刷新率的场景。
		- 关键场景提升帧率
			- 滑动中稳定 120Hz：可以用 CADisplayLink 来实现，`preferredFramesPerSecond`
			- CAAnimation 设置动态帧率, `CAAnimation.preferredFrameRateRange`
			- 手势/转场等其他场景解锁 120Hz, 启用一个解锁了频率的 CADisplayLink
			- 

- [移动芯片性能排行榜](https://www.socpk.com/)

![移动芯片综合天梯图](./assets/移动芯片综合天梯图.webp)