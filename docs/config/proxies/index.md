# 通用字段

```{.yaml linenums="1"}
proxies:
- name: "ss"
  type: ss
  server: server
  port: 443
  ip-version: ipv4
  udp: true
  interface-name: eth0
  routing-mark: 1234
  tfo: false
  mptcp: false

  dialer-proxy: ss1

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

代理节点，内容为数组

## name

必须，代理名称，不可重复

## type

必须，代理节点类型

## server

必须，代理节点服务器 (域名/ip)

## port

必须项，代理节点端口

## ip-version

代理软件出站使用的 ip 版本，如果不是 direct，则会影响 server 为域名时使用的 ip 地址

可选：`dual`/`ipv4`/`ipv6`/`ipv4-prefer`/`ipv6-prefer` ,默认使用 `dual`

* ipv4: 仅使用 IPv4
* ipv6: 仅使用 IPv6
* ipv4-prefer: 优先使用 IPv4，对于 TCP 会进行双栈解析，并发链接但是优先使用 IPv4 链接，UDP 则为双栈解析，获取结果中的第一个 IPv4
* ipv6-prefer:优先使用 IPv6，对于 TCP 会进行双栈解析，并发链接但是优先使用 IPv6 链接，UDP 则为双栈解析，获取结果中的第一个 IPv6

## udp

是否允许 UDP 通过代理，默认为 `false`

!!! note
    此选项在 `TUIC` 等基于 `UDP` 的协议以及 `direct` 和 `dns` 类型中默认开启

## interface-name

指定节点绑定的接口，从此接口发起连接

## routing-mark

节点发起连接时附加的路由标记

## tfo

启用 `TCP Fast Open`, 仅生效于 `TCP` 协议

## mptcp

启用 `TCP Multi Path`, 仅生效于 `TCP` 协议

## dialer-proxy

指定当前 `proxies` 通过 `dialer-proxy` 建立网络连接，值可以为[策略组](../proxy-groups/index.md)/[出站代理](../proxies/index.md)的 `name`，用法可参考[dialer-proxy文档](../proxies/dialer-proxy.md)

## smux

sing-mux，仅限使用 tcp 传输的协议

### smux.enabled

是否启用多路复用

### smux.protocol

多路复用协议，支持如下协议，默认使用 `h2mux`

| 协议     | 描述                                  |
|---------|--------------------------------------|
| `smux`  | <https://github.com/xtaci/smux>      |
| `yamux` | <https://github.com/hashicorp/yamux> |
| `h2mux` | <https://golang.org/x/net/http2>     |

### smux.max-connections

最大连接数量

与 `max-streams` 冲突

### smux.min-streams

在打开新连接之前，连接中的最小多路复用流数量

与 `max-streams` 冲突

### smux.max-streams

在打开新连接之前，连接中的最大多路复用流数量

与 `max-connections` 和 `min-streams` 冲突

### smux.statistic

控制是否将底层连接显示在面板中，方便打断底层连接

### smux.only-tcp

仅允许 tcp，如果设置为 true,smux 的设置将不会对 udp 生效，udp 连接会直接走节点默认 udp 协议传输

### smux.padding

启用填充

### smux.brutal-opts

TCP Brutal 设置

#### smux.brutal-opts.enabled

启用 TCP Brutal 拥塞控制算法

#### smux.brutal-opts.up/down

上传和下载带宽，以默认以 Mbps 为单位
