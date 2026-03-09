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
  # quic: true
  # congestion-controller: bbr
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## quic

是否开启QUIC协议，默认为false。

## congestion-controller

设置QUIC拥塞控制算法。
