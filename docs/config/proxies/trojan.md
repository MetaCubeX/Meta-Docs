# Trojan

```{.yaml linenums="1"}
proxies:
- name: "trojan"
  type: trojan
  server: server
  port: 443
  password: yourpsk
  udp: true
  alpn:
  - h2
  - http/1.1
  client-fingerprint: random
  sni: example.com
  fingerprint: xxxx
  skip-cert-verify: true
  reality-opts:
    public-key: xxx
    short-id: xxx
  network: grpc
```

[通用字段](./index.md)

[TLS 配置](./tls.md)

## password

必须，trojan 服务器密码

## network

传输层，支持 ws/grpc，不配置则为 tcp

参阅 [传输层配置](./transport.md)
