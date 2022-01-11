# Rust

> 性能、安全、抽象易用，向编译器学习（di tou🙈）。

## 工具集

- [Rustup](https://rust-lang.github.io/rustup/index.html)
  - 自动补全安装指南：`rustup help completions`
  - 版本信息：
    - [Overrides-The toolchain file: `rust-toolchain`](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file)  
    - [What Rust is it?](https://www.whatrustisit.com/)
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

## 社区&协作

> Rust 团队在工作流和团队协作方法论上也有诸多值得学习和借鉴的地方，在此记录

- [Lib.rs — home for Rust crates](https://lib.rs/) - 三方库索引，[crates.io](https://crates.io/) 的补充
- [Cross-Team Collaboration Fun Times](https://rust-ctcft.github.io/ctcft/) - Rust 「跨团队协作欢乐时光」会议
  - [协作文档](https://hackmd.io/@rust-ctcft)

## 链接

- [Book - The Rust Programming Language](https://doc.rust-lang.org/book/)
- [Rust Platform Support](https://forge.rust-lang.org/release/platform-support.html)
- [Closures: Magic Functions](https://rustyyato.github.io/rust/syntactic/sugar/2019/01/17/Closures-Magic-Functions.html)
- [Rust 编程语言](https://learnku.com/docs/rust-lang/2018) - 官方的《The Rust Programming Language》翻译
- [Rust异步浅谈](https://leaxoy.github.io/2020/03/rust-async-runtime/)
- [Rust Cookbook](https://rust-lang-nursery.github.io/rust-cookbook/) - Rust 优秀实践示例集锦
- [The Rust Performance Book](https://nnethercote.github.io/perf-book/) - Rust 性能之书
- [Rust Design Patterns](https://rust-unofficial.github.io/patterns/)
- [Rust 文档网 - Rust 官方文档中文教程](https://rustwiki.org/)

### 实践经验分享

- [x] [Why is my Rust build so slow?](https://fasterthanli.me/articles/why-is-my-rust-build-so-slow)
  - 升级 rust 版本后编译耗时劣化严重，作者的编译分析思路十分值得借鉴
  - 重点记录在 [Q&A速查](#qa-速查)：Cargo 编译耗时如何分析与优化？
- [My ideal Rust workflow](https://fasterthanli.me/articles/my-ideal-rust-workflow)
- [What is Rust and why is it so popular?](https://stackoverflow.blog/2020/01/20/what-is-rust-and-why-is-it-so-popular/)
- [Rust: A Language for the Next 40 Years - Carol Nichols](https://www.youtube.com/watch?v=A3AdN7U24iU)
- [3K, 60fps, 130ms: achieving it with Rust - by Tonarino](https://blog.tonari.no/why-we-love-rust) - [中文](https://mp.weixin.qq.com/s/-YfdohRpJxURNYLSfo6G7w)
- [Understanding Rust as a C++ developer](https://renoth.medium.com/understanding-rust-as-a-c-developer-69ee8ca76fd6)
- [x] [Building an (almost entirely) rust iOS app using uikit-sys. - Sebastian Imlay](https://simlay.net/posts/using-uikit-sys/) - 尝试几乎完全采用 Rust 构建 iOS 应用
- [x] [How we built Appflowy with Flutter and Rust](https://blog-appflowy.ghost.io/tech-design-flutter-rust/) ⭐️
- [GitHub - wooden-worm/ios-app-rs](https://github.com/wooden-worm/ios-app-rs) - 概念验证: 完全使用 Rust 写 iOS 应用
  - [objc-derive](https://github.com/wooden-worm/objc-derive)
  - [tao-foundation](https://github.com/wooden-worm/tao-foundation)
  - [tao-uikit](https://github.com/wooden-worm/tao-uikit)
- [x] [How Bevy uses Rust traits for labeling - Pascal’s Scribbles](https://deterministic.space/bevy-labels.html) - 使用 `traits` 处理标识

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

### GUI

- [rust-windowing/winit](https://github.com/rust-windowing/winit) - 跨平台窗口创建和管理
- [iced-rs/iced](https://github.com/iced-rs/iced) - Rust 跨平台 GUI 库
- [emilk/egui: egui: an easy-to-use immediate mode GUI in pure Rust](https://github.com/emilk/egui)

---

- [x] [Building a GUI app in Rust [Part A] - YouTube](https://www.youtube.com/watch?v=NtUkr_z7l84)
- [x] [Building a GUI app in Rust [Part B] - YouTube](https://www.youtube.com/watch?v=SvFPdgGwzTQ&t=0s)

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
- [DashMap](https://github.com/xacrimon/dashmap) - 支持高速并发的 HashMap
  - to be a direct replacement for `RwLock<HashMap<K, V>>`
- [parking_lot: Compact and efficient synchronization primitives for Rust.](https://github.com/Amanieu/parking_lot)

#### 错误处理

- [thiserror](https://github.com/dtolnay/thiserror) - 提供宏派生的方式简化错误定义
- [anyhow](https://github.com/dtolnay/anyhow) - 灵活处理实现了`trait Error`的错误类型转换
### 教程与参考

- [ ] [PNGme: An Intermediate Rust Project](https://picklenerd.github.io/pngme_book/) - Rust 实战练习
- [ ] [Rust Quiz - David Tolnay](https://github.com/dtolnay/rust-quiz) - 中等难度以上的小测试 ![Progress](https://img.shields.io/badge/Progress-5%25-brightgreen)
  - TODO: Rust-Quiz-Track
- [ ] [PingCAP - Talent Plan](https://github.com/pingcap/talent-plan) - PingCAP 的 Rust 学习资源
- [ ] [Ferrous Teaching Material](https://github.com/ferrous-systems/teaching-material) - Rust 教学资源，非常全面
  - [ferrous-systems/elements-of-rust](https://github.com/ferrous-systems/elements-of-rust) - Rust 风格与小技巧
- [x] 程序君的 Rust 培训课 by [@TyrChen](https://github.com/tyrchen) 🌟🌟🌟🌟🌟  ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
  - [讲义](https://tyrchen.github.io/rust-training/rust-training-all-in-one-cn.html) - 💡 可常温故参阅
  - [x] [上](https://www.bilibili.com/video/BV19b4y1o7Lt)
  - [x] [下](https://www.bilibili.com/video/BV1h64y197G3)
- [x] [Rust & Tell Berlin September 2021 - Bastian Gruber: Learning Rust - One tutorial to rule them all](https://www.youtube.com/watch?v=QoatPlzc0-Y) ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
  - [OneTutorial](https://git.sr.ht/~gruberb/onetutorial/) - Rust 快速实践（🌟 新手强烈推荐）
- [x] [Take your first steps with Rust](https://docs.microsoft.com/en-us/learn/paths/rust-first-steps/) - 微软学院出品的 Rust 学习路径 ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
- [x] [Rustlings](https://github.com/rust-lang/rustlings) - 官方的命令行交互式练习 🌟🌟🌟🌟🌟 
  - [Rustlings-Track](https://github.com/Binlogo/Rustlings-Track) ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
- [ ] [Rust Stream: Ownership, Closures, and Threads - Oh My!](https://www.youtube.com/watch?v=2mwwYbBRJSo)
- [x] [Build Your Text Editor With Rust!](https://medium.com/@otukof/build-your-text-editor-with-rust-678a463f968b) - 使用 Rust 写一个类似 vim 的命令行编辑器 ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
- [Rust 培养提高计划 | Databend](https://space.bilibili.com/275673537/channel/seriesdetail?sid=488491) ⭐️

## Q&A 速查

- 如何加速 crates.io-index 更新速度？

执行以下命令更换镜像源（或字节跳动镜像源：https://rsproxy.cn/）

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

- Cargo 编译耗时如何分析与优化？

  - `Cargo.lock`的分析：`cat Cargo.lock | toml2json | jq '.package | length'`
    - [toml2json](https://github.com/woodruffw/toml2json) - a Rust CLI utility
    - [jq](https://stedolan.github.io/jq/) - a CLI utility for JSON processor.
  - 编译产物分析：
    - `nm ./target/release/deps/libtree_sitter_highlight-dbbf005203d40df6.rlib | tail -8 | rustfilt`
    - or `llvm-nm --demangle ./target/release/deps/libtree_sitter_highlight-dbbf005203d40df6.rlib | head -8`
    - [`rustfilt`](https://crates.io/crates/rustfilt) to demangle
  - 编译单元："rust codegen unit" ([RCGU](https://github.com/rust-lang/rust/blob/3d57c61a9e04dcd3df633f41142009d6dcad4399/compiler/rustc_session/src/config.rs#L657))
  - Cargo 编译运行分析：
    - `cargo` invokes `rustc` **once per crate**
    - `rustc` decides how many "codegen units" to do in parallel, writes them as .o files, then archives them in an .rlib
    - `cargo` does one **final** rustc invocation with all the required .rlib, which ends up calling the linker to make a binary
  - Package v.s crate:
    - something that has a `Cargo.toml` is a package.
    - each package may have multiple crates: a build script crate, a lib crate, one or more bin crates, etc.
  - 编译耗时分析
    - cargo has a `-Z timings` option, **nightly-only**
    - a `cargo-timing.html` file
      - **Orange**: means we're running a build script.
      - **Light blue**: where we're still busy generating metadata, and any dependents (crates that depend on us) are blocked
  - 链接耗时分析
  - Debug symbols 分析
    - `nm` / `objdump` / `readelf` / `rustfilt`
  - 增量编译选项分析
    - 编译缓存可选方案：[sccache](https://lib.rs/crates/sccache)
  - Link-time optimization (LTO) 选项分析
  - 「杀手锏」：Rustc self-profiling
    - `-Z self-profile`
    - 可视化分析工具安装：`cargo install --git https://github.com/rust-lang/measureme crox flamegraph summarize`
    - summarize: `summarize summarize futile-1004573.mm_profdata | head -10`
    - flamegraph: `flamegraph futile-1004573.mm_profdata`
    - crox: `crox futile-1004573.mm_profdata`
      - `chrome://tracing`
    - 最终定位：Wrap 的 TraitPredicate 复杂泛型定义在 Rust 1.57.0 版本下，劣化
  - 解决办法：
    1. 升级 nightly
    2. 运行时性能适当妥协，通过封箱隐藏泛型「Warp provides `Filter::boxed`, and boxing is a great way to hide actual types.」
  - 其他尝试与分析：Splitting into more crates!
    - 编译耗时影响因素之一：LLVM IR 生成数量
    - [cargo-llvm-lines](https://github.com/dtolnay/cargo-llvm-lines)
