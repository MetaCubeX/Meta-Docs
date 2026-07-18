# ShadowQUIC

```{.yaml linenums="1"}
listeners:
- name: shadowquic-in-1
  type: shadowquic
  port: 10822
  listen: 0.0.0.0
  # routing-mark: 0 # 为监听 socket 设置 routing-mark（仅支持 Linux）
  # rule: sub-rule-name1
  # proxy: proxy
  users:
    - username: username
      password: password
  jls-upstream:
    addr: www.example.com:443
    # sni: example.com
    # proxy: proxy
    # rate-limit: 0
    # quic-version-probe: false
  # alpn:
  #   - h3
  # quic-versions: [v1]
  # zero-rtt: true
  # congestion-controller: bbr
  # up: 100 Mbps
  # down: 100 Mbps
  # ignore-client-bandwidth: false
  # cwnd: 10
  # bbr-profile: ""
  # max-idle-time: 30000
  # max-datagram-frame-size: 1400
  # recv-window-conn: 0
  # recv-window: 0
  # disable-mtu-discovery: false
```

[通用字段](./index.md)

## users

ShadowQUIC 认证用户列表。

### users.username

用户名。

### users.password

密码。

## jls-upstream

未通过 JLS 认证的连接会转发到该 QUIC 伪装上游。

### jls-upstream.addr

伪装上游地址。

### jls-upstream.sni

JLS 伪装目标 SNI，留空时从 `addr` 推导。

### jls-upstream.proxy

伪装上游使用的代理。

### jls-upstream.rate-limit

转发限速，单位 bit/s，`0` 表示不限速。

### jls-upstream.quic-version-probe

在首次连接时探测伪装上游版本。

## alpn

应用层协议协商列表。

## quic-versions

本机手动配置及探测失败时的版本，支持 `v1` / `v2`。

## zero-rtt

是否启用 0-RTT。

## congestion-controller

拥塞控制算法，可选值为 `cubic` / `new_reno` / `bbr`，默认为 `cubic`。

## up/down

Brutal 带宽协商配置。`up` 为服务端上传 / 客户端下载速度上限，`down` 为服务端下载 / 客户端上传请求速度。

## ignore-client-bandwidth

Brutal 使用，忽略出站 `down` 并使用 listener 的 `down` 或自动配置。

## cwnd

初始拥塞窗口大小，默认 `32`。

## bbr-profile

BBR 激进程度，可选值为 `standard` / `conservative` / `aggressive`，默认为 `standard`。

## max-idle-time

最大空闲时间，单位毫秒。

## max-datagram-frame-size

最大 UDP datagram frame 大小。

## recv-window-conn

连接级接收窗口大小，`0` 表示使用默认值。

## recv-window

流级接收窗口大小，`0` 表示使用默认值。

## disable-mtu-discovery

是否禁用路径 MTU 发现。
