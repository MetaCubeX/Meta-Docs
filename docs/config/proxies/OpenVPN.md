# OpenVPN

```{.yaml linenums="1"}
proxies

- name: "openvpn"
  type: openvpn
  server: vpn.example.com
  port: 1194
  proto: udp      
  # dev: tun      
  # cipher: AES-128-GCM 
  # auth: SHA256  
  ca: |
    -----BEGIN CERTIFICATE-----
    MIIB...example
    -----END CERTIFICATE-----
  cert: |
    -----BEGIN CERTIFICATE-----
    MIIB...example
    -----END CERTIFICATE-----
  key: |
    -----BEGIN PRIVATE KEY-----
    MIIE...example
    -----END PRIVATE KEY-----
  tls-crypt: |
    -----BEGIN OpenVPN Static key V1-----
    00000000000000000000000000000000
    ...
    -----END OpenVPN Static key V1-----
  udp: true
  # mtu: 1500
  # dialer-proxy: "ss1"
```

[通用字段](./index.md)

## server

必须，OpenVPN 服务器地址。

## port

必须，OpenVPN 服务器端口，默认 1194。

## proto

可选，协议类型，支持 udp 或 tcp，默认 udp。

## dev

可选，OpenVPN 虚拟网卡类型，当前仅支持 tun，默认 tun。

## cipher

可选，加密方式，目前仅支持 AES-128-GCM，默认 AES-128-GCM。

## auth

可选，认证方式，目前仅支持 SHA256，默认 SHA256。

## ca

必须，CA 证书内容，从 .ovpn 文件的 `<ca>` 标签中复制，不需要保留 `<ca>` 标签。

## cert

必须，客户端证书内容，从 .ovpn 文件的 `<cert>` 标签中复制，不需要保留 `<cert>` 标签。

## key

必须，客户端私钥内容，从 .ovpn 文件的 `<key>` 标签中复制，不需要保留 `<key>` 标签。

## tls-crypt

可选，TLS 加密密钥，从 .ovpn 文件的 `<tls-crypt>` 标签中复制，不需要保留 `<tls-crypt>` 标签。

## udp

可选，是否使用 UDP 协议，true 表示 UDP，false 表示 TCP。

## mtu

可选，最大传输单元，默认 1500。

## dialer-proxy

可选，用于指定出站代理的标识，如果不为空，则 OpenVPN 连接通过该代理发出。
