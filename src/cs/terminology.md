# 术语汇总

记不过来的那些术语概念💥


- [有向无环图(Directed acyclic graph)](https://zh.wikipedia.org/wiki/%E6%9C%89%E5%90%91%E6%97%A0%E7%8E%AF%E5%9B%BE)

    > 在图论中，如果一个有向图从任意顶点出发无法经过若干条边回到该点，则这个图是一个有向无环图（DAG,directed acyclic graph）

    - 引用来源：[Square/Cleanse](https://github.com/square/Cleanse#using-cleanse)

- [CRDT - Conflict-free Replicated Data Types](https://arxiv.org/pdf/1805.06358.pdf)
    
    > 无冲突复制数据类型
    
    - 引用来源：[Xi-Editor 设计说明文档](https://xi-editor.io/docs/crdt.html)

- [消息中间件 - aka: 消息队列(Message queue)](https://zh.wikipedia.org/wiki/%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97)

    > 一种进程间通信或同一进程的不同线程间的通信方式，提供了异步的通信协议
    
    > 为什么要用消息中间件？
    >   1. 系统解耦
    >   2. 异步调用
    >   3. 流量削峰

    - 引用来源：[采用 Rust 实现 NATS](https://github.com/Binlogo/nats-rs)
