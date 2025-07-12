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
  client-fingerprint: chrome
  skip-cert-verify: true
  reality-opts:
    public-key: xxxx
    short-id: xxxx

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

## client-fingerprint
客户端发送请求的指纹，用于标识自身设备 `chrome/firefox/safari/edge/android/ios/other`

## skip-cert-verify

是否跳过SSL证书验证 `true/false`

## reality-opts

如果使用 reality，则此项必须配置

*reality-opts.public-key*
> 必须，reality 私钥

*reality-opts.short-id*
> 必须，reality short id （这是由于mihomo vless 服务端也必须配置 short id 才能正常工作导致的，如果通过其它工具搭建服务端，则可按需配置此项）

## network

传输层，支持 ws/http/h2/grpc，不配置或配置其他值则为 tcp
参阅 [传输层配置](./transport.md)