# VMess

```{.yaml linenums="1"}
proxies:
- name: "vmess"
  type: vmess
  server: server
  port: 443
  udp: true
  uuid: uuid
  alterId: 0
  cipher: auto
  packet-encoding: packetaddr
  global-padding: false
  authenticated-length: false

  tls: true
  servername: example.com
  alpn:
  - h2
  - http/1.1
  fingerprint: xxxx
  client-fingerprint: chrome
  skip-cert-verify: true
  # name-cert-verify: example.com
  reality-opts:
    public-key: xxxx
    short-id: xxxx
  tlsmirror-opts:
    primary-key: xxxx

  network: tcp

  smux:
    enabled: false
```

[Common fields](./index.md)

[TLS fields](./tls.md)

## uuid

Required, VMess user ID.

## alterId

Required. If not `0`, the legacy protocol is enabled.

## cipher

Required, encryption method. Supports `auto`/`none`/`zero`/`aes-128-gcm`/`chacha20-poly1305`.

## packet-encoding

UDP packet encoding. Empty means raw encoding. Available values: `packetaddr` (supported by `v2ray 5+`)/`xudp` (supported by `xray`).

## global-padding

Protocol parameter. If enabled, traffic is randomly wasted. It is enabled by default in v2ray and cannot be disabled there.

## authenticated-length

Protocol parameter. Enables encrypted length blocks.

## name-cert-verifyOptional.

Only modifies the certificate's DNSName verification target, without altering the SNI.

## network

Transport layer. Supports `ws`/`http`/`h2`/`grpc`/`mkcp`/`mekya`. If unset or set to another value, TCP is used.

See [Transport configuration](./transport.md).
