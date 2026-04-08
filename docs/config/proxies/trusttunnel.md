# TrustTunnel

```{.yaml linenums="1"}
proxies:
- name: trusttunnel
  type: trusttunnel
  server: 1.2.3.4
  port: 443
  username: username
  password: password
  # client-fingerprint: chrome
  health-check: true
  udp: true
  # sni: "example.com"
  # alpn:
  #   - h2
  # skip-cert-verify: true
  ### quic options
  # quic: true
  # congestion-controller: bbr
  ### reuse options
  # max-connections: 1
  # min-streams: 0
  # max-streams: 0
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## quic

是否开启QUIC协议，默认为false。

## congestion-controller

设置QUIC拥塞控制算法。

### max-connections

最大连接数量。默认值为1即只使用一条底层链接

与 `max-streams` 冲突

### min-streams

在打开新连接之前，连接中的最小多路复用流数量

与 `max-streams` 冲突

### max-streams

在打开新连接之前，连接中的最大多路复用流数量

与 `max-connections` 和 `min-streams` 冲突
