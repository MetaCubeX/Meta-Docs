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

[Common fields](./index.md)

[TLS fields](./tls.md)

## password

Required, trojan server password.

## ss-opts

### ss-opfs.enabled

Enables trojan-go Shadowsocks AEAD encryption.

### ss-opfs.method

Encryption method. Supports `aes-128-gcm`/`aes-256-gcm`/`chacha20-ietf-poly1305`.

### ss-opfs.password

trojan-go Shadowsocks AEAD encryption password.

## network

Transport layer. Supports `ws`/`grpc`. If unset or set to another value, TCP is used.

See [Transport configuration](./transport.md).
