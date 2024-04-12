# TUIC

```{.yaml linenums="1"}
proxies:
- name: tuic
  server: www.example.com
  port: 10443
  type: tuic
  token: TOKEN
  uuid: 00000000-0000-0000-0000-000000000001
  password: PASSWORD_1
  # ip: 127.0.0.1
  # heartbeat-interval: 10000
  # alpn: [h3]
  disable-sni: true
  reduce-rtt: true
  request-timeout: 8000
  udp-relay-mode: native
  # congestion-controller: bbr
  # max-udp-relay-packet-size: 1500
  # fast-open: true
  # skip-cert-verify: true
  # max-open-streams: 20
  # sni: example.com
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## token

必须，用于 TUIC V4 的用户标识，使用 TUIC V5 时不可书写

## uuid

必须，用于 TUICV5 的用户唯一识别码，使用 TUIC V4 时不可书写

## password

必须，用于 TUICV5 的用户密码，使用 TUIC V4 时不可书写

## ip

用于覆盖“server”选项中设置的服务器地址的 DNS 查找结果

## heartbeat-interval

发送保持连接活动的心跳包的间隔时间，单位为毫秒

## disable-sni

设置是否在 TLS 握手中禁用 SNI（服务器名称指示）SNI 用于在同一 IP 地址上承载多个 HTTPS 站点

## reduce-rtt

设置是否在客户端启用 QUIC 的 0-RTT 握手这可以减少连接建立时间，但可能增加重放攻击的风险

## request-timeout

设置建立到 TUIC 代理服务器的连接的超时时间，单位为毫秒

## udp-relay-mode

设置 UDP 数据包中继模式，可以是 `native`/`quic`

## congestion-controller

设置拥塞控制算法，可选项为 `cubic`/`new_reno`/`bbr`

## max-udp-relay-packet-size

设置最大的 UDP 数据包中继大小，单位为字节

## fast-open

设置是否启用 Fast Open，这可以减少连接建立时间

## max-open-streams

设置最大打开流的数量过多的打开流可能会影响性能
