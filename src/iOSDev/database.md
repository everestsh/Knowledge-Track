# 数据库

让移动应用更强·大。

## 链接

- [GRDB.swift](https://github.com/groue/GRDB.swift)
    - [Why Adopt GRDB?](https://github.com/groue/GRDB.swift/blob/master/Documentation/WhyAdoptGRDB.md)
        <details>
            <summary>笔记概览</summary>

        - 核心原则
            - 通过「Record Types」更便捷地访问数据库（ORM）
            - 在必要时能够支持原始 SQL 查询
            - 数据库文件是唯一的真实数据源
            - 基于 SQLite，针对 GUI 客户端应用开发
        - 解决的问题
            - 支持任意 Swift struct/class 作为数据库记录（Record）
            - 允许数据库记录跨线程
            - 用数据库更改通知替换自动更新记录（Fetched records behave just like an in-memory cache of the database content. ）
            - 非阻塞数据库读取
            - 强大清晰的多线程保证
                - 并发写入
                - 故障隔离
                - 冲突处理
                - 阻塞 UI
            - 可以完全不和原始 SQL 直接交互，但随时可以

        </details>