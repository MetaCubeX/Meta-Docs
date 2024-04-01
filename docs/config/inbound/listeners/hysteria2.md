# Hysteria2

```{.yaml linenums="1"}
listeners:
- name: hy-in
  type: hysteria2
  port: 8443
  listen: 0.0.0.0
  users:
    user1: password1
    user2: password2
  up: 1000
  down: 1000
  ignore-client-bandwidth: false
  obfs: salamander
  obfs-password: password
  masquerade: ""
  alpn:
  - h3
  certificate: ./server.crt
  private-key: ./server.key
```

## [通用字段](./index.md)

## 协议配置

### users

Hysteria 用户以及认证密码，格式为`用户名: 密码`

!!! node ""
    用户名不参与认证,仅用于[入站规则](../../rules/index.md#in-user)匹配

### up/down

Hysteria 速率设置，默认 Mbps，具体可参考 [Hysteria 文档](https://v2.hysteria.network/zh/docs/advanced/Full-Server-Config/#_4)

### ignore-client-bandwidth

可用值：`true/false`

启用后，服务器将忽略客户端设置的任何带宽，永远使用传统的拥塞控制算法 (BBR)

### obfs

QUIC 流量混淆器，仅可设为 `salamander`,如果为空则禁用

!!! warning ""
    启用混淆将使服务器与标准的 QUIC 连接不兼容，失去 HTTP/3 伪装的能力

### obfs-password

QUIC 流量混淆器密码

### masquerade

伪装 HTTP/3 流量，仅支持`file`和`http/https`,如果为空，则始终返回 404 Not Found

具体可参考 [Hysteria 文档](https://v2.hysteria.network/zh/docs/advanced/Full-Server-Config/#masquerade)

|              | 示例                     | 描述         |
|--------------|-------------------------|--------------|
| `file`       | `file:///var/www`       | 作为文件服务器 |
| `http/https` | `http://127.0.0.1:8080` | 作为反向代理   |

### alpn

TLS 应用层协议协商列表，客户端需写入服务端 alpn 列表中的一个，否则将认证失败，默认为`h3`

### certificate/private-key

TLS 证书文件路径
