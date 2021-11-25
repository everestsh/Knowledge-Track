# macOS

记录 macOS 应用与习惯的环境配置。

## 键盘标识

`⇧` Shift `⌃` Control `⌥` Option `⌘` Command `↩` Enter

## 快捷键速览

- 打开"截屏"。按下 Shift-Command-5 以查看屏幕上的控件。

## 前置网络环境配置

- [ClashX](https://github.com/yichengchen/clashX)
- [ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG)

## 开发环境配置

> [Dotfiles 配置仓库](https://github.com/Binlogo/Dotfiles)

- [Oh My Zsh](https://ohmyz.sh/#install)

```sh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

- [Homebrew](../cli/homebrew.md)

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

```sh
export HOMEBREW_NO_AUTO_UPDATE=1
```

- [Git](https://git-scm.com/)

```shell
brew install git
```

- [rbenv](https://github.com/rbenv/rbenv) - ruby 环境管理工具

```sh
brew install rbenv
```

- [CocoaPods](https://cocoapods.org/)

```shell
sudo gem install cocoapods
pod setup
```

## 应用

记录日常使用与喜爱的 macOS 应用

- [iTerm](https://www.iterm2.com/) - 更强大易用的命令行终端

  - with [oh-my-zsh](http://ohmyz.sh/)

- [Fork](https://git-fork.com/) - Git 管理应用，简洁、易用

- [Dash](https://kapeli.com/dash) - 开发文档阅读

- [VSCode](https://code.visualstudio.com/) - 编辑器，内部直接使用命令行非常便捷

- [Typora](https://typora.io/) - markdown 编辑器，简洁、易用，所见即所得

- [滴答清单](https://guide.dida365.com/) - 一站式行动： GTD、项目流程、进度追踪、番茄专注、习惯打卡

- [The Unarchiver](https://theunarchiver.com/) - 解压缩

- [Alfred](./alfred.md) - 快捷打开任意应用或工作流

- [Excalidraw](https://excalidraw.com/) - 手绘风格的画图应用，适合流程图、架构图绘制与展示

- [Contexts](./contexts.md) - 快速窗口切换

- [SnippetsLab](./snippetsLab.md) - 代码片段库，支持 iCloud 备份 + [GitHub Gist](https://gist.github.com/Binlogo) 同步

- [Cleaner for Xcode](https://apps.apple.com/cn/app/cleaner-for-xcode/id1296084683?mt=12) - [开源](https://github.com/waylybaye/XcodeCleaner-SwiftUI)的 Xcode 清理应用

- [Kap](https://getkap.co/)

- [IINA](https://iina.io/)

### 其他

- [Raycast](https://www.raycast.com/features) - 类似 Alfred 的效率工具，应用启动&工作流扩展集成
- [Notable.md](https://notable.md/) - 另一个简洁易用的 markdown 编辑器，旧有版本开源(TypeScript)。
- [Kaleidoscope](https://kaleidoscope.app/) - Diff 差异检查应用，文本、图片、文件夹、开发流集成
- [BetterTouchTool](https://folivora.ai/) - 自定义触控板手势
- [Downie](https://software.charliemonroe.net/downie/) - Youtube、Bilibili 等流媒体视频下载

## Q&A 速查

- 如何快速清理 Time Machine 备份

  [清理 Time Machine 备份脚本](https://gist.github.com/Binlogo/6d309300e7d9afca91c93ff6d8fa453d)

- MacBook Pro 2018 款连接iPhone或iPad设备频繁连续断开问题

  ```sh
  sudo killall -STOP -c usbd
  ```

  会引发新问题：连接的设备会显示「不在充电」（也是解决问题的原因）

- 系统迁移之前，如何删除所有无用`node_modules`等目录

  ```sh
  find . -name "node_modules" -type d -prune -exec rm -rf '{}' +
  ```

- 加速 Chrome 浏览器内置下载速度
  
 - 地址栏输入并回车：`chrome://flags/#enable-parallel-downloading`, 将 Default 改为 Enabled 即可

## 链接

- [Apple Teacher Learning Center](https://appleteacher.apple.com/#/home/resources)

  - TODO: 🎖 Go get it, get it.

- [Why Is Apple’s M1 Chip So Fast?](https://debugger.medium.com/why-is-apples-m1-chip-so-fast-3262b158cba2)
  <details>
    <summary>论述纪要</summary>

    1. M1 并不是传统意义上的 CPU，而是 SoC（System on a chip）

    2. Apple 区别于其他添加通用核心的思路，在芯片中添加更多专用核心

    3. 通用内存架构(UMA)的特别之处，区别与以往的「集成显存」
        -  非CPU/GPU分区使用策略，真正共用内存，避免拷贝
        - 无须在不同类型内存中进行连接通讯，数据读写吞吐更大，速度更快
        - 基于 ARM 架构和更高密度的工艺，GPU 功率足够小，集成至 SoC 中，不会因发热量对芯片产生影响
        - 副作用：无法扩展内存，解决办法：加速与 SSD 交换内存的传输速度

    4. SoC 这么好，为什么 Intel/AMD 不复制苹果的策略？
        - 题外话：「生产方式决定生产力」
        - SoC 相当于是整个系统，更倾向于是 Dell/HP 这样的整机生产厂商去做
        - Intel/AMD 是传统 CPU 生产厂商，为整机提供零部件
        - 苹果对软硬件/上下游的掌握便发挥出巨大优势
        
  </details>

  - [M1 Pro and M1 Max Xcode Build and Test Benchmarks](https://blog.swiftpackageindex.com/posts/m1-pro-and-m1-max-build-and-test-benchmarks)

- [Back to the Mac](https://backtomac.org/) - Mac 开发者在线会议
  - 2021
    - [x] The making of VimR
    - [x] Automating Your Mac
    - [ ] Shipping your app without the App Store
  - 2020
    - [ ] Building a screenshot search app
    - [ ] Using XPC to execute Python code in your Mac app
    - [ ] Notarization and your CI system
    - [ ] Build a Mac app inside 30 minutes using nothing but SwiftUI
