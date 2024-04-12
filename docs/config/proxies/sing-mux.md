# sing-mux

```{.yaml linenums="1"}
smux:
  enabled: true
  protocol: smux
  max-connections: 4
  min-streams: 4
  max-streams: 0
  statistic: false
  only-tcp: false
  padding: true
  brutal-opts:
    enabled: true
    up: 50
    down: 100
```

## enabled

是否启用多路复用

## protocol

多路复用协议，支持如下协议，默认使用 `h2mux`

| 协议     | 描述                                  |
|---------|--------------------------------------|
| `smux`  | <https://github.com/xtaci/smux>      |
| `yamux` | <https://github.com/hashicorp/yamux> |
| `h2mux` | <https://golang.org/x/net/http2>     |

## max-connections

最大连接数量

与 `max-streams` 冲突

## min-streams

在打开新连接之前，连接中的最小多路复用流数量

与 `max-streams` 冲突

## max-streams

在打开新连接之前，连接中的最大多路复用流数量

与 `max-connections` 和 `min-streams` 冲突

## statistic

控制是否将底层连接显示在面板中，方便打断底层连接

## only-tcp

仅允许 tcp，如果设置为 true,smux 的设置将不会对 udp 生效，udp 连接会直接走节点默认 udp 协议传输

## padding

启用填充

## brutal-opts

TCP Brutal 设置

### brutal-opts.enabled

启用 TCP Brutal 拥塞控制算法

### brutal-opts.up/down

上传和下载带宽，以默认以 Mbps 为单位
