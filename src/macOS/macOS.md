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