# MASQUE

```{.yaml linenums="1"}
proxies:
- name: "masque"
  type: masque
  server: server.com
  port: 443
  private-key: "BASE64_ENCODED_PRIVATE_KEY"
  public-key: "BASE64_ENCODED_PUBLIC_KEY"
  ip: 172.16.0.2/32
  ipv6: fd00::2/128
  mtu: 1280
  udp: true
  # remote-dns-resolve: true
  # dns:
  #   - 8.8.8.8
  #   - 1.1.1.1
```

[通用字段](./index.md)

## private-key

必须，base64 编码的 ECDSA 私钥

## public-key

必须，base64 编码的 ECDSA 公钥（服务器端公钥）

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
