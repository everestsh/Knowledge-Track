# 现代浏览器是如何工作的？

- 多进程架构设计
	- 浏览器进程：
		- UI 线程
		- 网络线程
		- 存储线程
	- 渲染进程
- 导航流程
	- 处理输入
	- 建立连接，网络请求
	- 读取响应数据，检查校验
	- 创建或查找一个渲染进程
	- 进程间通讯，提交渲染请求
	- 渲染进程完成回调
- 渲染流程
	- Parsing，解析
		- 将 HTML 文本数据解析为 DOM（Document Object Model）
			- 由 [HTML 协议](https://whatwg-cn.github.io/html/)定义
		- 加载依赖资源：如 `<img>`、`<link>` 等标签
		- JavaScript 可以阻塞解析
			- [x] [解析流程](https://html.spec.whatwg.org/multipage/parsing.html#overview-of-the-parsing-model)
			- [ ] [V8 团队的分享](https://mathiasbynens.be/notes/shapes-ics)
	- 指定浏览器如何加载资源
		- `async` 或 `defer`
		- `<link rel="preload">`
	- CSS style 计算
		- 即使没有 CSS 指定，也会使用 Chrome [默认风格样式](https://source.chromium.org/chromium/chromium/src/+/main:third_party/blink/renderer/core/html/resources/html.css)
	- 布局
	- 绘制
	- Compositing

## 参考文献

Google Updates 的一系列介绍 Chrome 架构的文章，了解现代浏览器如何工作。
- [x] bit.ly/browsers-pt1  
- [x] bit.ly/browsers-pt2  
- [x] bit.ly/browsers-pt3  
- [ ] bit.ly/browsers-pt4
以及：
- [ ] https://browser.engineering

### 性能优化
- [为什么速度很重要？](https://web.dev/why-speed-matters/)
- [快速加载](https://web.dev/fast/#prioritize-resources)