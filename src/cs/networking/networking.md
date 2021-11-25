# 网络

讲述一个 `1 + 1 > 2` 的简单故事

## 工具集

- [Google Public DNS](https://dns.google/) - Google 的 DNS 查询服务
  - [DNS-over-HTTPS (DoH)](https://developers.google.com/speed/public-dns/docs/doh)
- [Cloudflare-Encrypt DNS traffic](https://developers.cloudflare.com/1.1.1.1/encrypted-dns) - Cloudflare 提供的 DNS 查询服务
## 备忘

### 常用特殊端口

| 服务、协议或应用                     | 端口号 | TCP or UDP |
| ------------------------------------ | ------ | ---------- |
| FTP (File Transfer Protocol)         | 20，21 | TCP        |
| SSH (Secure Shell Protocol)          | 22     | TCP        |
| Telnet                               | 23     | TCP        |
| SMTP (Simple Mail Transfer Protocol) | 25     | TCP        |
| DNS (Domain Name System)             | 53     | UDP        |
| HTTP                                 | 80     | TCP        |
| POP3                                 | 110    | TCP        |
| IMAP4                                | 143    | TCP        |
| HTTPS                                | 443    | TCP        |

## 链接

- [NATS.io 消息系统协议](https://docs.nats.io/)
  - [自己上手学习 Rust 的实现](https://github.com/Binlogo/nats-rs)
- 边缘网络系列文章
  - [x] [从网卡到应用以及Rust网络库实现](https://mp.weixin.qq.com/s/uIASAeayB7noOrBOzBc3_g)

### 网络协议与组织

- [The IETF HTTP Working Group](https://httpwg.org/) - HTTP 工作组
- [The IETF Datatracker](https://datatracker.ietf.org/) - RFC 文档查询
