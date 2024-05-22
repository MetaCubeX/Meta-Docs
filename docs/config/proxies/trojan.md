# Trojan

```{.yaml linenums="1"}
proxies:
- name: "trojan"
  type: trojan
  server: server
  port: 443
  password: yourpsk
  udp: true

  sni: example.com
  alpn:
  - h2
  - http/1.1
  client-fingerprint: random
  fingerprint: xxxx
  skip-cert-verify: true
  ss-opts:
    enabled: false
    method: aes-128-gcm
    password: "example"
  reality-opts:
    public-key: xxxx
    short-id: xxxx

  network: tcp

  smux:
    enabled: false
```

[通用字段](./index.md)

[TLS 字段](./tls.md)

## password

必须，trojan 服务器密码

## ss-opts

### ss-opfs.enabled

启用 trojan-go 的 shadowsocks AEAD 加密

### ss-opfs.method

加密方法，支持 aes-128-gcm/aes-256-gcm/chacha20-ietf-poly1305

### ss-opfs.password

trojan-go 的 shadowsocks AEAD 加密密码

## network

传输层，支持 ws/grpc，不配置或配置其他值则为 tcp

参阅 [传输层配置](./transport.md)

## smux

参阅 [sing-mux](./sing-mux.md)
