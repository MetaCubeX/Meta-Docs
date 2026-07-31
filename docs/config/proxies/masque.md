# MASQUE

```{.yaml linenums="1"}
proxies:
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true

- name: "masque-h3-l4proxy"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  udp: false
  network: h3-l4proxy

- name: "masque-h2"
  type: masque
  server: server.com
  port: 443
  private-key: BASE64_ENCODED_PRIVATE_KEY
  public-key: BASE64_ENCODED_PUBLIC_KEY
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true
  network: h2
```

## 获取 masque 配置

通过 [usque](https://github.com/Diniboy1123/usque) 工具生成 masque 配置。

[通用字段](./index.md)

## private-key

必须，base64 编码的 ECDSA 私钥

## public-key

必须，base64 编码的 ECDSA 公钥（服务器端公钥）

!!! note
    注意去掉 PEM 格式中的头尾标识符，如 `-----BEGIN PUBLIC KEY-----` ， `-----END PUBLIC KEY-----` 和 PEM 格式中的换行符 `\n`。

## ip

本地 IPv4 地址，CIDR 格式（如 172.16.0.2/32）

## ipv6

本地 IPv6 地址，CIDR 格式（如 fd00::2/128）

## mtu

TUN 设备的 MTU 大小，默认为 1280

## udp

是否启用 UDP 支持，默认为 false

## remote-dns-resolve

是否启用远程 DNS 解析，通过 MASQUE 隧道解析 DNS

## dns

远程 DNS 服务器列表，在启用 `remote-dns-resolve` 后生效

## congestion-controller

修改默认的拥塞控制算法，默认为不启用，可选值包括 `bbr`

## network

可选，默认为 `quic`。masque-h2 需要设置为 `h2`，h3-l4proxy 模式需要设置为 `h3-l4proxy`

!!! note
    `h3-l4proxy` 模式目前不支持 UDP。

## handshake-timeout

握手超时时间，单位为秒。默认值为 `0`，表示仅使用外层连接超时
