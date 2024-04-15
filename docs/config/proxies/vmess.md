# VMess

```{.yaml linenums="1"}
proxies:
- name: "vmess"
  type: vmess
  server: server
  port: 443
  udp: true
  uuid: uuid
  alterId: 0
  cipher: auto
  packet-encoding: packetaddr
  global-padding: false
  authenticated-length: false

  tls: true
  servername: example.com
  alpn:
  - h2
  - http/1.1
  fingerprint: xxxx
  client-fingerprint: chrome
  skip-cert-verify: true
  reality-opts:
    public-key: xxxx
    short-id: xxxx

  network: tcp

  smux:
    enabled: false
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## uuid

必须，VMess 用户 ID

## alterId

必须，如果不为 0，则启用旧协议

## cipher

必须，加密方法，支持 `auto`/`none`/`zero`/`aes-128-gcm`/`chacha20-poly1305`

## packet-encoding

UDP 包编码，为空则使用原始编码，可选 `packetaddr` (由 `v2ray 5+` 支持)/`xudp` (由 `xray` 支持)

## global-padding

协议参数。如果启用会随机浪费流量（在 v2ray 中默认启用并且无法禁用）。

## authenticated-length

协议参数。启用长度块加密

## network

传输层，支持 ws/http/h2/grpc，不配置或配置其他值则为 tcp

参阅 [传输层配置](./transport.md)

## smux

参阅 [sing-mux](./sing-mux.md)
