# Xcode

> 玩好这把屠龙刀

## 技巧

- 通过 [xcode-install](https://github.com/xcpretty/xcode-install) 安装管理 Xcode
    ```sh
    gem install xcode-install
    ```

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