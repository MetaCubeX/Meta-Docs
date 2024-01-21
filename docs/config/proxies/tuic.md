TUIC是一个轻量的基于QUIC的代理协议,由rust编写,你可以在[这里](https://github.com/EAimTY/tuic)找到更多信息

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

### token

用于 TUIC V4 的用户标识,使用TUIC V5时不可书写

### uuid

用于 TUICV5 的用户唯一识别码,使用TUIC V4时不可书写

### password

用于 TUICV5 的用户密码,使用TUIC V4时不可书写

### ip

可选字段,用于覆盖“server”选项中设置的服务器地址的DNS查找结果

### heartbeat-interval

可选字段,发送保持连接活动的心跳包的间隔时间,单位为毫秒

### alpn

可选字段,用于在TLS握手中进行应用层协议协商

### disable-sni

可选字段,设置是否在TLS握手中禁用SNI（服务器名称指示）SNI用于在同一IP地址上承载多个HTTPS站点

### reduce-rtt

可选字段,设置是否在客户端启用QUIC的0-RTT握手这可以减少连接建立时间,但可能增加重放攻击的风险

### request-timeout

可选字段,设置建立到TUIC代理服务器的连接的超时时间,单位为毫秒

### udp-relay-mode

可选字段,设置UDP数据包中继模式,可以是 `native`/`quic`

### congestion-controller

可选字段,设置拥塞控制算法,可选项为 `cubic`/`new_reno`/`bbr`

### max-udp-relay-packet-size

可选字段,设置最大的UDP数据包中继大小,单位为字节

### fast-open

可选字段,设置是否启用Fast Open,这可以减少连接建立时间

### skip-cert-verify

可选字段,设置是否跳过证书验证

### max-open-streams

可选字段,设置最大打开流的数量过多的打开流可能会影响性能

### sni

可选字段,设置在TLS握手中使用的服务器名称指示(SNI)的值
