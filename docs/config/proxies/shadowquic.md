# ShadowQUIC

```{.yaml linenums="1"}
proxies:
- name: shadowquic
  server: www.example.com
  port: 10443
  type: shadowquic
  username: username
  password: password
  # sni: example.com
  # alpn: [h3]
  # quic-versions: [v1] # 支持 v1/v2，默认 v1
  # udp-over-stream: false # UDP over stream，默认 false
  # zero-rtt: false
  # keep-alive-interval: 10000
  # congestion-controller: cubic
  # up: 100 Mbps 
  # down: 100 Mbps
  # cwnd: 32
  # bbr-profile: "standard"
  # max-datagram-frame-size: 1400
  # max-open-streams: 1024
  # recv-window-conn: 0
  # recv-window: 0
  # disable-mtu-discovery: false

```

[通用字段](./index.md)

[TLS 字段](./tls.md)

!!! tip
    ShadowQUIC 强制启用 JLS，无需单独配置 [`jls-opts`](./tls.md#jls-opts)。

## username

必须，用于 ShadowQUIC 的认证用户名

## password

必须，用于 ShadowQUIC 的认证密码

## sni

可选，ShadowQUIC TLS 握手使用的 SNI

## alpn

可选，应用层协议协商列表，默认使用 `h3`

## quic-versions

可选，设置支持的 QUIC 版本，可选值为 `v1`/`v2`，默认为 `v1`

## udp-over-stream

可选，设置是否通过数据流传输 UDP（UDP over stream），默认为 `false`

## zero-rtt

可选，设置是否开启 0-RTT（零往返时延握手）。

!!! warning
    官方 server 的 0-RTT 路径可能在 JLS 认证完成前读取用户状态（已确认影响 v0.3.11）；使用官方 server 如果遇到不通或断联情况，请在服务端关闭 zero-rtt

## keep-alive-interval

可选，发送保持连接活动的心跳包的间隔时间，单位为毫秒

## congestion-controller

可选，设置拥塞控制算法，可选值为 `cubic`/`new_reno`/`bbr`，默认为 `cubic`

## up

可选，设置客户端上传速度。填写后将通过 mihomo 私有扩展协商 Brutal；对端不支持时回退到 `congestion-controller`

## down

可选，设置客户端请求的下载速度，受 Listener 的 `up` 限制。填写后将通过 mihomo 私有扩展协商 Brutal；对端不支持时回退到 `congestion-controller`

## cwnd

可选，设置初始拥塞窗口大小（Initial Congestion Window size），默认为 `32`

## bbr-profile

可选，设置 BBR 算法的激进程度配置，可选值为 `standard`/`conservative`/`aggressive`，仅在拥塞控制算法为 `bbr` 时有效

## max-datagram-frame-size

可选，设置允许的最大数据报帧大小，单位为字节，默认为 `1400`

## max-open-streams

可选，设置最大同时打开的并发流数量，默认为 `1024`

## recv-window-conn

可选，设置单个流级别的接收窗口大小，如果设为 `0` 则使用内部默认值

## recv-window

可选，设置连接级别的接收窗口大小，如果设为 `0` 则使用内部默认值

## disable-mtu-discovery

可选，设置是否禁用路径 MTU 发现（Path MTU Discovery），若设为 `true` 则使用固定的 MTU 大小
