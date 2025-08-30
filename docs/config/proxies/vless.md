# VLESS

```{.yaml linenums="1"}
proxies:
- name: "vless"
  type: vless
  server: server
  port: 443
  udp: true
  uuid: uuid
  flow: xtls-rprx-vision
  packet-encoding: xudp

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
  encryption: ""

  network: tcp

  smux:
    enabled: false
```

!!! note
    Meta 的 `xtls-*` 流控实际上与 Xray-core 中的 `xtls-*-udp443` 等效，如需拦截 443 端口的 UDP 流量，请使用逻辑规则：`AND,((NETWORK,UDP),(DST-PORT,443)),REJECT`

[通用字段](./index.md)

[TLS 字段](./tls.md)

## uuid

必须，VLESS 用户 ID

## flow

VLESS 子协议，可用值为 `xtls-rprx-vision`

## packet-encoding

UDP 包编码，为空则使用原始编码，可选 `packetaddr` (由 `v2ray 5+` 支持)/ `xudp` (由 `xray` 支持)

## encryption

vless encryption客户端配置：

`encryption: "mlkem768x25519plus.native/xorpub/random.1rtt/0rtt.(X25519 Password).(ML-KEM-768 Client)..."`

（native/xorpub 的 XTLS Vision 可以 Splice。只使用 1-RTT 模式 / 若服务端发的 ticket 中秒数不为零则 0-RTT 复用）

 / 是只能选一个，后面 base64 至少一个，无限串联，使用  `mihomo generate vless-x25519` 和 `mihomo generate vless-mlkem768` 生成，替换值时需去掉括号

## network

传输层，支持 ws/http/h2/grpc，不配置或配置其他值则为 tcp

参阅 [传输层配置](./transport.md)

