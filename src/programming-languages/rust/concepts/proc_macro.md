# 过程宏

Meta programming.

## 链接

- [x] [Rust 过程宏（第一弹）](https://www.bilibili.com/video/BV1Za411q7LQ) ![Progress](https://img.shields.io/badge/Progress-100%25-brightgreen)
    - [JSON Schema](https://json-schema.org/) - 通过 JSON 描述数据结构/文档生成/代码生成等
    - [djc/askama](https://github.com/djc/askama) - Jinja 模板渲染引擎
        - [Jinja](https://jinja.palletsprojects.com/) - 类似 Python 语法的一套模板引擎
    - [withoutboats/heck](https://github.com/withoutboats/heck) - 命名风格转换
    - [LukasKalbertodt/litrs](https://github.com/LukasKalbertodt/litrs) - 解析&检查 Rust 字面量
- [ ] [Rust 过程宏（第二弹）](https://www.bilibili.com/video/BV1Fu411m7W7?share_source=copy_web) ![Progress](https://img.shields.io/badge/Progress-75%25-brightgreen)
    - [syn](https://github.com/dtolnay/syn) - Parser for Rust source code
    - [quote](https://github.com/dtolnay/quote) - 操作 Rust 语法树，便捷生成 `TokenStream`
    - `if let` 获取内部嵌套数据

- [ ] [dtolnay/proc-macro-workshop](https://github.com/dtolnay/proc-macro-workshop) ![Progress](https://img.shields.io/badge/Progress-15%25-brightgreen)
    - [x] Derive macro: `derive(Builder)`
    - [ ] Derive macro: `derive(CustomDebug)`
    - [ ] Function-like macro: `seq!`
    - [ ] Attribute macro: `#[sorted]`
    - [ ] Attribute macro: `#[bitfield]`