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

[通用字段](./index.md)

[TLS 字段](./tls.md)

## idle-session-check-interval

检查空闲会话的时间间隔。默认值：30 秒。

## idle-session-timeout

在检查中，关闭闲置时间超过此值的会话。默认值：30 秒。

## min-idle-session

在检查中，至少前 n 个空闲会话保持打开状态。默认值：n=0