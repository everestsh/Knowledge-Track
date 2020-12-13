# Xcode

> 玩好这把屠龙刀

## 快捷键

- `⌘ + ⇧ + o` 快速搜索打开文件

- `⌃ + 4/5/6` 快速定位工程/文件/方法（输入关键字过滤）

- `⌘ + ⇧ + j` 快速在目录树跳转定位至当前文件

- `⌘ + ⇧ + a` 快速展示代码操作选项

- `⌘ + ⌃ + e` 快速切换至全局编辑当前选中

- `⌘ + ⌥ + ←/→` 快速折叠/展开当前区块代码

- `⌘ + ⌥ + p` 快速刷新画布（SwiftUI 等预览）

- `⌘ + \` 快速在当前行添加/移除调试断点


- `⌘ + ;/:` 快速检查拼写和语法/展示改正建议


## 技巧

- 通过 [xcode-install](https://github.com/xcpretty/xcode-install) 安装管理 Xcode
    ```sh
    gem install xcode-install
    ```
- 为 Class 快速生成初始化方法
    
    > 选中类名 > Editor (或直接右键) > Refactor > Generate Memberwise Initializer

- 运行时切换系统主题样式、文本字号、可访问性等属性
    ![Xcode-Environment Overrides](./assets/xcode_environment_overrides.png)

- 双击 `{` `}` `"` 等，可快速选中区块间代码或字符串

- 检查拼写与语法
    > Format > Spelling and Grammar > Check Spelling While Typing

- Xcode 与模拟器已经支持左右分屏

- 自动补全提示窗口大小支持拉伸
    
## 链接

- [安裝 Xcode 的正確姿勢](https://www.notion.so/Xcode-dfbe2d934ff84b2d84e34ffceef56fe0)
- [Xcode 快速鍵與技巧](https://www.notion.so/ff93434e1b954702a8e552014f119a6b?v=4d88ab84fe8a4551b26d5a1ac13b213b)
- [Improve build times in Swift with Xcode](https://tomasznazarenko.com/improve-build-times-in-swift-with-xcode/) - 优化编译速度
    <details>
        <summary>编译参数备忘</summary>

    - -Xfrontend -warn-long-expression-type-checking=400 (apple/swift GitHub)
    - -Xfrontend -warn-long-function-bodies=400 (apple/swift GitHub)
    - -Xfrontend -debug-time-function-bodies
    </details>