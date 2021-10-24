# Rust

> 性能、安全、抽象易用，向编译器学习（di tou🙈）。

## 工具集

- [Rustup](https://rust-lang.github.io/rustup/index.html)
  - 自动补全安装指南：`rustup help completions`
- VSCode
  - 扩展插件：
    - rust-analyzer
    - rust syntax
    - crates
    - better toml
    - Tabnine
    - Error Lens
  - 配置选项：
    - 括号对着色
    - 括号对指南
- [Rust Search Extension](https://rust.extension.sh/) - 浏览器搜索扩展插件

- [cargo-xtask](https://github.com/matklad/cargo-xtask/) - （rust-analyzer [在用](https://rust-analyzer.github.io/rust-analyzer/xtask/index.html)的）构建辅助工具
- [JSON to Rust Serde](https://transform.tools/json-to-rust-serde) - 快速通过 JSON 生成 Rust Serde 代码
- [Rust Cheat Sheet](https://cheats.rs/) - [PDF](https://cheats.rs/rust_cheat_sheet.pdf) - 速查表

## 链接

- [PingCAP - Talent Plan](https://github.com/pingcap/talent-plan) - PingCAP 的 Rust 学习资源
- [Ferrous Teaching Material](https://github.com/ferrous-systems/teaching-material) - Rust 教学资源，非常全面
- [Book - The Rust Programming Language](https://doc.rust-lang.org/book/)
- [Rust Platform Support](https://forge.rust-lang.org/release/platform-support.html)
- [Closures: Magic Functions](https://rustyyato.github.io/rust/syntactic/sugar/2019/01/17/Closures-Magic-Functions.html)
- [Rust 编程语言](https://learnku.com/docs/rust-lang/2018) - 官方的《The Rust Programming Language》翻译
- [Rust异步浅谈](https://leaxoy.github.io/2020/03/rust-async-runtime/)
- [Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/) - Rust 优秀实践示例集锦
- [The Rust Performance Book](https://nnethercote.github.io/perf-book/) - Rust 性能之书
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/)

### 实践经验分享

- [What is Rust and why is it so popular?](https://stackoverflow.blog/2020/01/20/what-is-rust-and-why-is-it-so-popular/)
- [Rust: A Language for the Next 40 Years - Carol Nichols](https://www.youtube.com/watch?v=A3AdN7U24iU)
- [3K, 60fps, 130ms: achieving it with Rust - by Tonarino](https://blog.tonari.no/why-we-love-rust) - [中文](https://mp.weixin.qq.com/s/-YfdohRpJxURNYLSfo6G7w)
- [Understanding Rust as a C++ developer](https://renoth.medium.com/understanding-rust-as-a-c-developer-69ee8ca76fd6)

### Rust 移动端跨平台

- [swapi-rust-mobile](https://github.com/xajik/rust-cross-platform-mobile)
- [Rust FFI Omnibus](http://llever.com/rust-ffi-omnibus/)
- [High performance flexbox implementation written in rust](https://github.com/vislyhq/stretch)
- [深度探索：前端中的后端](https://mp.weixin.qq.com/s/W-EvvKzmj1A8VsNKFYkuhQ)
- [深度分析：前端中的后端-实现篇](https://mp.weixin.qq.com/s/5FfSRpRG-F8Y8X3BX9mJ-Q)
- [mozilla/uniffi-rs](https://github.com/mozilla/uniffi-rs) - Rust 跨平台交互代码生成器

#### 跨平台开源项目

- [mozilla/application-services](https://github.com/mozilla/application-services) - Firefox Application Services
- [signalapp/libsignal-client](https://github.com/signalapp/libsignal-client)
- [deltachat/deltachat-core-rust](https://github.com/deltachat/deltachat-core-rust)

### 音视频领域实践

- [Ian Hobson - An introduction to Rust for audio developers](https://www.youtube.com/watch?v=Yom9E-67bdI&ab_channel=JUCE) - Rust 在音频开发的介绍与实践
  - [Sample Code](https://github.com/irh/freeverb-rs)
  - 🆕 [JUCE.com](https://juce.com/) - 音频开发工具

### 库

- [A Rust implementation of DEFLATE algorithm and related formats (ZLIB, GZIP)](https://github.com/sile/libflate)
- [FlatBuffers: Use in Rust](https://google.github.io/flatbuffers/flatbuffers_guide_use_rust.html) - Memory Efficient Serialization Library
- [Cap'n Proto for Rust](https://github.com/capnproto/capnproto-rust) - serialization/RPC system
- [maidsafe-utilities](https://github.com/maidsafe/maidsafe-utilities) - Utility functions
- [maidsafe/ffi-utils](https://github.com/maidsafe/ffi-utils)
- [getditto/safer_ffi](https://github.com/getditto/safer_ffi) - 更安全地写 FFI 代码
- [rayon-rs/rayon](https://github.com/rayon-rs/rayon) - 数据并发
- [dotenv-rs/dotenv](https://github.com/dotenv-rs/dotenv) - 通过`.env`文件配置与读取环境变量
- [BLAKE3-team/BLAKE3](https://github.com/BLAKE3-team/BLAKE3) - BLAKE3 加密哈希算法官方实现

### 教程

- [ ] 程序君的 Rust 培训课 by [@TyrChen](https://github.com/tyrchen) 🌟🌟🌟🌟🌟  ![Progress](https://img.shields.io/badge/Progress-60%25-brightgreen)
  - [讲义](https://tyrchen.github.io/rust-training/rust-training-all-in-one-cn.html) - 💡 可常温故参阅
  - [x] [上](https://www.bilibili.com/video/BV19b4y1o7Lt)
  - [ ] [下](https://www.bilibili.com/video/BV1h64y197G3)
- [x] [Rust & Tell Berlin September 2021 - Bastian Gruber: Learning Rust - One tutorial to rule them all](https://www.youtube.com/watch?v=QoatPlzc0-Y) ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
  - [OneTutorial](https://git.sr.ht/~gruberb/onetutorial/) - Rust 快速实践（🌟 新手强烈推荐）
- [x] [Take your first steps with Rust](https://docs.microsoft.com/en-us/learn/paths/rust-first-steps/) - 微软学院出品的 Rust 学习路径 ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
- [x] [Rustlings](https://github.com/rust-lang/rustlings) - 官方的命令行交互式练习 🌟🌟🌟🌟🌟 
  - [Rustlings-Track](https://github.com/Binlogo/Rustlings-Track) ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
- [ ] [Rust Stream: Ownership, Closures, and Threads - Oh My!](https://www.youtube.com/watch?v=2mwwYbBRJSo)

## Q&A 速查

- 如何加速 crates.io-index 更新速度？

执行以下命令更换镜像源

```sh
tee $HOME/.cargo/config <<-'EOF'
[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"
EOF
```

- 私有库 ssh 下载失败如何解决？

参考报错信息：

```
Caused by:
  failed to authenticate when downloading repository
attempted ssh-agent authentication, but none of the usernames `git` succeeded
attempted to find username/password via git's `credential.helper` support, but failed

Caused by:
  failed to acquire username/password from local configuration
```

解决办法：

```sh
ssh-add -K ~/.ssh/id_rsa
```

- Cargo build 时报`Blocking waiting for file lock on package cache`.

```
cargo build --verbose
    Blocking waiting for file lock on package cache
```

解决办法：

```
rm -r ~/.cargo/.package-cache
```
