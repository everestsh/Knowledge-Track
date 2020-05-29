# Rust

> 为了安全，向编译器学习（di tou）。

- [化解神奇的闭包](./closure.md)

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

## 链接

- [Rust Platform Support](https://forge.rust-lang.org/release/platform-support.html)
- [Closures: Magic Functions](https://rustyyato.github.io/rust/syntactic/sugar/2019/01/17/Closures-Magic-Functions.html)

### Rust 移动端跨平台

- [swapi-rust-mobile](https://github.com/xajik/rust-cross-platform-mobile)
- [Rust FFI Omnibus](http://llever.com/rust-ffi-omnibus/)
- [High performance flexbox implementation written in rust](https://github.com/vislyhq/stretch)

### 库

- [A Rust implementation of DEFLATE algorithm and related formats (ZLIB, GZIP)](https://github.com/sile/libflate)
- [FlatBuffers: Use in Rust](https://google.github.io/flatbuffers/flatbuffers_guide_use_rust.html) - Memory Efficient Serialization Library
- [Cap'n Proto for Rust](https://github.com/capnproto/capnproto-rust) - serialization/RPC system