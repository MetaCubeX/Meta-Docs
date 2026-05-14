# AnyTLS

```{.yaml linenums="1"}
proxies:
- name: anytls
  type: anytls
  server: 1.2.3.4
  port: 443
  password: "<your password>"
  client-fingerprint: chrome
  udp: true
  idle-session-check-interval: 30
  idle-session-timeout: 30
  min-idle-session: 0
  sni: "example.com"
  alpn:
    - h2
    - http/1.1
  skip-cert-verify: true
```

[Common fields](./index.md)

[TLS fields](./tls.md)

!!! tip
    Mihomo does not support AnyTLS+Reality, and will not support this combination in the future. If you want to hide SNI, use [ECH](./tls.md#ech-opts). If you must use Reality, choose the [VMess](./vmess.md), [VLESS](./vless.md), or [Trojan](./trojan.md) protocol.

## idle-session-check-interval

Interval for checking idle sessions. Default: 30 seconds.

## idle-session-timeout

During checks, sessions idle longer than this value are closed. Default: 30 seconds.

## min-idle-session

During checks, keep at least the first n idle sessions open. Default: n=0.
