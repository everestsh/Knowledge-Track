# TCP 网络协议

> 雕刻互联网基石

## 备忘

- `TCP_NODELAY`选项：
    - 是否禁用 Nagle 算法，开启意味着禁用
    - 对于延时敏感型，同时数据传输量比较小的应用，开启 TCP_NODELAY 选项
    - 参考：[TCP连接中启用和禁用TCP_NODELAY有什么影响？](https://www.zhihu.com/question/42308970)
    - 遇到过的问题：
        - 客户端发送「短」消息，一直等待发不出去，多发几条才陆续成功

## 链接

- [RFC 793: Transmission Control Protocol](https://datatracker.ietf.org/doc/rfc793/)
