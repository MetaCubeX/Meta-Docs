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
    # tls-auth: |
    #   -----BEGIN OpenVPN Static key V1-----
    #   ...
    #   -----END OpenVPN Static key V1-----
    # key-direction: "1"
    ca: |
      -----BEGIN CERTIFICATE-----
      MIIB...example
      -----END CERTIFICATE-----
    # tls-crypt: |
    #  -----BEGIN OpenVPN Static key V1-----
    #  ...
    #  -----END OpenVPN Static key V1-----
    # peer-info:
    #   IV_HWADDR: "52:54:00:ff:72:87"
    #   UV_DEVICE_ID: "laptop-001"
    # ping: 10
    # ping-restart: 60
    # handshake-timeout: 30
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

## tls-auth

**可选**，从 `.ovpn` 文件的 `<tls-auth>` 标签中复制，**与 `tls-crypt` 互斥**

## key-direction

**可选**，使用 `tls-auth` 时填写，支持 `"1"` 或 `"0"`；如果不填或为空字符串，则默认为双向模式（`bidirectional`）。

## tls-crypt

**可选**，TLS 加密密钥。从 `.ovpn` 文件的 `<tls-crypt>` 标签中复制，不需要保留标签。

## ping

可选，默认值为`0`。

## ping-restart

可选，默认值为`0`。

## peer-info

可选，透传给服务端的 peer-info 键值对，会追加在内置 `IV_VER`/`IV_PROTO`/`IV_CIPHERS` 之后，用于服务端基于 peer-info 做准入决策。

## handshake-timeout

可选，握手超时时间，单位为秒。默认值为 `0`，表示仅使用外层连接超时。

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
