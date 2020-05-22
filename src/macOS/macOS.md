# macOS

记录 macOS 应用与习惯的环境配置。

## 键盘标识

`⇧` Shift `⌃` Control `⌥` Option `⌘` Command `↩` Enter

## 快捷键速览

- 打开"截屏"。按下 Shift-Command-5 以查看屏幕上的控件。

## 开发环境配置

- [Homebrew](https://brew.sh/)

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

- [Git](https://git-scm.com/)

```shell
brew install git
```

- [CocoaPods](https://cocoapods.org/)

```shell
sudo gem install cocoapods
pod setup
```

## Q&A 速查

- MacBook Pro 2018 款连接iPhone或iPad设备频繁连续断开问题

  ```sh
  sudo killall -STOP -c usbd
  ```

  会引发新问题：连接的设备会显示「不在充电」（也是解决问题的原因）

## 链接

- [Apple Teacher Learning Center](https://appleteacher.apple.com/#/home/resources)

  - TODO: 🎖 Go get it, get it.
