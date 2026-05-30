# OpenVPN

```{.yaml linenums="1"}
proxies:
  - name: "openvpn"
    type: openvpn
    server: vpn.example.com
    port: 1194
    proto: udp
    # auth-user-pass 模式
    username: "user"
    password: "pass"
    # 证书模式 (与上方二选一)
    # cert: |
    #   -----BEGIN CERTIFICATE-----
    #   ...
    #   -----END CERTIFICATE-----
    # key: |
    #   -----BEGIN PRIVATE KEY-----
    #   ...
    #   -----END PRIVATE KEY-----
    ca: |
      -----BEGIN CERTIFICATE-----
      MIIB...example
      -----END CERTIFICATE-----
    # tls-crypt: |
    #  -----BEGIN OpenVPN Static key V1-----
    #  ...
    #  -----END OpenVPN Static key V1-----
    # dev: tun
    # cipher: AES-128-GCM
    # auth: SHA256
    # comp-lzo: "no"
    udp: true
    # mtu: 1500
    # dialer-proxy: "ss1"
    # remote-dns-resolve: true
    # dns: [ 1.1.1.1, 8.8.8.8 ]

```

[通用字段](./index.md)

## server

**必须**，OpenVPN 服务器地址。

## port

**必须**，OpenVPN 服务器端口，默认 `1194`。

## proto

可选，协议类型，支持 `udp` 或 `tcp`，默认 `udp`。

## username / password

**可选**，`auth-user-pass` 认证模式所需的用户名与密码。

> **注意**：需与 `cert` / `key` 二选一，两者不能同时为空。

## ca

**必须**，CA 证书内容。请从 `.ovpn` 文件的 `<ca>` 标签中复制。

## cert

**可选**，客户端证书内容。从 `.ovpn` 文件的 `<cert>` 标签中复制。使用用户名/密码认证时可省略。

## key

**可选**，客户端私钥内容。从 `.ovpn` 文件的 `<key>` 标签中复制。使用用户名/密码认证时可省略。

## tls-crypt

**可选**，TLS 加密密钥。从 `.ovpn` 文件的 `<tls-crypt>` 标签中复制，不需要保留标签。

## dev

可选，虚拟网卡类型，当前仅支持 `tun`，默认 `tun`。

## cipher

可选，加密方式，支持 `AES-128-GCM` / `AES-256-GCM`/ `AES-128-CBC` / `AES-256-CBC` /`CHACHA20-POLY1305`默认 `AES-128-GCM`， `AES-CBC` 会按 `AES-128-CBC` 处理。

## auth

可选，数据验证算法，支持 `MD5` /`SHA1` / `SHA256`/ `SHA384` / `SHA512`，默认 `SHA256`；AEAD cipher 会忽略 auth。

## comp-lzo

可选，数据压缩方式。可选值：`yes`、`no`、`adaptive`。

## udp

可选，是否启用 UDP。`true` 表示使用 UDP，`false` 表示使用 TCP。

## mtu

可选，最大传输单元，默认 `1500`。

## dialer-proxy

可选，指定出站代理的标识。若设置，OpenVPN 连接将通过该代理发出。

## remote-dns-resolve

可选，是否强制 DNS 远程解析，默认 `false`。

## dns

可选，仅在 `remote-dns-resolve` 为 `true` 时生效，指定远程解析的 DNS 地址列表。
