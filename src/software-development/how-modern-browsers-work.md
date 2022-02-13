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

## 参考文献

Google Updates 的一系列介绍 Chrome 架构的文章，了解现代浏览器如何工作。
- [x] bit.ly/browsers-pt1  
- [x] bit.ly/browsers-pt2  
- [ ] bit.ly/browsers-pt3  
- [ ] bit.ly/browsers-pt4
以及：
- [ ] https://browser.engineering
